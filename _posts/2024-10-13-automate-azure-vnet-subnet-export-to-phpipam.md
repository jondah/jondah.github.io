---
title: "Automate Azure VNET Subnet Export to Phpipam"
date: "2024-10-13"
categories: 
  - "azure"
  - "python"
tags: 
  - "phpipam"
redirect_from:
- 2024/10/13/automate-azure-vnet-subnet-export-to-phpipam
---

I wanted to explore how to export subnets in Azure to Phpipam. Phpipam is used to document all ip allocation but only on VNET level. All subnets that are created inside VNETs needs to be allocated by some kind of automation.  

![](/wp-content/uploads/2024/10/azure-vnet.png?w=1024)

This is the setup in Azure. A VNET with several subnets.

![](/wp-content/uploads/2024/10/2024-10-11-20_19_11-phpipam-_-subnets-_-azure.png?w=1024)

In phpipam only the VNET address space is documented. I want the subnets to have 10.0.0.0/20 as a parent subnet inside phpipam. Just like the structure in Azure. To achieve this i decided to write a solution in python. I have splitted the code in to two files. azureadd.py contains the code used for reading VNET and subnet information from azure and call functions in ipam.py that handle the interaction with phpipam.

azureadd.py

```python
from azure.identity import DefaultAzureCredential
from azure.mgmt.network import NetworkManagementClient
from ipam import *

# Set up your subscription ID
subscription_id = 'subnetID'

# Authenticate using DefaultAzureCredential (supports various methods, like Service Principal, Managed Identity, etc.)
credential = DefaultAzureCredential()

# Initialize the Network Management Client
network_client = NetworkManagementClient(credential, subscription_id)

# Define the resource group and VNet.
resource_group_name = 'Network-RG'
vnet_name = 'example-vnet'

# Get the specific VNet
def add_vnet_details():
    try:
        vnet = network_client.virtual_networks.get(resource_group_name, vnet_name)
        print(f"VNet Address Space: {vnet.address_space.address_prefixes}")

        for subnet in vnet.subnets:
            subnet = dict(
                name = subnet.name,
                cidr = subnet.address_prefix,
                vnet = vnet.address_space.address_prefixes
            )
            add_subnet(subnet)
            

    except Exception as e:
        print(f"Error getting VNet details: {e}")

# Call the functions
add_vnet_details()  # Should be parameterised in future


```

ipam.py

```python
import phpypam
from phpypam.core.exceptions import PHPyPAMException  # Make sure to import the exception

pi = phpypam.api(
  url='http://192.168.1.3/phpipam',
  app_id='azure',
  username='admin',
  password='admin123',
  ssl_verify=False
)

def get_parentid(parentcidr):
    parentcidr = ','.join(parentcidr)
    entity = pi.get_entity(controller='subnets', controller_path='cidr/' + parentcidr + '/')

    parents = dict(
        subnetid =entity[0]['id'],
        sectionid = entity[0]['sectionId']
    )
    return parents

def add_subnet(subnet):
    addressplit = subnet['cidr'].split("/")
    parrents = get_parentid(subnet['vnet'])
    new_subnet = dict(
        description=subnet['name'], #Subnet name
        subnet=addressplit[0], #Subnet address
        mask=addressplit[1], # Subnet mask
        masterSubnetId=parrents['subnetid'], # Set this to the parent subnet's ID
        sectionId=parrents['sectionid'] #Ensure sectionId is correctly assigned
    )
    print(f"New subnet: {new_subnet}")
    # Try to create the new child subnet
    try:
        print('Creating new subnet (as child)...')
        created_subnet = pi.create_entity(controller='subnets', data=new_subnet)
        print(f"Child subnet created: {created_subnet}")
    except PHPyPAMException as e:
        print(f"Failed to create subnet might already exist. Error code: {e._code}, message: {e._message}")


```

Let's run the code.  
![](/assets/img/run.png)

Now we can see the end result in phpipam.

![](/assets/img/ipam_done.png)

In the future i would like to extend the code to also allocate IPs that is tied to a static Azure NIC. For larger environments with multiple subscriptions and VNETS it would be better to read the subscriptions and VNETs from Azure without a static entry in the code.
