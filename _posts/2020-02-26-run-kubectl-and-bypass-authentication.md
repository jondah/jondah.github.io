---
title: "Run kubectl and bypass authentication"
date: "2020-02-26"
categories: 
  - "kubernetes"
redirect_from:
  - 2020/02/26/run-kubectl-and-bypass-authentication/
---

If you are unable to login to you cluster but still have access over SSH for example. Run this command to bypass cloudctl login.

`kubectl --kubeconfig /etc/cfc/conf/admin.kubeconfig get pods --all-namespaces`
