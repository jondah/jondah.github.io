---
title: "Filter all IPv6 traffic on HPE procurve switches"
date: "2017-09-27"
categories: 
  - "hp-procurve"
  - "networking"
tags: 
  - "hpe"
  - "ipv6"
  - "network"
  - "procurve"
---

This config will dropp all ipv6 packages on vlan 1
```
ASW-02# conf ASW-02(config)# ipv6 access-list "drop-all-v6" 
ASW-02(config-ipv6-acl)# 10 deny ipv6 ::/0 ::/0
ASW-02(config-ipv6-acl)# exit
ASW-02(config)# vlan 1 
ASW-02(vlan-1)# ipv6 access-group "drop-all-v6" vlan
ASW-02(vlan-1)#
```