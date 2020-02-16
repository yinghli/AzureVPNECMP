# Azure VPN ECMP Testing Report

Azure VPN gateway support high redundancy architecture that customer can have multiple devices connect to Azure VPN via multiple tunnels. You can refer Azure [documentation]( https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-highlyavailable#activeactiveonprem) for more details. <br>
This document recorded my testing steps when I simulate this topology at Global Azure. There are some tips and lesson learn. <br>

## Topology

![](https://github.com/yinghli/AzureVPNECMP/blob/master/ECMP.jpg)

Parameters      | Azure          |OnPrem1       |OnPrem2    
----------------| -------------  |-----------   |---------     
BGP ASN         |65002           |65003         |65003         
BGP Public IP   |40.83.97.14     | 13.70.19.201 |13.75.118.3
BGP peer IP     |10.2.255.254    |10.3.255.254  |10.3.254.254    
Local Network   |10.2.0.0/16     |10.3.0.0/16   |10.3.0.0/16   

