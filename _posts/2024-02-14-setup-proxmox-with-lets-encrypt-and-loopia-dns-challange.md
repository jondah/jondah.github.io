---
id: 1152
title: 'Setup Proxmox with Let&#8217;s Encrypt and Loopia DNS challange'
date: '2024-02-14T20:01:00+00:00'
author: jonas
layout: post
guid: 'https://blog.jonasdahlgren.se/?p=1152'
permalink: /2024/02/14/setup-proxmox-with-lets-encrypt-and-loopia-dns-challange/
firehose_sent:
    - '1707937261'
    - '1707937261'
    - '1707937261'
    - '1707937261'
    - '1707937261'
    - '1707937261'
    - '1707937261'
wordads_ufa:
    - 's:wpcom-ufa-v4:1707937565'
    - 's:wpcom-ufa-v4:1707937565'
    - 's:wpcom-ufa-v4:1707937565'
    - 's:wpcom-ufa-v4:1707937565'
    - 's:wpcom-ufa-v4:1707937565'
    - 's:wpcom-ufa-v4:1707937565'
    - 's:wpcom-ufa-v4:1707937565'
timeline_notification:
    - '1707937262'
    - '1707937262'
    - '1707937262'
    - '1707937262'
    - '1707937262'
    - '1707937262'
    - '1707937262'
publicize_linkedin_url:
    - ''
    - ''
    - ''
    - ''
    - ''
    - ''
    - ''
categories:
    - Homelab
tags:
    - proxmox
---

I recently installed Proxmox in my homelab and wanted to use Let’s encrypt certificates for my nodes. I have my domain registered at Loopia a Swedish webhosting company. When using their DNS services it’s possible to create an API user for the ACME client to automate the DNS challenge. This is needed to prove that you own the domain.   
  
First log in to Loopia customer portal and create a subdomain that your proxmox node will use. In my case it’s pve.homelab.domain.com. No need to create a associated A record. That should be handled by your internal DNS.

<figure class="wp-block-image size-large">![](http://192.168.5.181/wp-content/uploads/2024/02/create_subdomain-2.png?w=850)</figure>Next step is to create a API user and assign appropriate permissions.

<figure class="wp-block-image size-large">![](http://192.168.5.181/wp-content/uploads/2024/02/api_user-2.png?w=945)</figure>It needs to have the following permissions:

- addZoneRecord
- getZoneRecords
- removeZoneRecord
- removeSubdomain

<figure class="wp-block-image size-large">![](http://192.168.5.181/wp-content/uploads/2024/02/api_permissions-3.png?w=890)</figure>Next step is to configure proxmox. Log in and locate ACME under datacenter. Click add under challenge plugins. Enter your Loopia API user in following format.   
`LOOPIA_User = proxmoxlab@loopiaapi<br></br>LOOPIA_Password = secretpassword`

<figure class="wp-block-image size-large">![](http://192.168.5.181/wp-content/uploads/2024/02/acme_settings-2.png?w=1024)</figure>Select the node you are creating a certificate for. Under certificates and ACME click add. Set DNS as challenge type. Plugin should be the one created in previously step. And as domain pve.homelab.domain.com   
  
The default Let’s encrypt account is configured for the staging environment. This could be used for testing. Create a new account for Let’s Encrypt V2 directory.

<figure class="wp-block-image size-large">![](http://192.168.5.181/wp-content/uploads/2024/02/registeraccount-3.png?w=1024)</figure><figure class="wp-block-image size-large">![](http://192.168.5.181/wp-content/uploads/2024/02/create_domain-1.png?w=1024)</figure>The final step is to press order certificate and wait for the validation. Make sure to select the production account.

<figure class="wp-block-image size-large">![](http://192.168.5.181/wp-content/uploads/2024/02/order_cert.png?w=1024)</figure><figure class="wp-block-image size-large">![](http://192.168.5.181/wp-content/uploads/2024/02/cert-1.png?w=541)</figure>Access node on https://pve.homelab.domain.com:8006/ and verify your new certificate.