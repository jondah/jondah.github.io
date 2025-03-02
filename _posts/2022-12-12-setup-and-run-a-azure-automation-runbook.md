---
title: "Setup and run a Azure automation runbook"
date: "2022-12-12"
categories: 
  - "powershell"
redirect_from:
- 2022/12/12/setup-and-run-a-azure-automation-runbook/
---

A runbook can help you to run scripts on a schedule or trigger them with a webhook. You can either run them in Azure or on your on-premises server. This example will show how to run a script on a on-premises that is connected to Azure Arc.  
  
![](/assets/img/create-account2.png)  
First step is to create an automation account in your favorite region. You might need to create a resource group as well.  

![](/assets/img/hybrid1.png)  
Go to your newly created automation account and look for Hybrid worker groups.  

![](/assets/img/hybrid2.png)

Create a new hybrid worker group and select a name.  

![](/assets/img/hybrid3.png)  
Now we have a Hybrid worker group without any hybrid workers. Click hybrid workers to the left  
  
![](/assets/img/hybrid4.png)  
Click add  
  
![](/assets/img/hybrid5.png)  
Select one or more servers and add them to your hybrid worker group. If your list is empty you need to enable Azure Arc at least on one server.  
  
  
![](/assets/img/runbook_overview2.png)  
Back to automation account and press Runbooks.  
  
![](/assets/img/runbook_creation.png)  
Create a new runbook  
  
![](/assets/img/runbook_creation2.png)  
Give the runbook a name and select type. In this example Powershell och runtime version 5.1. 
![](/assets/img/runbook_content.png)  
Now to the fun part, edit the new runbook and write or paste your script. Select publish when done.  
  
![](/assets/img/runbook_start-1.png)  
When pressing start a meny to the right enables you to select to start the runbook and let it run on a server in your hybrid worker group.  
  
![](/assets/img/runbook_finish.png)  
When the runbook is finished we can view the output or errors. The last line shows the name of the server the script was executed on. Can be useful for troubleshooting if the hybrid worker group contains multiple servers.  
  
To make more use of this capability ro trigger script on a local server from Azure start explore schedules and webhooks.
