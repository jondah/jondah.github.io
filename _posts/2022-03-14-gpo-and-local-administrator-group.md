---
title: "GPO and local Administrator group"
date: "2022-03-14"
categories: 
  - "active-directory"
coverImage: "your-group.png"
---

It's common to use a Group policy preference to push a AD group to local admin group on a server or client.  
But something that you might not know is that you can use a variable in the group name to make it a bit more dynamic. With this setup you only need to create the GPO and one AD group per server. No need to logon or use a Powershell script to target each server and and add a group in to det local Administrators group.  
  
First create an empty GPO and link it to an OU containing your servers you want to target.  
![](/assets/img/create-gpo.png)  
Then edit your new GPO and go to **Computer configuration -> Preferences -> Local Users and Groups** and right click on the white area to the right and select New -> Local Group  
![](/assets/img/new-group-gpp.png)  
  
Set action to update and Administrators (built-in) as group name. Then press Add... under members.  
![](/assets/img/select-admin.png)  
  
As a member name add your desired group name for server admin groups and end it with the variable **%computername%**. In my lab environment the group name is homelab\\SeverLocalAdmin-%computername%. My AD domain is called homelab.  
![](/assets/img/your-group.png)  
  
Next part is to create a AD group following your naming convention and add the computer name at the end. My server I want to test this on is called SQL2016 so the group name is ServerLocalAdmin-SQL2016. 
![](/assets/img/new-group.png)  
  
Next step is to verify the result of the GPO. First log in on your server and then run gpupdate /force.  
![](/assets/img/gpupdate.png)  
  
Open Computer Management to verify the members in the local admin group.  
![](/assets/img/local-admin-target.png)
