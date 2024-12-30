---
id: 84
title: 'Upgrade firmware on HP Procurve switches with TFTP'
date: '2017-06-12T12:11:11+00:00'
author: jonas
layout: post
guid: 'http://blog.jonasdahlgren.se/?p=84'
permalink: /2017/06/12/upgrade-firmware-on-hp-procurve-switches-with-tftp/
categories:
    - 'HP Procurve'
    - networking
    - Uncategorized
---

Short instruction for firmware upgrade over TFTP and CLI.

![2017-06-12 07_33_52-Underhåll - Internet Explorer](http://192.168.5.181/wp-content/uploads/2017/06/2017-06-12-07_33_52-underhc3a5ll-internet-explorer.png)

Type `show version` to determine what version your switch is running on.

![2017-06-12 07_25_41-dahladm@csesv-rancid_ ~](http://192.168.5.181/wp-content/uploads/2017/06/2017-06-12-07_25_41-dahladmcsesv-rancid_.png)

Use command `copy tftp flash hostname filename primary` to download firmware from a TFTP server. Change hostname and filname to fit your enironment.

![2017-06-12 07_27_37-COM1 - PuTTY](http://192.168.5.181/wp-content/uploads/2017/06/2017-06-12-07_27_37-com1-putty.png)

Reload the switch and verify new firmware version.

![2017-06-12 07_52_20-RhinoTM (IND) _ Dymo](http://192.168.5.181/wp-content/uploads/2017/06/2017-06-12-07_52_20-rhinotm-ind-_-dymo1.png)

Done