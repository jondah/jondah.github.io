---
id: 49
title: 'Change wallpaper on windows 7'
date: '2017-06-07T13:59:16+00:00'
author: jonas
layout: post
guid: 'http://blog.jonasdahlgren.se/?p=49'
permalink: /2017/06/07/change-backround-image-on-windows-7/
geo_public:
    - '0'
    - '0'
    - '0'
    - '0'
    - '0'
    - '0'
    - '0'
categories:
    - powershell
    - 'Windows 7'
---

I needed a script to change desktop wallpaper for all users using the default one with an outdated company logo on. I came up with this startup script as a solution. Set $filesize to the size of the old wallpaper to only change wallpapers with same filesize. Use as logon script.

\[code language=”powershell”\]$filesize = 100593  
$bg = Get-Item ("$env:USERPROFILE\\appdata\\Roaming\\Microsoft\\Windows\\Themes\\TranscodedWallpaper.jpg)  
if ($bg.length -eq $filesize)  
{  
Copy-item "\\\\fileserver\\UsrLogon\\wallpaper\\img0.jpg" "$env:USERPROFILE\\appdata\\Roaming\\Microsoft\\Windows\\Themes\\img0.jpg" -Force  
Set-ItemProperty -path ‘HKCU:\\Control Panel\\Desktop\\’ -name wallpaper -value "$env:USERPROFILE\\appdata\\Roaming\\Microsoft\\Windows\\Themes\\img0.jpg"  
rundll32.exe user32.dll, UpdatePerUserSystemParameters  
}\[/code\]