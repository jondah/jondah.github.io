---
title: "Ansible dynamic inventory with Vcenter"
date: "2020-10-07"
categories: 
  - "ansible"
tags: 
  - "vcenter"
  - "vmware"
coverImage: "image.png"
---

In order to create your Ansible groups based on vmware tags you need to create a inventory file and name it to something.vmware.yaml.

Also make sure to enable vmware inventory plugin in ansible.cfg

```
[inventory]
enable_plugins = vmware_vm_inventory
```

something.vmware.yaml

```
plugin: vmware_vm_inventory
strict: False
hostname: vc.homelab.jonasdahlgen.se
username: administrator@vsphere.local
password: secretpassword
validate_certs: false
with_tags: true
properties:
    - 'name'
    - 'config.uuid'
    - 'config.name'
    - 'guest.toolsStatus'
    - 'guest.toolsRunningStatus'
    - 'guest.ipAddress'
    - 'configIssue'
    - 'config.bootOptions'
    - 'config.annotation'
    - 'config.alternateGuestName'
compose:
      ansible_host: 'guest.ipAddress'
keyed_groups:
        - key: 'tags'
          separator: ''
```

Test new inventory with ansible-inventory --graph -i vcenter.vmware.yaml Groups named Linux, ansible\_managed,win are created from Vmware tags.  

![](/wp-content/uploads/2020/10/image.png?w=1024)

```
[root@d4e7b19ae23b playbooks]# ansible-inventory --graph -i vcenter.vmware.yaml
@all:
|--@Linux:
| |--Ansible01_564d4590-0ab7-8192-33c4-8690b6394208
|--@ansible_managed:
| |--Ansible01_564d4590-0ab7-8192-33c4-8690b6394208
| |--SQL2016_421e22dd-ecbd-83f2-6010-b679a3840a70
|--@ungrouped:
| |--2019Template_42237f2e-f317-3fbd-2688-023de31ffcdf
| |--AD_564d896d-f1cb-3837-4c15-6355e1b78b1d
| |--Docker01_4223eb67-2e2b-519c-0f90-959234307b44
| |--Esxi1_4223f791-c6b1-294b-b8be-00441105f73b
| |--Ubuntu_template_4223cce6-d618-6994-1550-db4d872cdf98
| |--Vcenter7_564d3c9f-07a3-90fa-60df-bfb39b3f697b
| |--Win10_01_564da1fa-1da0-7b6b-d386-8b173a48fbc6
| |--centos_template_42232f6e-96bb-f385-7ea4-aedaec2dedea
| |--esxi-01_422366be-06b3-eb4e-b837-cb472cce4208
| |--esxi-02_4223d129-24c4-98fe-205f-af445f578a03
| |--esxi-03_4223e353-2be9-a388-5203-60f887bfb14a
|--@win:
| |--SQL2016_421e22dd-ecbd-83f2-6010-b679a3840a70
```
