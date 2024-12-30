---
id: 26
title: New-Harddisk
date: '2017-06-07T07:34:49+00:00'
author: jonas
layout: post
guid: 'http://blog.jonasdahlgren.se/?p=26'
permalink: /2017/06/07/new-harddisk/
geo_public:
    - '0'
    - '0'
    - '0'
    - '0'
    - '0'
    - '0'
    - '0'
categories:
    - powercli
---

Short commad to add a new thin disk to a existing VM.

![2017-06-07 09_28_32-Välj Windows PowerShell](http://192.168.5.181/wp-content/uploads/2017/06/2017-06-07-09_28_32-vc3a4lj-windows-powershell.png)

\[code language=”powershell”\]New-HardDisk -VM TEST04 -StorageFormat Thin -CapacityGB 20\[/code\]