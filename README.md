 wi-fi wars: Enterprise Wlan Deployment with inter-Vlan Routing
 Packet Tracer project demonstrating end-to-end wireless deployment using VLAN segmentation, 802.1Q trunking, router-on-a-stick, and DHCP. core focus: Layer 1-3 troubleshooting using CDPand CLI verification.
 ##Topology
 [Network Topology](screenshots/topology.png)
 
 **Devices:**
 - 1x Router 2911 -router0
 - 1x switch 2960 - switch0
 - 1x Access point - AP-PT
 - 1x Wireless client - smartphone0
 
**Addressing:**

| Device | Interface | IP Address | VLAN |
| --- | --- | --- | --- |
| Router0 | G0/0.10 | 192.168.10.1/24 | 10 |
| Switch0 | VLAN 1 | 192.168.1.2/24 | 1 |
| Smartphone0 | Wireless0 | DHCP | 10 |
 
 
 
 ## objectives
 Deploy secure wireless access for VLAN 10 users. configure WPA2-PSK, enable inter-VLAN routing, and serve DHCP from router subinterface. Validate connectivity and document troubledhooting steps.
 
 ## key configurations
 
 ## switch0
 cisco
 Vlan 10
 name Wireless
 interface FASTEthernet0/5
 description TRUNK_TO_ROUTER
 switchport mode trunkingswitchport trunk allowed vlan 1,10
 interface FastEthernet0/8
 description TO_AP
 switchport mode access
 switchport access vlan 10
 no shutdown
 
 ## Router0
 interface GigabitEthernet0/0
 no shutdowninterface GigabitEthernet0/0.10
 description GATEWAY_FOR_VLAN10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0
 ip dhcp exclude - address 192.168.10.1
 ip dhcp pool VLAN10
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 dns-server 8.8.8.8
 
 AP-PT
 1. SSID: wifi-wars
 2. Authentication: WPA2-PSK
 3. passphrase : ccna2026
 4. VLAN:10
 
 Verification & Proof
 
 1. CDP Neighbor Discovery-identified
    correct trunk port
show cdp neighbors: fa0/5 vlans 1,10
![cdp](screenshots/cdp-neigbhos.png)
2. TRUNK VALIDATION
show interface trunk: Fa0/5 vlans 1,10
![trunk](screenshots/show-trunk.png)
3. DHCPBinding-client successfullyleased IP
show ip dhcp binding:192.168.10.2 0001.971B.BEED
![blinding](screenshots/dhcp-blinding.png)
4. End-to-Endconnectivity
Smartphone0: ping 192.168.10.1 4/4 success
![ping]{screenshot/phone-ipconfig.png)

Troubleshooting Log
Symptom                      Root cause                                    Resoulution Method                        Command Used
DHCP request failed:         Trunk configuredon fa0/1, cablein fa0/5:      Used CDP to find real nieghbor port:      show cdp neighbors
operational mode: down:      FGa0/8 administratively down:                 Enable interface:                         no shutdown
ping 192.168.10.1 timeout:   Tested before DHCP lease:                     Renewed IP first:                         ipconfig /renew

Skills Demonstrated
1. VLAN segmentation & 802.1Q trunking
2. Router-on-stick/subinterfaces
3. Wireless security: WPA2-PSK
4. DHCP server configuration & verification
5. Cisco Discovery Protocol for L2 troubleshooting
6. Systematic CLI debugging: Layer1-Layer3
7. Packet Tracer Simulation

Files Included
. Wifi-wars.pkt-Packet Tracer 8.2 file
. /screenshots-Verification images  
