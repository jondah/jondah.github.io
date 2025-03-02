---
title: "Setup Proxmox with Let's Encrypt and Loopia DNS challange"
date: "2024-02-14"
categories: 
  - "homelab"
tags: 
  - "proxmox"
redirect_from:
- 2024/02/14/setup-proxmox-with-lets-encrypt-and-loopia-dns-challange/
---

I recently installed Proxmox in my homelab and wanted to use Let's encrypt certificates for my nodes. I have my domain registered at Loopia a Swedish webhosting company. When using their DNS services it's possible to create an API user for the ACME client to automate the DNS challenge. This is needed to prove that you own the domain.  
  
First log in to Loopia customer portal and create a subdomain that your proxmox node will use. In my case it's pve.homelab.domain.com. No need to create a associated A record. That should be handled by your internal DNS.

![](/wp-content/uploads/2024/02/create_subdomain.png?w=850)

Next step is to create a API user and assign appropriate permissions.

![](/wp-content/uploads/2024/02/api_user.png?w=945)

It needs to have the following permissions:

- addZoneRecord

- getZoneRecords

- removeZoneRecord

- removeSubdomain

![](/wp-content/uploads/2024/02/api_permissions.png?w=890)

Next step is to configure proxmox. Log in and locate ACME under datacenter. Click add under challenge plugins. Enter your Loopia API user in following format.  
`LOOPIA_User = proxmoxlab@loopiaapi   LOOPIA_Password = secretpassword`

![](/wp-content/uploads/2024/02/acme_settings.png?w=1024)

Select the node you are creating a certificate for. Under certificates and ACME click add. Set DNS as challenge type. Plugin should be the one created in previously step. And as domain pve.homelab.domain.com  
  
The default Let's encrypt account is configured for the staging environment. This could be used for testing. Create a new account for Let's Encrypt V2 directory.

![](/wp-content/uploads/2024/02/registeraccount.png?w=1024)

  

![](/wp-content/uploads/2024/02/create_domain.png?w=1024)

The final step is to press order certificate and wait for the validation. Make sure to select the production account.

![](/wp-content/uploads/2024/02/order_cert.png?w=1024)

![](/wp-content/uploads/2024/02/cert.png?w=541)

Access node on https://pve.homelab.domain.com:8006/ and verify your new certificate.
