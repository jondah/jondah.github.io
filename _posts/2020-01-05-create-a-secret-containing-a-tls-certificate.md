---
id: 194
title: 'Create a secret containing a TLS certificate'
date: '2020-01-05T10:35:47+00:00'
author: jonas
layout: post
guid: 'http://blog.jonasdahlgren.se/?p=194'
permalink: /2020/01/05/create-a-secret-containing-a-tls-certificate/
timeline_notification:
    - '1578216951'
    - '1578216951'
    - '1578216951'
    - '1578216951'
    - '1578216951'
    - '1578216951'
categories:
    - Uncategorized
tags:
    - icp
    - kubernetes
---

Example code to create a secret in namespace lab containing a certificate to be used with an ingress for example.

` kubectl -n lab create secret tls tls-icp-cert-com --cert=cert.pem --key=cert.key `