---
id: 281
title: 'Setup AWX Vcenter inventory with tags part 2'
date: '2020-11-18T21:23:41+00:00'
author: jdahlgren
layout: post
guid: 'http://blog.jonasdahlgren.se/?p=281'
permalink: /2020/11/18/setup-awx-vcenter-inventory-with-tags-part-2/
timeline_notification:
    - '1605731024'
    - '1605731024'
    - '1605731024'
    - '1605731024'
    - '1605731024'
    - '1605731024'
    - '1605731024'
publicize_linkedin_url:
    - ''
    - ''
    - ''
    - ''
    - ''
    - ''
    - ''
publicize_twitter_user:
    - JonasDahlgren
    - JonasDahlgren
    - JonasDahlgren
    - JonasDahlgren
    - JonasDahlgren
    - JonasDahlgren
    - JonasDahlgren
wordads_ufa:
    - 's:wpcom-ufa-v3-beta:1666860264'
    - 's:wpcom-ufa-v3-beta:1666860264'
    - 's:wpcom-ufa-v3-beta:1666860264'
    - 's:wpcom-ufa-v3-beta:1666860264'
    - 's:wpcom-ufa-v3-beta:1666860264'
    - 's:wpcom-ufa-v3-beta:1666860264'
    - 's:wpcom-ufa-v3-beta:1666860264'
image: /wp-content/uploads/2020/11/2020-11-17-21_35_53-ansible-awx-_-all-groups-1.png
categories:
    - Ansible
    - Vmware
tags:
    - AWX
---

This is the second part in how you can setup Vmware inventory in AWX based on VM tags. [Link to first part](https://blog.jonasdahlgren.se/2020/11/04/__trashed/).

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

Create a project, my is called Inventory an select Git as SCCM Type. Add you Git repository URL as SCM URL. Don´t forget to create SCM credentials if you haven’t done that already. Set /opt/my-envs/vm-tags as Ansible environment.

<figure class="wp-block-image size-large">![](http://192.168.5.181/wp-content/uploads/2020/11/ansible-awx-_-create-project.png?w=1024)</figure>Next step is to create a custom credential type. I called my Vmware\_Inventory

<figure class="wp-block-image size-large">![](http://192.168.5.181/wp-content/uploads/2020/11/credentialtype.png?w=1024)</figure>```
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

<figure class="wp-block-image size-large">![](http://192.168.5.181/wp-content/uploads/2020/11/create-inventory-credential-1.png?w=1024)</figure>Go to Inventory and create a new Inventory and give a name and hit Save.

<figure class="wp-block-image size-large">![](http://192.168.5.181/wp-content/uploads/2020/11/2020-11-13-21_20_29-ansible-awx-_-vmwareinventory.png?w=1024)</figure>Click sources and create a new source. Give it a name and select Sourced from a project as a sources. Set /opt/my-ens/vm-tags/ as ansible environment. Search for and select for your credential with read permission in Vcenter.

<figure class="wp-block-image size-large">![](http://192.168.5.181/wp-content/uploads/2020/11/inventoury-sources.png?w=1024)</figure>Now you should be able to sync your new inventory.

<figure class="wp-block-image size-large">![](http://192.168.5.181/wp-content/uploads/2020/11/start-sync-1.png?w=1024)</figure>After the sync has finished you should see your hosts and groups.

<figure class="wp-block-image size-large">![](http://192.168.5.181/wp-content/uploads/2020/11/2020-11-17-21_35_53-ansible-awx-_-all-groups-1.png?w=1024)</figure>For example I have connection details as a group variable on my group win. All my windows server are in the group win. I needed you cand have second group like SQL servers and then add that group to win group to allow it to inherit all win group variables.

```
---
ansible_winrm_server_cert_validation: ignore
ansible_port: 5986
ansible_connection: winrm
ansible_winrm_transport: kerberos
```

<figure class="wp-block-image size-large">![](http://192.168.5.181/wp-content/uploads/2020/11/win-grouc2a8p-2.png?w=1024)</figure>That’s it, hopefully this can be helpful to any one seeking information about AWX and Vmware tags as inventory groups.