---
title: "Batch create portgroups"
date: "2018-04-25"
categories: 
  - "powercli"
  - "powershell"
---

How to create new portgroups in a VD Switch from a CSV-file. Very handy when adding several portgroups at one time. 
```powershell
$file = Import-Csv -path "c:\vlan.csv"
foreach ($row in $file)
{
    New-VDPortgroup -VDSwitch dvSwitch_LAB1 -Name $row.name -VlanId $row.vlanid
}
```
