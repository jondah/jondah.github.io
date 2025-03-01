---
title: "Change wallpaper on windows 7"
date: "2017-06-07"
categories: 
  - "powershell"
  - "windows-7"
---

I needed a script to change desktop wallpaper for all users using the default one with an outdated company logo on. I came up with this startup script as a solution. Set $filesize to the size of the old wallpaper to only change wallpapers with same filesize. Use as logon script.

```powershell
$filesize = 100593
$bg = Get-Item ("$env:USERPROFILE\appdata\Roaming\Microsoft\Windows\Themes\TranscodedWallpaper.jpg)
if ($bg.length -eq $filesize){
  Copy-item "\\fileserver\UsrLogon\wallpaper\img0.jpg" "$env:USERPROFILE\appdata\Roaming\Microsoft\Windows\Themes\img0.jpg" -Force
  Set-ItemProperty -path 'HKCU:\Control Panel\Desktop\' -name wallpaper -value "$env:USERPROFILE\appdata\Roaming\Microsoft\Windows\Themes\img0.jpg"
  rundll32.exe user32.dll, UpdatePerUserSystemParameters
}
```
