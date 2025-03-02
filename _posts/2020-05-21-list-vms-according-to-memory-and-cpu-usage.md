---
title: "List VMs according to memory and CPU usage"
date: "2020-05-21"
categories: 
  - "powercli"
  - "powershell"
tags: 
  - "vcenter"
  - "vmware"
redirect_from:
  - 2020/05/21/list-vms-according-to-memory-and-cpu-usage/
---

For internal billing purpose I needed a way list all Windows VMs for a given subsidiary and their CPU and memory configuration.

```powershell
Connect-VIServer -Server vcenter.corp.lan

$var = get-vm -location Subsidiary1 | Where{ $_.Guest.OSFullName -like '*windows*' }  | select numcpu, memorygb | Group-Object numcpu,memorygb

function get-numOfVms
{
	param
	(
		[parameter(Mandatory = $true)]
		[pscustomobject]$VMs
	)

	$results = foreach ($row in $var)
	{
		$cpu, $mem = $row.Name -split ',', 2
		[pscustomobject]@{
			NumOfVMs = $row.Count
			NumOfCPUs   = $cpu
			MemoryGB = $mem.Trim()
		}
	}
	
	return $results
}
$total = get-numOfVms -VMs $var
$total | Export-Csv -Path totalvms.csv -NoTypeInformation
```

Example of totalvms.csv. It gives you a number of each specific CPU and memory configuration.

```
"NumOfVMs","NumOfCPUs","MemoryGB"
"1","2","8"
"12","1","4"
"4","4","8"
"2","4","4"
"9","1","8"
"5","4","12"
"22","4","32"
"6","4","16"
"2","1","12"
"1","4","24"
"1","1","16"
"1","4","6"
"1","1","6"
"1","24","32"
"1","4","25"
"3","2","16"
"1","8","6"
"1","1","3"
```
