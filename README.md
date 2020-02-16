# Azure VPN ECMP Testing Report

Azure VPN gateway support high redundancy architecture that customer can have multiple devices connect to Azure VPN via multiple tunnels. You can refer Azure [documentation]( https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-highlyavailable#activeactiveonprem) for more details. <br>
This document recorded my testing steps when I simulate this topology at Global Azure. There are some tips and lesson learn. <br>

## Topology

![](https://github.com/yinghli/AzureVPNECMP/blob/master/ECMP.jpg)

We use this topology to simulate high available customer scenarios. VNET2 is customer on Azure resource. It includes one VPN gateway and IP prefix 10.2.0.0/16. VNET3 and VNET33 simulate customer two on premise VPN devices but share with same customer IP prefix 10.3.0.0/16. Detail setup PowerShell can be refer [here](https://github.com/yinghli/AzureVPNECMP/blob/master/vpn1.txt).<br>

Parameters      | Azure          |OnPrem1       |OnPrem2    
----------------| -------------  |-----------   |---------     
BGP ASN         |65002           |65003         |65003         
BGP Public IP   |40.83.97.14     | 13.70.19.201 |13.75.118.3
BGP peer IP     |10.2.255.254    |10.3.255.254  |10.3.254.254    
Local Network   |10.2.0.0/16     |10.3.0.0/16   |10.3.0.0/16  

```
Tips:
1.	VNET3 and VNET33 should have same IP range. VM setup should have same IP address.
2.	Gateway subnet should be different. VPN gateway BGP ASN should be same. 
```

## Setup Verification 
After BGP is up, you can check with PowerShell that VPN gateway BGP status. <br>
```
Get-AzVirtualNetworkGatewayBGPPeerStatus -VirtualNetworkGatewayName vnet2gw -ResourceGroupName testrg2

LocalAddress      : 10.2.255.254
Neighbor          : 10.3.254.254
Asn               : 65003
State             : Connected
ConnectedDuration : 02:52:19.1202268
RoutesReceived    : 1
MessagesSent      : 199
MessagesReceived  : 202

LocalAddress      : 10.2.255.254
Neighbor          : 10.3.255.254
Asn               : 65003
State             : Connected
ConnectedDuration : 00:32:44.5785787
RoutesReceived    : 1
MessagesSent      : 2341
MessagesReceived  : 220
```

