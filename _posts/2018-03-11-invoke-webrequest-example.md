---
title: "Invoke webrequest example"
date: "2018-03-11"
categories: 
  - "powershell"
---

This example uses invoke-webrequest to retrieve computer information from a company reporting webpage. Only text inside TD elements are stored in a array for future use and added to a PSObject.

```powershell
function Get-ComputerInfo
{
    [CmdletBinding()]
    param
(
        [parameter(
             ValueFromPipeline = $true,
             position = 0,
             Mandatory = $true)]
    [string]$computername
)
 
    process
    {
        $Request = @{
            'domain'      = 'domain1.company.net'
            'name'    = $computername
        }
        $lab = Invoke-WebRequest -Uri https://reporting.company.net/searchcomputer.php -Body $Request -Method Post
 
        $output = $lab.ParsedHtml.body.getElementsBytagname('TD') | select -expand innerhtml
 
        if ($output[7] -match '\D\d\d\d\d\d\d\d')
        {
            $user = Get-ADUser -Identity $output[7] | select -ExpandProperty name
        }
        else
        {
            $user = "Unknown"
            $output[7]= "Unknown"
        }
 
        $result = New-Object PSObject -Property @{
            Computername        = $output[0];
            OU                  = $output[2];
            IP                  = $output[6];
            OS                  = $output[3];
            LastUser            = $output[7];
            LastUSerFullName    = $user;
            LastSeen            = $output[5];
            Master              = $output[4];
        }
        Write-Output $result | select computername, ou, ip, os, master, lastuser, lastuserfullname, lastseen
    }#End Process
}
```
