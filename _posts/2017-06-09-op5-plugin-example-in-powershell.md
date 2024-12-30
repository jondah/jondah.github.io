---
id: 73
title: 'OP5 plugin example in powershell'
date: '2017-06-09T10:53:25+00:00'
author: jonas
layout: post
guid: 'http://blog.jonasdahlgren.se/?p=73'
permalink: /2017/06/09/op5-plugin-example-in-powershell/
categories:
    - OP5
    - Uncategorized
---

If you want to monitor your windows server uptime in OP5. Run from NSClient++ on all hosts you want to monitor uptime on.

\[code language=”powershell”\]param  
(  
 \[parameter(Mandatory = $true)\]  
 \[int\]$w,  
 \[parameter(Mandatory = $true)\]  
 \[int\]$c  
)  
function Get-Uptime  
{  
 $os = Get-WmiObject win32\_operatingsystem  
 $uptime = (Get-Date) – ($os.ConvertToDateTime($os.lastbootuptime))  
 return $Uptime  
}  
$os = Get-CimInstance Win32\_OperatingSystem | Select-Object Caption | ForEach{ $\_.Caption }  
$uptime = Get-Uptime

if ( $uptime.Days -gt $c)  
{  
 Write-Host Write-Host "CRITICAL Uptime:" $Uptime.Days "days"  
 exit 2

}  
if ($uptime.Days -gt $w)  
{  
 Write-Host Write-Host "WARNING Uptime:" $Uptime.Days "days"  
 exit 1

}  
if ($uptime.Days -lt $w)  
{  
 Write-Host "OK" $Uptime.Days "days uptime"  
 Write-Host "OS: $os"  
 exit 0  
}\[/code\]