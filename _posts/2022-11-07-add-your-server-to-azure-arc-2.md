---
id: 1868
title: 'Add your server to Azure Arc'
date: '2022-11-07T08:00:00+00:00'
author: jonas
layout: post
guid: 'https://blog.jonasdahlgren.se/?p=579'
permalink: /2022/11/07/add-your-server-to-azure-arc-2/
timeline_notification:
    - '1667805990'
publicize_linkedin_url:
    - ''
publicize_twitter_user:
    - JonasDahlgren
categories:
    - Azure
    - powershell
---

Azure Arc helps your manage your on-prem servers from Azure portal. To add a server to Azure Arc just search for “Servers – Azure Arc” in the portal and press Add.  
  
![](http://192.168.5.181/wp-content/uploads/2022/11/arc_script1-2.png)  
This time we will only add one server and can select Generate Script option.   
  
![](http://192.168.5.181/wp-content/uploads/2022/11/arc_script2.png)  
Select your subscription and a new or existing resource group. You also need to select a location.   
  
![](http://192.168.5.181/wp-content/uploads/2022/11/arc_script3.png)  
In this step you can add your desired tags.   
  
![](http://192.168.5.181/wp-content/uploads/2022/11/arc_script4-1.png)  
The script is ready to be downloaded or copied to your server.   
  
![](http://192.168.5.181/wp-content/uploads/2022/11/arc_run1.ps1_-3.png)  
Start powershell as local admin and navigate to the folder where your onbording script is stored.  
   
![](http://192.168.5.181/wp-content/uploads/2022/11/arc_run1.ps2_-3.png)  
The script will download and install Azure machine agent and open a web browser where you need to sign in to Azure.   
  
![](http://192.168.5.181/wp-content/uploads/2022/11/arc_portal1.png)  
After a couple of minutes our on-prem server is visible in Azure portal.  
  
![](http://192.168.5.181/wp-content/uploads/2022/11/arc_portal2-1.png)  
We can now see some details like operating system and the tags we defined during setup. In a future post I will show what we can achieve with Azure Arc enabled servers.