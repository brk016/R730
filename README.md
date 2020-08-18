# Openlabs
> TBD Picture here

# IP Addressing

# Wind River Corporate Range
```html
147.11.127.0/24
147.11.127.230 - 147.11.127.254
147.11.127.230	: ala-se0-lx
147.11.127.231	: ala-se1-lx
147.11.127.232	: ala-se2-lx
147.11.127.233	: ala-se3-lx
147.11.127.234	:
147.11.127.235	:
147.11.127.236	:
147.11.127.237	:
147.11.127.238	:
147.11.127.239	:
147.11.127.240	: PAC-6009 - Floating
147.11.127.241	: PAC-6009 - node 1
147.11.127.242	: PAC-6009 - node 2
147.11.127.243	: PAC-6009 - node 3
147.11.127.244	: PAC-6009 - node 4
147.11.127.245	: PAC-6009 - node 5.1
147.11.127.246	: PAC-6009 - node 5.2
147.11.127.247	: PAC-6009 - node 6.1
147.11.127.248	: PAC-6009 - node 6.2
147.11.127.249	: PAC-6009 - node 7.1
147.11.127.250	: PAC-6009 - node 7.2
147.11.127.251	: (was node 7.2 - check)
147.11.127.252	:
147.11.127.254	:
147.11.127.254	:

```

# Lab Isolated Range 147.11.88.0/22
88.0	Partner Customer Lab<br>
89.0	Partner Customer Lab<br>
90.0	Partner Customer Lab<br>
91.0	Partner Customer Lab<br>
These IP are reachable from the Wind River Corp network, but they cannot get back to the Wind River Corp network.  Which makes them sutible for customer/partner VPN access.

## 147.11.88.0/22 - 147.11.88.254
Currently used in Partner lab, document IPs are currently @ http://twiki.wrs.com/PBUeng/SantaClaraTiSCloudLab.  This info needs to be moved to git.

## 147.11.89.0/22 - 147.11.89.254
Currently open

## 147.11.90.0/22 - 147.11.90.254
Used for Lab R&D + PXE/DHCP
```
147.11.90.0	: ala-se0-lx
147.11.90.1	: PAC6009 floating
147.11.90.2	: PAC6009 controller-0
147.11.90.3	: PAC6009 controller-1
147.11.90.4	:
147.11.90.5	: lanner-6200-1
147.11.90.6	: lanner-6200-2
147.11.90.7	:
147.11.90.8	:
147.11.90.9	:
147.11.90.10	: subcloud-0005 OAM
147.11.90.11	:
147.11.90.12	:
147.11.90.13	:
147.11.90.14	:
147.11.90.15	:
147.11.90.16	:
147.11.90.17	:
147.11.90.18	:
147.11.90.19	:
147.11.90.20	: Dell R730 DX Floating
147.11.90.21	: Dell R730 Controller-0   (iDRAC 147.11.88.52 root/windriver)
147.11.90.22	: Dell R730 Controller-1   (iDRAC 147.11.88.53 root/windriver)
147.11.90.23	:
147.11.90.24	:
147.11.90.25	:
147.11.90.26	:
147.11.90.27	:
147.11.90.28	:
147.11.90.29	:
147.11.90.30	:
147.11.90.31	:
147.11.90.32	:
147.11.90.33	:
147.11.90.34	:
147.11.90.35	:
147.11.90.36	:
147.11.90.37	:
147.11.90.38	:
147.11.90.39	:
147.11.90.40	:
147.11.90.41	:
147.11.90.42	:
147.11.90.43	:
147.11.90.44	:
147.11.90.45	:
147.11.90.46	:
147.11.90.47	:
147.11.90.48	:
147.11.90.49	:
147.11.90.50-255: DHCP/PXE
```


## 147.11.91.0/22 - 147.11.91.254
Lab BMC address - TBD to move partner lab BMCs to this range.
```
147.11.91.0	: ala-se0-lx
147.11.91.1	: ala-se1-lx
147.11.91.2	: ala-se2-lx - sublcoud-0005
147.11.91.3	: ala-se3-lx
147.11.91.4	: ala-jump-vep4600
147.11.91.5	: lanner-6200-1
147.11.91.6	: lanner-6200-2
147.11.91.7
147.11.91.8	: ala-openlabs-40G QUANTA 40/10G
147.11.91.9	: ala-openlabs-10G Cisco Nexus 3548-X
147.11.91.10	: Kontron SYMKLOUD ALA 1 Switch 1
147.11.91.11	: Kontron SYMKLOUD ALA 1 Switch 2
147.11.91.12	: Kontron SYMKLOUD ALA 1 Base IP
147.11.91.13	: Kontron SYMKLOUD ALA 1 HubNode 1
147.11.91.14	: Kontron SYMKLOUD ALA 1 HubNode 2
147.11.91.15	: Kontron SYMKLOUD ALA 1 Node 1
147.11.91.16	: Kontron SYMKLOUD ALA 1 Node 2
147.11.91.17	: Kontron SYMKLOUD ALA 1 Node 3
147.11.91.18	: Kontron SYMKLOUD ALA 1 Node 4
147.11.91.19	: Kontron SYMKLOUD ALA 1 Node 5
147.11.91.20	: Kontron SYMKLOUD ALA 1 Node 6
147.11.91.21	: Kontron SYMKLOUD ALA 1 Node 7
147.11.91.22	: Kontron SYMKLOUD ALA 1 Node 8
147.11.91.23	: Kontron SYMKLOUD ALA 1 Node 9
147.11.91.24	: Kontron SYMKLOUD ALA 2 Switch 1
147.11.91.25	: Kontron SYMKLOUD ALA 2 Switch 2
147.11.91.26	: Kontron SYMKLOUD ALA 2 Base IP
147.11.91.27	: Kontron SYMKLOUD ALA 2 HubNode 1
147.11.91.28	: Kontron SYMKLOUD ALA 2 HubNode 2
147.11.91.29	: Kontron SYMKLOUD ALA 2 Node 1
147.11.91.30	: Kontron SYMKLOUD ALA 2 Node 2
147.11.91.31	: Kontron SYMKLOUD ALA 2 Node 3
147.11.91.32	: Kontron SYMKLOUD ALA 2 Node 4
147.11.91.33	: Kontron SYMKLOUD ALA 2 Node 5
147.11.91.34	: Kontron SYMKLOUD ALA 2 Node 6
147.11.91.35	: Kontron SYMKLOUD ALA 2 Node 7
147.11.91.36	: Kontron SYMKLOUD ALA 2 Node 8
147.11.91.37	: Kontron SYMKLOUD ALA 2 Node 9
147.11.91.38	: 
147.11.91.39	: 
147.11.91.40	: Advantech PAC-6009 ShMc
147.11.91.41	: Advantech PAC-6009 Node 1
147.11.91.42	: Advantech PAC-6009 Node 2
147.11.91.43	: Advantech PAC-6009 Node 3
147.11.91.44	: Advantech PAC-6009 Node 4
147.11.91.45	: Advantech PAC-6009 Node 5.1
147.11.91.46	: Advantech PAC-6009 Node 5.2
147.11.91.47	: Advantech PAC-6009 Node 6.1
147.11.91.48	: Advantech PAC-6009 Node 6.2
147.11.91.49	: Advantech PAC-6009 Node 7.1 [not working]
147.11.91.50	: Advantech PAC-6009 Node 7.2 [not working]
147.11.91.51	: Advantech PAC-6009 Node 8.1 [not installed]
147.11.91.52	: Advantech PAC-6009 Node 8.2 [not installed]
147.11.91.53	: Advantech PAC-6009 Node 9.1 [not installed]
147.11.91.54	: Advantech PAC-6009 Node 9.2 [not installed]
147.11.91.55	:
147.11.91.56	:
147.11.91.57	:
147.11.91.58	:
147.11.91.59	:
147.11.91.60	:
```


# DMZ Range 147.11.92.0/23 - Used for openlabs
```
 	        92.0	Colo lab Public
 	        93.0	Colo lab Public
 	        94.0	Colo Lab Management
 	        95.0	Reserved for Ala-LabOPs Extranet labs
```

Document individual IPs - TBD

## DNS Names and Certs
The following DNS names are registered
```
147.11.92.230 : system controller floating [openlabs-sys-ctrl.wrs.com]
147.11.92.231 : system controller-0        [openlabs-sys-ctrl-0.wrs.com]
147.11.92.232 : system controller-1        [openlabs-sys-ctrl-1.wrs.com]
147.11.92.233 : subcloud-0000 floating     [openlabs-sub-0.wrs.com]
147.11.92.234 : subcloud-0000 controller-0 [openlabs-sub-0-0.wrs.com]
147.11.92.235 : subcloud-0000 controller-1 [openlabs-sub-0-1.wrs.com]
147.11.92.236 : subcloud-0001 floating
147.11.92.237 : subcloud-0001 controller-0
147.11.92.238 : subcloud-0001 controller-1
147.11.92.239 : subcloud-0002 SX
147.11.92.240 : sudbloud-0003 SX
147.11.92.241 : subcloud-0004 SX
147.11.92.242 : subcloud-0005 SX

 147.11.92.252 R5 DX Floating
 147.11.92.253 R5 DX controller-0
 147.11.92.254
```


# Jump stations

## ALA 

- IP: 147.11.92.224
- Users:
    - eraineri/Ask Eddy
    - wruser / Ask Eddy/Gooch
- BMC: 147.11.91.4
```
        $ ipmitool -I lanplus -U admin -P Li69nux* -H 147.11.91.4 power status
```

# Switches

## Cisco Nexus 3548-X 10 switch 
    - ssh 147.11.91.9 [sysadmin/Li69nux*]
    - OLD: user: rku Pass: Cable!123
Guide:  https://www.cisco.com/c/en/us/td/docs/switches/datacenter/nexus3548/sw/layer_2_switching/60x/b_Cisco_N3548_Layer_2_Switching_Config_602_A1_1/b_Cisco_N3548_Layer_2_Switching_Config_602_A1_1_chapter_0100.pdf

Currently not used - Sample config

```
ala-openlabs-10G# configure
ala-openlabs-10G(config)# vlan 100-199

Node A
 Eth1/1
   ala-openlabs-10G# configure terminal
   ala-openlabs-10G(config)# interface ethernet Eth1/1
   ala-openlabs-10G(config-if)# switchport
   ala-openlabs-10G(config-if)# switchport mode trunk
   ala-openlabs-10G(config-if)# switchport trunk native vlan 1
   ala-openlabs-10G(config-if)# switchport trunk allowed vlan add 1,100-199
   ala-openlabs-10G(config-if)# no shutdown	
   ala-openlabs-10G(config-if)# show interface ethernet Eth1/1


Sample computes
 compute-00
 Eth1/5
   ala-openlabs-10G# configure terminal
   ala-openlabs-10G(config)# interface ethernet Eth1/5
   ala-openlabs-10G(config-if)# switchport
   ala-openlabs-10G(config-if)# switchport mode trunk
   ala-openlabs-10G(config-if)# switchport trunk allowed vlan add 100-199
   ala-openlabs-10G(config-if)# no shutdown	
   ala-openlabs-10G(config-if)# show interface ethernet Eth1/5
 compute-01
 Eth1/6
   ala-openlabs-10G# configure terminal
   ala-openlabs-10G(config)# interface ethernet Eth1/6
   ala-openlabs-10G(config-if)# switchport
   ala-openlabs-10G(config-if)# switchport mode trunk
   ala-openlabs-10G(config-if)# switchport trunk allowed vlan add 100-199
   ala-openlabs-10G(config-if)# no shutdown	
   ala-openlabs-10G(config-if)# show interface ethernet Eth1/6

to save:
ala-openlabs-10G(config-if)# copy running-config startup-config

```

## QUANTA 40/10G IZ1  
```       
147.11.91.8
for WRLinux [onsadmin/onsadmin]
for swtich [admin/admin] 

Switch >en
Switch #config
Switch (config)#interface mgmt-ethernet
<cr> Press ENTER to execute the command.
Switch (config)#interface mgmt-ethernet
Switch (config-if)#
Switch (config-if)#hostname ala-openlabs-40G
Switch (config-if)#ip address 147.11.91.8/22

Error! Management port configuration not valid!
Switch (config-if)#ip default-gateway 147.11.88.1
Switch (config-if)#show interface mgmt-ethernet

Interface ............................... eth0
Hostname ................................ ala-openlabs-40G
IP Address Mode ......................... Static
IP Address .............................. 147.11.91.8/22
Gateway ................................. 147.11.88.1
Administrative State .................... Up
MTU ..................................... 1500
Speed ................................... 1000
Duplex .................................. Full
Autonegotiation ......................... On

Switch#save config

Switch #vlan
Switch (vlan)#

xe1	: ala-core compute-00 east-west
   Switch (config)#interface xe1
   Switch (config-if xe1)#switchport vlan add 100-199 tagged
xe2	: ala-core compute-01 east-west
   Switch (config)#interface xe2
   Switch (config-if xe2)#switchport vlan add 100-199 tagged
xe3	: to SYMKLOUD 1 - 1/1/1.4
   Switch (config)#interface xe3
   Switch (config-if xe3)#no switchport vlan add 1 
   Switch (config-if xe3)#switchport pvid 20
   Switch (config-if xe3)#switchport vlan add 20 untagged
xe4	:
xe5	:
xe6	:
xe7	:
xe8	:
xe9	:
xe10	:
xe11	:
xe12	:
xe13	:
xe14	:
xe15	:
xe16	:
xe17	:
xe18	:
xe19	:
xe20	:
xe21	:
xe22	:
xe23	:
xe24	:
xe25	:
xe26	:
xe27	:
xe28	:
xe29	:
xe10	:
xe31	:
xe32	:
xe33	:
xe34	:
xe35	:
xe36	:
xe37	:
xe38	:
xe39	:
xe40	:
xe41	:
xe42	:
xe43	:
xe44	:
xe45	:
xe46	:
xe47	: Advantech PAC-6009
   Switch (config)#interface xe47
   Switch (config-if xe47)#no switchport vlan add 1 
   Switch (config-if xe47)#switchport pvid 20
   Switch (config-if xe47)#switchport vlan add 20 untagged

xe48	: ala-openlabs jump station
   Switch (config)#interface xe48
   Switch (config-if xe48)#switchport vlan add 1 untagged
   Switch (config-if xe48)#switchport vlan add 4-9 tagged

qe49	: Kontron SYMKLOUD 1
   Switch (config)#interface qe49
   Switch (config-if qe49)#switchport vlan add 1 untagged
   Switch (config-if qe49)#switchport vlan add 4-9 tagged
   Switch (config-if qe49)#switchport vlan add 100-199 tagged

qe53	: Kontron SYMKLOUD 2
   Kontron SYMKLOUD 2 switch TBD
qe57 	:
qe61	:
```



# R730
