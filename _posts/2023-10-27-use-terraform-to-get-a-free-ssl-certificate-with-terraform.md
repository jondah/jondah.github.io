---
title: "Use Terraform to get a free SSL certificate for Azure"
date: "2023-10-27"
categories: 
  - "azure"
tags: 
  - "terraform"
coverImage: "cert_in_keyvault.png"
---

I want to use a free SSL certificate from Let's encrypt to secure my Azure resources. For example an application behind a Application gateway. I will use a key vault to store my certificate. Beside azurerm Terraform provider vancluever/acme is also used. For documentation visit [https://registry.terraform.io/providers/vancluever/acme/latest/docs](https://registry.terraform.io/providers/vancluever/acme/latest/docs)  
  
In order to get this setup to work my domain needs to be handled by Azure a public DNS zone for DNS-01 challenge ([https://letsencrypt.org/docs/challenge-types/#dns-01-challenge](https://letsencrypt.org/docs/challenge-types/#dns-01-challenge))

First we need to create the dns zone and key vault in a TF-file for example main.tf

```
resource "azurerm_resource_group" "rg_dns" {
      name     = "rg-dns"
  location = "Sweden Central"
  
}
resource "azurerm_resource_group" "rg_keyvault" {
      name     = "rg-keyvault"
  location = "Sweden Central"
  
}
resource "azurerm_key_vault" "keyvault" {
    name = "kv-secrets-keyvault"
<code>    resource_group_name = azurerm_resource_group.rg_keyvault.name
</code>    location = azurerm_resource_group.rg_keyvault.location
    sku_name = "standard"
    tenant_id = "41ded048-xxxx-4758-a8a7-xxxxxxxxxxx"
    enable_rbac_authorization = true
}
resource "azurerm_dns_zone" "dns_public" {
  name                = "cloud.labbserver.online"
  resource_group_name = azurerm_resource_group.rg_dns.name
}
```

Point the domain or subdomain to your Azure DNS zone at your domain registrator. In this example Cloudflare is used.

![](/wp-content/uploads/2023/10/cloudflare.png?w=987)

Values for name servers are found on DNS zone overview page to the right.

![](/wp-content/uploads/2023/10/copy_ns_values.png?w=1024)

Before proceeding make sure that your user have permissions to create certificates in the key vault. For RBAC this can be handled direct on the resource or by inheritance. The server URL is this example is for the staging environment used for testing your code before applying for a production certificate. Read more at [https://letsencrypt.org/docs/staging-environment/](https://letsencrypt.org/docs/staging-environment/)

```
provider "acme" {
  server_url = "https://acme-staging-v02.api.letsencrypt.org/directory"
}

resource "tls_private_key" "private_key" {
  algorithm = "RSA"
}

resource "acme_registration" "reg" {
  account_key_pem = tls_private_key.private_key.private_key_pem
  email_address   = "blog@example.com"
}

resource "acme_certificate" "certificate" {
  account_key_pem           = acme_registration.reg.account_key_pem
  common_name               = "cloud.labbserver.online"
  dns_challenge {
    provider = "azuredns"

    config = {
        AZURE_SUBSCRIPTION_ID = "xxxxxx-xxxx-xxxx-xxxx-xxxxxxx"
        AZURE_RESOURCE_GROUP = "rg-dns"
    }
  }
}

resource "azurerm_key_vault_certificate" "keyvault" {
  name         = "cloud-labbserver-online"
  key_vault_id = azurerm_key_vault.keyvault.id

  certificate {
contents = acme_certificate.certificate.certificate_p12
    
  }
}
```

![](/wp-content/uploads/2023/10/cert_in_keyvault.png?w=1024)

As we can se we now have a certificate in our key vault and can see expiration date and have the possibility to download it to our local disk in different formats. If used in Azure it's best to load it directly from the key vault to make it easier to automate renewals.  

![](/wp-content/uploads/2023/10/cert_details.png?w=700)
