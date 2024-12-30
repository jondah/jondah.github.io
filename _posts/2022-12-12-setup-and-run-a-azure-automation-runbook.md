---
id: 1869
title: 'Setup and run a Azure automation runbook'
date: '2022-12-12T08:00:00+00:00'
author: jonas
layout: post
guid: 'https://blog.jonasdahlgren.se/?p=616'
permalink: /2022/12/12/setup-and-run-a-azure-automation-runbook/
wordads_ufa:
    - 's:wpcom-ufa-v3-beta:1670877570'
    - 's:wpcom-ufa-v3-beta:1670877570'
    - 's:wpcom-ufa-v3-beta:1670877570'
    - 's:wpcom-ufa-v3-beta:1670877570'
    - 's:wpcom-ufa-v3-beta:1670877570'
    - 's:wpcom-ufa-v3-beta:1670877570'
    - 's:wpcom-ufa-v3-beta:1670877570'
timeline_notification:
    - '1670877447'
    - '1670877447'
    - '1670877447'
    - '1670877447'
    - '1670877447'
    - '1670877447'
    - '1670877447'
publicize_linkedin_url:
    - ''
    - ''
    - ''
    - ''
    - ''
    - ''
    - ''
publicize_twitter_user:
    - JonasDahlgren
    - JonasDahlgren
    - JonasDahlgren
    - JonasDahlgren
    - JonasDahlgren
    - JonasDahlgren
    - JonasDahlgren
categories:
    - powershell
---

A runbook can help you to run scripts on a schedule or trigger them with a webhook. You can either run them in Azure or on your on-premises server. This example will show how to run a script on a on-premises that is connected to Azure Arc.  
  
![](http://192.168.5.181/wp-content/uploads/2022/12/create-account2.png)  
First step is to create an automation account in your favorite region. You might need to create a resource group as well.

![](http://192.168.5.181/wp-content/uploads/2022/12/hybrid1-4.png)  
Go to your newly created automation account and look for Hybrid worker groups.

![](http://192.168.5.181/wp-content/uploads/2022/12/hybrid2-2.png)

Create a new hybrid worker group and select a name.

![](http://192.168.5.181/wp-content/uploads/2022/12/hybrid3-2.png)  
Now we have a Hybrid worker group without any hybrid workers. Click hybrid workers to the left  
  
![](http://192.168.5.181/wp-content/uploads/2022/12/hybrid4-2.png)  
Click add  
  
![](http://192.168.5.181/wp-content/uploads/2022/12/hybrid5-2.png)  
Select one or more servers and add them to your hybrid worker group. If your list is empty you need to enable Azure Arc at least on one server.  
  
  
![](http://192.168.5.181/wp-content/uploads/2022/12/runbook_overview2-1.png)  
Back to automation account and press Runbooks.  
  
![](http://192.168.5.181/wp-content/uploads/2022/12/runbook_creation.png)  
Create a new runbook  
  
![](http://192.168.5.181/wp-content/uploads/2022/12/runbook_creation2-4.png)  
Give the runbook a name and select type. In this example Powershell och runtime version 5.1.   
![](http://192.168.5.181/wp-content/uploads/2022/12/runbook_content.png)  
Now to the fun part, edit the new runbook and write or paste your script. Select publish when done.   
  
![](http://192.168.5.181/wp-content/uploads/2022/12/runbook_start-3-1.png)  
When pressing start a meny to the right enables you to select to start the runbook and let it run on a server in your hybrid worker group.  
  
![](http://192.168.5.181/wp-content/uploads/2022/12/runbook_finish-5.png)  
When the runbook is finished we can view the output or errors. The last line shows the name of the server the script was executed on. Can be useful for troubleshooting if the hybrid worker group contains multiple servers.   
  
To make more use of this capability ro trigger script on a local server from Azure start explore schedules and webhooks.