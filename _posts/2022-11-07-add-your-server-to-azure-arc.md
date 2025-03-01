---
title: "Add your server to Azure Arc"
date: "2022-11-07"
categories: 
  - "azure"
  - "powershell"
---

Azure Arc helps your manage your on-prem servers from Azure portal. To add a server to Azure Arc just search for "Servers - Azure Arc" in the portal and press Add.  
  
![](/assets/img/arc_script1.png)  
This time we will only add one server and can select Generate Script option.  
  
![](/assets/img/arc_script2.png)  
Select your subscription and a new or existing resource group. You also need to select a location.  
  
![](/assets/img/arc_script3.png)  
In this step you can add your desired tags.  
  
![](/assets/img/arc_script4.png)  
The script is ready to be downloaded or copied to your server.  
  
![](/assets/img/arc_run1.ps1_.png)  
Start powershell as local admin and navigate to the folder where your onbording script is stored.  
  
![](/assets/img/arc_run1.ps2_.png)  
The script will download and install Azure machine agent and open a web browser where you need to sign in to Azure.  
  
![](/assets/img/arc_portal1.png)  
After a couple of minutes our on-prem server is visible in Azure portal.  
  
![](/assets/img/arc_portal2.png)  
We can now see some details like operating system and the tags we defined during setup. In a future post I will show what we can achieve with Azure Arc enabled servers.
