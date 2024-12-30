---
id: 62
title: 'Create a new host via OP5 API with powershell'
date: '2017-06-08T13:45:08+00:00'
author: jdahlgren
layout: post
guid: 'http://blog.jonasdahlgren.se/?p=62'
permalink: /2017/06/08/create-a-new-host-via-op5-api-with-powershell/
categories:
    - OP5
    - powershell
---

If you need to create a new host with OP5 api you can use something like this.  
Template, hostgroups and contactgroups are not mandatory and can be removed from hash table.

 \[code language=”powershell”\]function new-op5host  
(  
 \[parameter(Mandatory = $true)\]  
 \[string\]$hostname,  
 \[parameter(Mandatory = $true)\]  
 \[string\]$alias  
)  
{  
 $username = ‘api\_user$Default’  
 $password = "password"

 $base64AuthInfo = \[Convert\]::ToBase64String(\[Text.Encoding\]::ASCII.GetBytes(("{0}:{1}" -f $username, $password)))

 $hash = @{  
 file\_id = "etc/hosts.cfg";  
 host\_name = "$hostname";  
 address = "$hostname.domain.com";  
 alias = "$alias";  
 max\_check\_attempts = "3";  
 notification\_interval = "5";  
 notification\_options = "d", "r";  
 notification\_period = "24×7";  
 template = "Win\_hosts";  
 hostgroups = "Windows servers";  
 contact\_groups = "Windowsservrar"  
 }  
 $jsonData = $hash | convertto-json

 $result = Invoke-RestMethod -Method POST -ContentType "application/json" -Uri "https://op5.domain.com/api/config/host" -Body $jsonData -Headers @{ Authorization = ("Basic {0}" -f $base64AuthInfo) }

 #Save  
 $result = Invoke-RestMethod -Method POST -ContentType "application/json" -Uri "https://op5.domain.com/api/config/change" -Headers @{ Authorization = ("Basic {0}" -f $base64AuthInfo) }

}\[/code\]