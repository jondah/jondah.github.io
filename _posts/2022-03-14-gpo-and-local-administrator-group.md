---
id: 454
title: 'GPO and local Administrator group'
date: '2022-03-14T15:09:00+00:00'
author: jdahlgren
layout: post
guid: 'https://blog.jonasdahlgren.se/?p=454'
permalink: /2022/03/14/gpo-and-local-administrator-group/
timeline_notification:
    - '1647268262'
    - '1647268262'
    - '1647268262'
    - '1647268262'
    - '1647268262'
    - '1647268262'
    - '1647268262'
publicize_twitter_user:
    - JonasDahlgren
    - JonasDahlgren
    - JonasDahlgren
    - JonasDahlgren
    - JonasDahlgren
    - JonasDahlgren
    - JonasDahlgren
wordads_ufa:
    - 's:wpcom-ufa-v3-beta:1667637682'
    - 's:wpcom-ufa-v3-beta:1667637682'
    - 's:wpcom-ufa-v3-beta:1667637682'
    - 's:wpcom-ufa-v3-beta:1667637682'
    - 's:wpcom-ufa-v3-beta:1667637682'
    - 's:wpcom-ufa-v3-beta:1667637682'
    - 's:wpcom-ufa-v3-beta:1667637682'
image: /wp-content/uploads/2022/03/your-group-2.png
categories:
    - 'Active Directory'
---

It’s common to use a Group policy preference to push a AD group to local admin group on a server or client.   
But something that you might not know is that you can use a variable in the group name to make it a bit more dynamic. With this setup you only need to create the GPO and one AD group per server. No need to logon or use a Powershell script to target each server and and add a group in to det local Administrators group.   
  
First create an empty GPO and link it to an OU containing your servers you want to target.  
![](http://192.168.5.181/wp-content/uploads/2022/03/create-gpo.png)  
Then edit your new GPO and go to **Computer configuration -&gt; Preferences -&gt; Local Users and Groups** and right click on the white area to the right and select New -&gt; Local Group  
![](http://192.168.5.181/wp-content/uploads/2022/03/new-group-gpp.png)  
  
Set action to update and Administrators (built-in) as group name. Then press Add… under members.  
![](http://192.168.5.181/wp-content/uploads/2022/03/select-admin-2.png)  
  
As a member name add your desired group name for server admin groups and end it with the variable **%computername%**. In my lab environment the group name is homelab\\SeverLocalAdmin-%computername%. My AD domain is called homelab.  
![](http://192.168.5.181/wp-content/uploads/2022/03/your-group-1.png)  
  
Next part is to create a AD group following your naming convention and add the computer name at the end. My server I want to test this on is called SQL2016 so the group name is ServerLocalAdmin-SQL2016.  
![](http://192.168.5.181/wp-content/uploads/2022/03/new-group-1.png)  
  
Next step is to verify the result of the GPO. First log in on your server and then run gpupdate /force.  
![](http://192.168.5.181/wp-content/uploads/2022/03/gpupdate-1.png)  
  
Open Computer Management to verify the members in the local admin group.  
![](http://192.168.5.181/wp-content/uploads/2022/03/local-admin-target-2.png)