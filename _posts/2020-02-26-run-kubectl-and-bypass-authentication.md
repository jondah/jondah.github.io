---
id: 199
title: 'Run kubectl and bypass authentication'
date: '2020-02-26T12:24:53+00:00'
author: jonas
layout: post
guid: 'http://blog.jonasdahlgren.se/?p=199'
permalink: /2020/02/26/run-kubectl-and-bypass-authentication/
timeline_notification:
    - '1582716296'
    - '1582716296'
    - '1582716296'
    - '1582716296'
    - '1582716296'
    - '1582716296'
    - '1582716296'
categories:
    - kubernetes
---

If you are unable to login to you cluster but still have access over SSH for example. Run this command to bypass cloudctl login.

`kubectl --kubeconfig /etc/cfc/conf/admin.kubeconfig get pods --all-namespaces`