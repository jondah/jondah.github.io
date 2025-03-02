---
title: "Terraform and Active Directory"
date: "2023-01-04"
categories: 
  - "powershell"
redirect_from:
- 2023/01/04/terraform-and-active-directory/
---

I have worked a lot with AD during the past years mostly with powershell. This time I needed to create a AD computer object with terraform and started to look into how to setup terraform AD provider.

First you need to configure the provider. I am running terraform on my non domain joined laptop. Terraform needs winrm access to a domain joined server with Active directory powershell modules installed. It's important to use capital letters in all FQDNs for kerberos to work both in provider.tf and krb5.conf.

provider.tf  

```
provider "ad" {
  winrm_hostname         = "SERVER.HOMELAB.DOMAIN.COM"
  winrm_username         = var.aduser
  winrm_password         = var.adpassword
  winrm_port             = 5986
  winrm_proto            = "https"
  winrm_pass_credentials = true
  krb_realm              = "HOMELAB.DOMAIN.COM"
  krb_conf               = "krb5.conf"
  krb_spn                = "SERVER"
  winrm_insecure         = true
}
```

We also need to create krb5.conf in order to set up kerberos authentication

```
[libdefaults]
   default_realm = HOMELAB.DOMAIN.COM
   dns_lookup_realm = false
   dns_lookup_kdc = false


[realms]
    HOMELAB.DOMAIN.COM = {
        kdc     =   DC01.HOMELAB.DOMAIN.COM
        admin_server = DC01.HOMELAB.DOMAIN.COM
        default_domain = HOMELAB.DOMAIN.COM
        master_kdc = DC01.HOMELAB.DOMAIN.COM
    }

[domain_realm]
    .kerberos.server = HOMELAB.DOMAIN.COM
    .homelab.domain.com = HOMELAB.DOMAIN.COM
    homelab.domain.com = HOMELAB.DOMAIN.COM
```

Now the provider should be configured and ready to use. Next step is to create a AD computer object in main.tf.

```
resource "ad_computer" "c" {
  name        = "test01"
  container   = "OU=Servers,OU=Stockholm,OU=SWE,DC=homelab,DC=domain,DC=com"
  description = "My TF AD object"
}
```

To test the code run `terraform apply -var aduser=adadmin -var adpassword=secretpw123`  
and wait for output.and type yes if everything seems fine.

![](/wp-content/uploads/2023/01/apply_computer-2.png?w=756)

Now we have a new computer object in AD managed with terraform.

Next thing I wanted to test was a bit more complex. I wanted to create a OU structure with several sub OUs for each office. In powershell you can solve it with a nested foreach loop.

```powershell
$sites = ("Malmo", "Ystad", "Karlstad")
$subOU = ("Servers","Computers","Groups","Users")

foreach ($site in $sites){
    New-ADOrganizationalUnit -Name $site -Description "My office in $($site)" -Path "OU=SWE,DC=homelab,DC=domain,DC=com"
    foreach ($ou in $subOU){
        New-ADOrganizationalUnit -Name $ou -Description "OU for $($ou)" -Path "OU=$($site),OU=SWE,DC=homelab,DC=domain,DC=com"
    }
}
```

In terraform we need to create two variables as lists. One containing each office and one with our sub OUs. We also use locals to combine them with the [setproduct](https://developer.hashicorp.com/terraform/language/functions/setproduct) function.  

```
variable "sites" {
  type = list
  default = ["Malmo", "Ystad", "Karlstad"]
}

variable "siteOUs" {
  type = list
  default = ["Servers", "Users", "Groups", "Computers"]
}

locals {
  ous = setproduct(var.sites, var.siteOUs)
}


```

In order to get this to work we first need to create all Office OUs with for\_each and our variable sites. For all sub OUs we use our locals named ous as a source and loops trough all combinations that we created with setproduct. We pick the name from the second array and a part of the path from the first array containing office names. Note that we make sure all office OUs are created first with depends\_on.

```
resource "ad_ou" "ou" { 
  for_each = toset(var.sites)
    name = each.value
    path = "OU=SWE,DC=homelab,DC=domain,DC=com"
    description = "OU for ${each.value} Office"
    protected = false
}

resource "ad_ou" "o" {
  for_each = {
    for o in local.ous : "${o[0]}-${o[1]}" => {
      name = o[1]
      path = "OU=${o[0]},OU=SWE,DC=homelab,DC=domain,DC=com"
      description = "OU for ${o[1]} in ${o[0]} Office"
    }

  }
  name        = each.value.name
  path        = each.value.path
  description = each.value.description
  protected   = false

    depends_on = [
    ad_ou.ou
  ]
}

```

![](/wp-content/uploads/2023/01/apply_ous.png?w=561)

This is the final result i AD console. I learned a lot while figure out how to solve this in terraform.  

![](/wp-content/uploads/2023/01/ad_final.png?w=691)
