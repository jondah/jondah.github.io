---
title: "OP5 plugin example in powershell"
date: "2017-06-09"
categories: 
  - "op5"
---

If you want to monitor your windows server uptime in OP5. Run from NSClient++ on all hosts you want to monitor uptime on.

```powershell
param
(
    [parameter(Mandatory = $true)]
    [int]$w,
    [parameter(Mandatory = $true)]
    [int]$c
)
function Get-Uptime
{
    $os = Get-WmiObject win32_operatingsystem
    $uptime = (Get-Date) - ($os.ConvertToDateTime($os.lastbootuptime))
    return $Uptime
}
$os = Get-CimInstance Win32_OperatingSystem | Select-Object  Caption | ForEach{ $_.Caption }
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
}
```
