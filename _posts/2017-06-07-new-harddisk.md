---
title: "New-Harddisk"
date: "2017-06-07"
categories: 
  - "powercli"
---

Short commad to add a new thin disk to a existing VM.

![2017-06-07 09_28_32-Välj Windows PowerShell](/assets/img/2017-06-07-powershell.png)

```powershell
New-HardDisk -VM TEST04 -StorageFormat Thin -CapacityGB 20\
```
