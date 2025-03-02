---
title: "Powershell group export"
date: "2020-11-23"
categories: 
  - "powershell"
redirect_from:
  - 2020/11/23/powershell-group-export/
---

I needed to get a list of people in som AD group for an audit so I wrote a quick script to export each group matching my filter to a CSV-file and populate it with name and samaccountname. Setting semi colon as an delimiter ensures that you can open the CSV-file in Excel with no additional work to get columns correct.

```powershell
$groups = Get-ADGroup -filter { name -like "Company-Fileserver-ACL*" }

foreach ($group in $groups)
{
	Write-Output $group.name
	$file=$group.name + ".csv"
	Get-ADGroupMember $group.name | Select-Object name, samaccountname | Export-Csv -path $file -NoTypeInformation -delimiter ";"
}
```
