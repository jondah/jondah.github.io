---
title: "Migrate VMs between clusters with powecli"
date: "2017-11-08"
categories: 
  - "powercli"
  - "powershell"
---

A short script to migrate VMs to a new cluster or host. Migrates one vm at a time to save network bandwith. After migration it upgrades Vmware tools to match current host version.

Takes a CSV file as input with VMs to migrate.

```powershell
Param (
    [Parameter (Mandatory = $True)]
    $file
)
$vms = Import-Csv $file
 
foreach ($vm in $vms)
{
    Write-Host "Migrating VM" $vm.name
    Move-VM -VM $vm.name -Destination Cluster01
    Write-Host "Updating Vm-tools on " $vm.name
    Update-Tools -VM $vm.name -NoReboot
}
```
