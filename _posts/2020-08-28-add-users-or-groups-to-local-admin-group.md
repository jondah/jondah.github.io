---
id: 232
title: 'Add users or groups to local admin group'
date: '2020-08-28T21:14:42+00:00'
author: jonas
layout: post
guid: 'http://blog.jonasdahlgren.se/?p=232'
permalink: /2020/08/28/add-users-or-groups-to-local-admin-group/
spay_email:
    - ''
    - ''
    - ''
    - ''
    - ''
    - ''
    - ''
timeline_notification:
    - '1598645685'
    - '1598645685'
    - '1598645685'
    - '1598645685'
    - '1598645685'
    - '1598645685'
    - '1598645685'
publicize_twitter_user:
    - JonasDahlgren
    - JonasDahlgren
    - JonasDahlgren
    - JonasDahlgren
    - JonasDahlgren
    - JonasDahlgren
    - JonasDahlgren
amp_status:
    - ''
    - ''
    - ''
    - ''
    - ''
    - ''
    - ''
categories:
    - powershell
tags:
    - multiserver
---

Sometimes you need to add users or groups in local Administrators group on a windows server. This function helps to accomplish that on one or more servers. Load a text- or csv-file and pipe it to Add-AdminGroup. All servers not responding will be shown at the end for later follow-up.

```
function Add-AdminGroup
{
	Param (
		[parameter(Mandatory = $true,
				   ValueFromPipeline = $true,
				   position = 0)]
		[Alias('IPAddress', '__Server', 'CN', 'server')]
		[string[]]$Computername,
		[parameter(ValueFromPipelineByPropertyName)]
		[Alias('groupname', 'adgroup')]
		[string[]]$group
	)
	
	Process
	{
		if (Test-Connection -quiet -Computername $computername)
		{
			Write-Output "Adding $group to local administrators on" $Computername
			Invoke-Command -ComputerName $Computername -ScriptBlock {
			Add-LocalGroupMember -Group Administrators -Member $args[0]
			} -ArgumentList $group
			
		}
		else
		{
			write-output "No response from" $Computername
			$failed += $computername
		}
		
	}
	end
		{
			foreach ($obj in $failed)
			{
				Write-Output $obj
			}
		}
	
}
```