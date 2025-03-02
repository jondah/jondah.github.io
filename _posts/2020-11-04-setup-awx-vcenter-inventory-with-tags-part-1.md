---
title: "Setup AWX Vcenter inventory with tags part 1"
date: "2020-11-04"
categories: 
  - "powershell"
redirect_from:
  - 2020/11/04/setup-awx-vcenter-inventory-with-tags-part-1/
  - 2020/11/04/__trashed/
---

New to AWX and I had a goal to setup Vcenter as an inventory source with groups based on vmware tags. I got that setup working with ansible and started to investigate how to achieve same result in AWX. After a couple of days testing I got some hints on Reddit and was able to get it working as expected. Hopefully this guide can help someone (and me next time) setup inventory with tags.

First step if not already done is to install AWX. I will not cover the setup. It is already covered here [https://github.com/ansible/awx/blob/devel/INSTALL.md](https://github.com/ansible/awx/blob/devel/INSTALL.md). I have chosen to install on a standalone Docker host in my Home lab running CentOS.  
  
![](/assets/img/edit-inventory.png)  
Open the inventory file install/inventory in your favorite editor.  
Look for and uncomment `custom_venv_dir=/opt/my-envs/`  
![](/assets/img/edit-inventory2.png)  
Create dir: `mkdir /opt/my-envs`

Run the playbook: `ansible-playbook install.yml -i inventory`

Create folder and Python env and install all prerequisites

```
mkdir /opt/my-envs/vm-tags
python3 -m venv /opt/my-envs/vm-tags/
source /opt/my-envs/vm-tags/bin/activate
yum install gcc
yum install python36-devel
pip3 install psutil
pip3 install ansible
pip3 install pyaml
pip3 install requests
pip3 install PyVmomi
pip3 install --upgrade pip setuptools
pip3 install --upgrade git+https://github.com/vmware/vsphere-automation-sdk-python.git
deactivate
```

Log on to AWX and navigate to Settings -> System.  
Add /opt/my-envs/vm-tags to CUSTOM VIRTUAL ENVIRONMENT PATHS  
![](/assets/img/awx-settings-env.png)

Last step in this part is to verify our new custom env. Go to ORGANIZATIONS and push then pencil to edit default organization.  
![](/assets/img/verify-env.png)  
Verify that you can see your new Ansible Environment.
