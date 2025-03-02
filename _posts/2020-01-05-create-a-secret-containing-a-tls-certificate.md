---
title: "Create a secret containing a TLS certificate"
date: "2020-01-05"
tags: 
  - "icp"
  - "kubernetes"
redirect_from:
  - 2020/01/05/create-a-secret-containing-a-tls-certificate/
---

Example code to create a secret in namespace lab containing a certificate to be used with an ingress for example.

`kubectl -n lab create secret tls tls-icp-cert-com --cert=cert.pem --key=cert.key`
