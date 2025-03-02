---
title: "Create DHCP scopes from a CSV file"
date: "2020-05-05"
categories: 
  - "powershell"
redirect_from:
  - 2020/05/05/create-dhcp-scopes-from-a-csv-file/
  
---

A fast way to import multiple DHCP scopes to a DHCP server. Some settings needs to be added on top level. For example DNS servers.

Required header in CSV: **name;description;startrange;endrange;subnetmask;scopeid;router**

```powershell
$dhcpserver = "1.1.1.1"
$scopes = Import-Csv -Path dhcp.csv -Delimiter ";"
foreach ($scope in $scopes)
{
    $name = $scope.name
    $description = $scope.description
Write-Output "Creating scope  $name"
Add-DhcpServerv4Scope -ComputerName $dhcpserver -Name "$name" -Description "$description" -StartRange $scope.startrange -EndRange $scope.endrange -SubnetMask $scope.subnetmask -State Active -LeaseDuration 1.00:00:00
Set-DhcpServerv4OptionValue -Router $scope.router -ScopeId $scope.scopeid -ComputerName $dhcpserver
}
```
