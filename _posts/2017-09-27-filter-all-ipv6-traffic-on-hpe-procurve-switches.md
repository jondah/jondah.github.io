---
id: 147
title: 'Filter all IPv6 traffic on HPE procurve switches'
date: '2017-09-27T15:15:38+00:00'
author: jonas
layout: post
guid: 'http://blog.jonasdahlgren.se/?p=147'
permalink: /2017/09/27/filter-all-ipv6-traffic-on-hpe-procurve-switches/
categories:
    - 'HP Procurve'
    - networking
tags:
    - hpe
    - IPv6
    - Network
    - Procurve
---

This config will dropp all ipv6 packages on vlan 1  
`<br></br>ASW-02# conf<br></br>ASW-02(config)#  ipv6 access-list "drop-all-v6"<br></br>ASW-02(config-ipv6-acl)#    10 deny ipv6 ::/0 ::/0<br></br>ASW-02(config-ipv6-acl)#  exit<br></br>ASW-02(config)# vlan 1<br></br>ASW-02(vlan-1)# ipv6 access-group "drop-all-v6" vlan<br></br>ASW-02(vlan-1)#<br></br>`