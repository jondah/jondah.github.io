---
title: "Create a new host via OP5 API with powershell"
date: "2017-06-08"
categories: 
  - "op5"
  - "powershell"
---

If you need to create a new host with OP5 api you can use something like this. Template, hostgroups and contactgroups are not mandatory and can be removed from hash table.

```powershell
function new-op5host
(
    [parameter(Mandatory = $true)]
    [string]$hostname,
    [parameter(Mandatory = $true)]
    [string]$alias
)
{
    $username = 'api_user$Default'
    $password = "password"
     
    $base64AuthInfo = [Convert]::ToBase64String([Text.Encoding]::ASCII.GetBytes(("{0}:{1}" -f $username, $password)))
     
    $hash = @{
        file_id = "etc/hosts.cfg";
        host_name = "$hostname";
        address = "$hostname.domain.com";
        alias = "$alias";
        max_check_attempts = "3";
        notification_interval = "5";
        notification_options = "d", "r";
        notification_period = "24x7";
        template = "Win_hosts";
        hostgroups = "Windows servers";
        contact_groups = "Windowsservrar"
    }
    $jsonData = $hash | convertto-json
     
    $result = Invoke-RestMethod -Method POST -ContentType "application/json" -Uri "https://op5.domain.com/api/config/host" -Body $jsonData -Headers @{ Authorization = ("Basic {0}" -f $base64AuthInfo) }
     
    #Save
    $result = Invoke-RestMethod -Method POST -ContentType "application/json" -Uri "https://op5.domain.com/api/config/change" -Headers @{ Authorization = ("Basic {0}" -f $base64AuthInfo) }
         
}
```
