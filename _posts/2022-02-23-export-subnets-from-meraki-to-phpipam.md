---
title: "Export subnets from Meraki to phpIPAM"
date: "2022-02-23"
categories: 
  - "networking"
  - "powershell"
redirect_from:
  - 2022/02/23/export-subnets-from-meraki-to-phpipam/

---

In order to populate [phpIPAM](https://phpipam.net/) I needed to export all subnets that was already present in Meraki Dashboard without type all information manually. I found [PSMeraki](https://github.com/sanderkl/PSMeraki) module on Github which is a prerequisite for this script. The script creates a CSV that can be importet in to phpIPAM

```powershell
$networks = Get-MrkNetwork
$subnets =@()

foreach ($network in $networks) {
    foreach ($vlan in $network) {
        $vlans = Get-MrkNetworkvlan -networkId $network.id

            foreach ($vlan in $vlans){

                    if (!$vlan.subnet){
                        break
                    }
            else{
            $sub = Get-Subnet $vlan.subnet

            $subnetname = $network.name + "_" + $vlan.name 
            [hashtable]$net = @{}
            $net.add('VLAN',$vlan.id)
            $net.add('Section','Company')
            $net.add('Subnet',$sub.ipaddress)
            $net.add('Mask',$sub.Maskbits)
            $net.add('Domain',$network.name)
            $net.add('Description',$subnetname)

            $objVlan = New-Object -TypeName psobject -Property $net
            $subnets += $objVlan
            }
        }
    }
}
$subnets | Export-Csv -Path subnets.csv -Delimiter ","
```
