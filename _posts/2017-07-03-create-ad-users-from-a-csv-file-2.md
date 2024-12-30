---
id: 1836
title: 'Create AD users from a CSV-file'
date: '2017-07-03T05:23:22+00:00'
author: jonas
layout: post
guid: 'http://blog.jonasdahlgren.se/?p=138'
permalink: /2017/07/03/create-ad-users-from-a-csv-file-2/
categories:
    - Uncategorized
---

Here is an example of a script to create AD users from a CSV-file and assign a temoporary password.

\[code language=”powershell”\]  
param  
(  
 \[parameter(Mandatory = $true)\]  
 $infile,  
 $output  
)

$OU = "OU=Users,DC=PSLABB,Dc=local"

$users = Import-Csv -Path $infile

foreach ($user in $users)  
{  
 $rand = Get-Random -Minimum 1000 -Maximum 9999  
 $pwd = "Temp$rand"  
 $name = "$($user.givenname)" + " " + $($user.surname)

 New-ADUser -Name $name -GivenName $user.Givenname -Surname $user.Surname -SamAccountName $user.username -user $user.username -Description "Tempuser" -path $ou -enabled $true -AccountPassword (convertTo-securestring -AsPlainText "Temp$rand" -Force)

 $details = @{  
 ‘Name’= $name;  
 ‘username’ = $($user.username);  
 ‘Password’ = $pwd  
 }  
 $results = New-Object PSObject -Property $details  
 $results | Export-Csv -Path $output -NoTypeInformation -Append

}  
\[/code\]