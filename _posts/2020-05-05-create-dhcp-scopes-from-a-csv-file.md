---
id: 211
title: 'Create DHCP scopes from a CSV file'
date: '2020-05-05T15:12:10+00:00'
author: jonas
layout: post
guid: 'http://blog.jonasdahlgren.se/?p=211'
permalink: /2020/05/05/create-dhcp-scopes-from-a-csv-file/
geo_public:
    - '0'
    - '0'
    - '0'
    - '0'
    - '0'
    - '0'
    - '0'
timeline_notification:
    - '1588687933'
    - '1588687933'
    - '1588687933'
    - '1588687933'
    - '1588687933'
    - '1588687933'
    - '1588687933'
wordads_ufa:
    - 's:wpcom-ufa-v3-beta:1668674697'
    - 's:wpcom-ufa-v3-beta:1668674697'
    - 's:wpcom-ufa-v3-beta:1668674697'
    - 's:wpcom-ufa-v3-beta:1668674697'
    - 's:wpcom-ufa-v3-beta:1668674697'
    - 's:wpcom-ufa-v3-beta:1668674697'
    - 's:wpcom-ufa-v3-beta:1668674697'
categories:
    - powershell
---

A fast way to import multiple DHCP scopes to a DHCP server. Some settings needs to be added on top level. For example DNS servers.

Required header in CSV:  
**name;description;startrange;endrange;subnetmask;scopeid;router**

\[code language=”powershell”\]  
$dhcpserver = “1.1.1.1”  
$scopes = Import-Csv -Path dhcp.csv -Delimiter “;”  
foreach ($scope in $scopes)  
{  
 $name = $scope.name  
 $description = $scope.description  
Write-Output “Creating scope $name”  
Add-DhcpServerv4Scope -ComputerName $dhcpserver -Name “$name” -Description “$description” -StartRange $scope.startrange -EndRange $scope.endrange -SubnetMask $scope.subnetmask -State Active -LeaseDuration 1.00:00:00  
Set-DhcpServerv4OptionValue -Router $scope.router -ScopeId $scope.scopeid -ComputerName $dhcpserver  
}\[/code\]