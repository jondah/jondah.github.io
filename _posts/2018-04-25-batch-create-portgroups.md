---
id: 179
title: 'Batch create portgroups'
date: '2018-04-25T10:29:25+00:00'
author: jdahlgren
layout: post
guid: 'http://blog.jonasdahlgren.se/?p=179'
permalink: /2018/04/25/batch-create-portgroups/
timeline_notification:
    - '1524648569'
    - '1524648569'
    - '1524648569'
    - '1524648569'
    - '1524648569'
    - '1524648569'
    - '1524648569'
categories:
    - powercli
    - powershell
---

How to create new portgroups in a VD Switch from a CSV-file. Very handy when adding several portgroups at one time.  
\[code language=”powershell”\]  
$file = Import-Csv -path “c:\\vlan.csv”  
foreach ($row in $file)  
{  
 New-VDPortgroup -VDSwitch dvSwitch\_LAB1 -Name $row.name -VlanId $row.vlanid  
}  
\[/code\]