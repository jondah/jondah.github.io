---
title: "Fix VMware tools not running on Windows servers"
date: "2017-06-20"
categories: 
  - "powercli"
  - "powershell"
redirect_from:
  - 2017/06/20/fix-vmware-tools-not-running-on-windows-servers/
---

I had some trouble with VMwaretools stopping on some machines and decided to automate restart och vmware tools. I wrote this script which takes a view parameter to view current status. If you start the script whitout the view parameter it will try to restart vmware tools.

`fix-toolsnorunning.ps1`

```powershell
param ( \[parameter(Mandatory = $false)\] \[switch\] $view )

#Connect to Vcenter $cred = Get-Credential Connect-VIServer vcenter -Credential $cred | Out-Null

Write-Host -ForegroundColor Green "Collecting VMs" $toolsvm=get-vm | where { $\_.GuestId -like "\*Windows\*" -and $\_.Extensiondata.Summary.Guest.ToolsStatus -like "toolsnotrunning" -and $\_.Powerstate -eq "PoweredOn"}

#Stop script if no VMs reports tools not running if ($toolsvm -eq $null) { Write-Host "Nothing to fix" Disconnect-VIServer vcenter -Confirm:$false exit }

if ($view) { Write-Host "VMs whitout tools running" foreach ($vm in $toolsvm) { Write-Host $vm } } Else {

foreach ($vm in $toolsvm) {

if ($vm -match "^(?<Name>\\S\*)\\s.\*$") { $vm\_name = $Matches.Name } else { $vm\_name = $vm }

$vm\_hostname = $vm\_name.name +".domain.local"

if (Test-Connection -Computername $vm\_hostname -BufferSize 16 -Count 1 -Quiet) { Invoke-Command -Computername $vm\_hostname -ScriptBlock { stop-Service -name vmvss; start-service vmvss } -Credential $cred Write-Host "Service restarted on $vm\_name" } Else { Write-Host -ForegroundColor Red "Unable to contact $vm\_name" }

} } #Disconnect from Vcenter. Disconnect-VIServer vcenter -Confirm:$false
```
