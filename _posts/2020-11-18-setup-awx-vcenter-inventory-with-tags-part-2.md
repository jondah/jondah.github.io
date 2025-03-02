---
title: "Setup AWX Vcenter inventory with tags part 2"
date: "2020-11-18"
categories: 
  - "ansible"
  - "vmware"
tags: 
  - "awx"
coverImage: "2020-11-17-21_35_53-ansible-awx-_-all-groups.png"
redirect_from:
  - 2020/11/18/setup-awx-vcenter-inventory-with-tags-part-2/
---

This is the second part in how you can setup Vmware inventory in AWX based on VM tags. [Link to first part](/posts/setup-awx-vcenter-inventory-with-tags-part-1/).

Create a file that ends with vmware.yml or vmware.yaml. For example invent. I called my file invent.vmware.yml

```
---
plugin: vmware_vm_inventory
strict: False
hostname: vc.homelab.domain.com
validate_certs: False
with_tags: True
hostnames: 
  - 'config.name'
compose:
  ansible_host: 'guest.hostName'
keyed_groups:
  - key: 'tags'
    separator: ''
```

Make sure to set you hostname and check in your file in a Git repository.

Create a project, my is called Inventory an select Git as SCCM Type. Add you Git repository URL as SCM URL. Don´t forget to create SCM credentials if you haven't done that already. Set /opt/my-envs/vm-tags as Ansible environment.

![](/wp-content/uploads/2020/11/ansible-awx-_-create-project.png)

Next step is to create a custom credential type. I called my Vmware\_Inventory

![](/wp-content/uploads/2020/11/credentialtype.png?w=1024)

```
#Input configuration
fields:
  - id: username
    type: string
    label: Username
  - id: password
    type: string
    label: Password
    secret: true
required:
  - username
  - password

#Injector configuration
env:
  VMWARE_PASSWORD: '{{password}}'
  VMWARE_USERNAME: '{{username}}'

```

Create a new credential with your new credential type. You will need a user with read permissions in your Vcenter server.

![](/wp-content/uploads/2020/11/create-inventory-credential.png?w=1024)

Go to Inventory and create a new Inventory and give a name and hit Save.

![](/wp-content/uploads/2020/11/2020-11-13-21_20_29-ansible-awx-_-vmwareinventory.png?w=1024)

Click sources and create a new source. Give it a name and select Sourced from a project as a sources. Set /opt/my-ens/vm-tags/ as ansible environment. Search for and select for your credential with read permission in Vcenter.

![](/wp-content/uploads/2020/11/inventoury-sources.png?w=1024)

Now you should be able to sync your new inventory.

![](/wp-content/uploads/2020/11/start-sync.png?w=1024)

After the sync has finished you should see your hosts and groups.

![](/wp-content/uploads/2020/11/2020-11-17-21_35_53-ansible-awx-_-all-groups-1.png?w=1024)

For example I have connection details as a group variable on my group win. All my windows server are in the group win. I needed you cand have second group like SQL servers and then add that group to win group to allow it to inherit all win group variables.

```
---
ansible_winrm_server_cert_validation: ignore
ansible_port: 5986
ansible_connection: winrm
ansible_winrm_transport: kerberos
```

![](/wp-content/uploads/2020/11/win-grouc2a8p.png?w=1024)

That's it, hopefully this can be helpful to any one seeking information about AWX and Vmware tags as inventory groups.
