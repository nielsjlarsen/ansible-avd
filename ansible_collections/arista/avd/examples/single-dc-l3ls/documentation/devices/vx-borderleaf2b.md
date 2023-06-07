# vx-borderleaf2b

## Table of Contents

- [Management](#management)
  - [Management Interfaces](#management-interfaces)
  - [DNS Domain](#dns-domain)
  - [IP Name Servers](#ip-name-servers)
  - [NTP](#ntp)
  - [Management API HTTP](#management-api-http)
- [Authentication](#authentication)
  - [Local Users](#local-users)
  - [TACACS Servers](#tacacs-servers)
  - [RADIUS Server](#radius-server)
  - [AAA Authentication](#aaa-authentication)
  - [AAA Authorization](#aaa-authorization)
  - [AAA Accounting](#aaa-accounting)
- [Monitoring](#monitoring)
  - [TerminAttr Daemon](#terminattr-daemon)
  - [SFlow](#sflow)
- [MLAG](#mlag)
  - [MLAG Summary](#mlag-summary)
  - [MLAG Device Configuration](#mlag-device-configuration)
- [Spanning Tree](#spanning-tree)
  - [Spanning Tree Summary](#spanning-tree-summary)
  - [Spanning Tree Device Configuration](#spanning-tree-device-configuration)
- [Internal VLAN Allocation Policy](#internal-vlan-allocation-policy)
  - [Internal VLAN Allocation Policy Summary](#internal-vlan-allocation-policy-summary)
  - [Internal VLAN Allocation Policy Configuration](#internal-vlan-allocation-policy-configuration)
- [VLANs](#vlans)
  - [VLANs Summary](#vlans-summary)
  - [VLANs Device Configuration](#vlans-device-configuration)
- [Interfaces](#interfaces)
  - [Ethernet Interfaces](#ethernet-interfaces)
  - [Port-Channel Interfaces](#port-channel-interfaces)
  - [Loopback Interfaces](#loopback-interfaces)
  - [VLAN Interfaces](#vlan-interfaces)
  - [VXLAN Interface](#vxlan-interface)
- [Routing](#routing)
  - [Service Routing Protocols Model](#service-routing-protocols-model)
  - [Virtual Router MAC Address](#virtual-router-mac-address)
  - [IP Routing](#ip-routing)
  - [IPv6 Routing](#ipv6-routing)
  - [Static Routes](#static-routes)
  - [Router BGP](#router-bgp)
- [BFD](#bfd)
  - [Router BFD](#router-bfd)
- [Multicast](#multicast)
  - [IP IGMP Snooping](#ip-igmp-snooping)
- [Filters](#filters)
  - [Prefix-lists](#prefix-lists)
  - [Route-maps](#route-maps)
- [VRF Instances](#vrf-instances)
  - [VRF Instances Summary](#vrf-instances-summary)
  - [VRF Instances Device Configuration](#vrf-instances-device-configuration)
- [EOS CLI](#eos-cli)

## Management

### Management Interfaces

#### Management Interfaces Summary

##### IPv4

| Management Interface | description | Type | VRF | IP Address | Gateway |
| -------------------- | ----------- | ---- | --- | ---------- | ------- |
| Management1 | oob_management | oob | MGMT | 10.151.11.22/26 | 10.151.11.1 |

##### IPv6

| Management Interface | description | Type | VRF | IPv6 Address | IPv6 Gateway |
| -------------------- | ----------- | ---- | --- | ------------ | ------------ |
| Management1 | oob_management | oob | MGMT | - | - |

#### Management Interfaces Device Configuration

```eos
!
interface Management1
   description oob_management
   no shutdown
   vrf MGMT
   ip address 10.151.11.22/26
```

### DNS Domain

#### DNS domain: sdn.lego.com

#### DNS Domain Device Configuration

```eos
dns domain sdn.lego.com
!
```

### IP Name Servers

#### IP Name Servers Summary

| Name Server | VRF | Priority |
| ----------- | --- | -------- |
| 10.148.30.200 | MGMT | - |
| 10.149.30.200 | MGMT | - |

#### IP Name Servers Device Configuration

```eos
ip name-server vrf MGMT 10.148.30.200
ip name-server vrf MGMT 10.149.30.200
```

### NTP

#### NTP Summary

##### NTP Local Interface

| Interface | VRF |
| --------- | --- |
| Management1 | MGMT |

##### NTP Servers

| Server | VRF | Preferred | Burst | iBurst | Version | Min Poll | Max Poll | Local-interface | Key |
| ------ | --- | --------- | ----- | ------ | ------- | -------- | -------- | --------------- | --- |
| utc1.corp.lego.com | MGMT | - | - | - | - | - | - | - | - |
| utc2.corp.lego.com | MGMT | - | - | - | - | - | - | - | - |
| utcd.corp.lego.com | MGMT | - | - | - | - | - | - | - | - |

#### NTP Device Configuration

```eos
!
ntp local-interface vrf MGMT Management1
ntp server vrf MGMT utc1.corp.lego.com
ntp server vrf MGMT utc2.corp.lego.com
ntp server vrf MGMT utcd.corp.lego.com
```

### Management API HTTP

#### Management API HTTP Summary

| HTTP | HTTPS | Default Services |
| ---- | ----- | ---------------- |
| False | True | - |

#### Management API VRF Access

| VRF Name | IPv4 ACL | IPv6 ACL |
| -------- | -------- | -------- |
| MGMT | - | - |

#### Management API HTTP Configuration

```eos
!
management api http-commands
   protocol https
   no shutdown
   !
   vrf MGMT
      no shutdown
```

## Authentication

### Local Users

#### Local Users Summary

| User | Privilege | Role | Disabled | Shell |
| ---- | --------- | ---- | -------- | ----- |
| netmanager | 15 | network-admin | False | - |

#### Local Users Device Configuration

```eos
!
username netmanager privilege 15 role network-admin secret sha512 <removed>
```

### TACACS Servers

#### TACACS Servers

| VRF | TACACS Servers | Single-Connection |
| --- | -------------- | ----------------- |
| MGMT | 10.148.56.50 | False |
| MGMT | 10.149.56.50 | False |
| MGMT | 10.150.56.50 | False |

#### TACACS Servers Device Configuration

```eos
!
tacacs-server host 10.148.56.50 vrf MGMT key 7 <removed>
tacacs-server host 10.149.56.50 vrf MGMT key 7 <removed>
tacacs-server host 10.150.56.50 vrf MGMT key 7 <removed>
```

### RADIUS Server

#### RADIUS Server Hosts

| VRF | RADIUS Servers | Timeout | Retransmit |
| --- | -------------- | ------- | ---------- |
| MGMT | 10.148.56.50 | - | - |
| MGMT | 10.149.56.50 | - | - |
| MGMT | 10.150.56.50 | - | - |

#### RADIUS Server Device Configuration

```eos
!
radius-server host 10.148.56.50 vrf MGMT key 7 <removed>
radius-server host 10.149.56.50 vrf MGMT key 7 <removed>
radius-server host 10.150.56.50 vrf MGMT key 7 <removed>
```

### AAA Authentication

#### AAA Authentication Summary

| Type | Sub-type | User Stores |
| ---- | -------- | ---------- |
| Login | default | group tacacs+ local |

#### AAA Authentication Device Configuration

```eos
aaa authentication login default group tacacs+ local
aaa authentication enable default group tacacs+ local
aaa authentication dot1x default group radius
!
```

### AAA Authorization

#### AAA Authorization Summary

| Type | User Stores |
| ---- | ----------- |
| Exec | group tacacs+ local |

Authorization for configuration commands is disabled.

#### AAA Authorization Privilege Levels Summary

| Privilege Level | User Stores |
| --------------- | ----------- |
| all | group tacacs+ local |

#### AAA Authorization Device Configuration

```eos
aaa authorization exec default group tacacs+ local
aaa authorization commands all default group tacacs+ local
!
```

### AAA Accounting

#### AAA Accounting Summary

| Type | Commands | Record type | Group | Logging |
| ---- | -------- | ----------- | ----- | ------- |
| Exec - Default | - | start-stop | tacacs+ | - |
| Dot1x - Default  | - | start-stop | radius | - |
| Commands - Default | 15 | start-stop | tacacs+ | False |

#### AAA Accounting Device Configuration

```eos
aaa accounting exec default start-stop group tacacs+
aaa accounting dot1x default start-stop group radius
aaa accounting commands 15 default start-stop group tacacs+
```

## Monitoring

### TerminAttr Daemon

#### TerminAttr Daemon Summary

| CV Compression | CloudVision Servers | VRF | Authentication | Smash Excludes | Ingest Exclude | Bypass AAA |
| -------------- | ------------------- | --- | -------------- | -------------- | -------------- | ---------- |
| gzip | apiserver.arista.io:443 | MGMT | token-secure,/tmp/cv-onboarding-token | ale,flexCounter,hardware,kni,pulse,strata | /Sysdb/cell/1/agent,/Sysdb/cell/2/agent | False |

#### TerminAttr Daemon Device Configuration

```eos
!
daemon TerminAttr
   exec /usr/bin/TerminAttr -cvaddr=apiserver.arista.io:443 -cvauth=token-secure,/tmp/cv-onboarding-token -cvvrf=MGMT -smashexcludes=ale,flexCounter,hardware,kni,pulse,strata -ingestexclude=/Sysdb/cell/1/agent,/Sysdb/cell/2/agent -taillogs
   no shutdown
```

### SFlow

#### SFlow Summary

| VRF | SFlow Source | SFlow Destination | Port |
| --- | ------------ | ----------------- | ---- |
| default | - | 127.0.0.1 | 6343 |

sFlow Sample Rate: 16384

sFlow Polling Interval: 10

sFlow is enabled.

#### SFlow Extensions

| Extension | Enabled |
| --------- | ------- |
| bgp | True |

#### SFlow Device Configuration

```eos
!
sflow sample 16384
sflow polling-interval 10
sflow destination 127.0.0.1
sflow extension bgp
sflow run
```

## MLAG

### MLAG Summary

| Domain-id | Local-interface | Peer-address | Peer-link |
| --------- | --------------- | ------------ | --------- |
| BORDERLEAF2 | Vlan4094 | 10.100.54.100 | Port-Channel491 |

Dual primary detection is disabled.

### MLAG Device Configuration

```eos
!
mlag configuration
   domain-id BORDERLEAF2
   local-interface Vlan4094
   peer-address 10.100.54.100
   peer-link Port-Channel491
   reload-delay mlag 300
   reload-delay non-mlag 330
```

## Spanning Tree

### Spanning Tree Summary

STP mode: **mstp**

#### MSTP Instance and Priority

| Instance(s) | Priority |
| -------- | -------- |
| 0 | 4096 |

#### Global Spanning-Tree Settings

- Spanning Tree disabled for VLANs: **4093-4094**

### Spanning Tree Device Configuration

```eos
!
spanning-tree mode mstp
no spanning-tree vlan-id 4093-4094
spanning-tree mst 0 priority 4096
```

## Internal VLAN Allocation Policy

### Internal VLAN Allocation Policy Summary

| Policy Allocation | Range Beginning | Range Ending |
| ------------------| --------------- | ------------ |
| ascending | 1006 | 1199 |

### Internal VLAN Allocation Policy Configuration

```eos
!
vlan internal order ascending range 1006 1199
```

## VLANs

### VLANs Summary

| VLAN ID | Name | Trunk Groups |
| ------- | ---- | ------------ |
| 11 | VLAN11-Firewall_Linknet-borderleaf | - |
| 1213 | mobile-prod-device | - |
| 1250 | enterprisenet | - |
| 4093 | LEAF_PEER_L3 | LEAF_PEER_L3 |
| 4094 | MLAG_PEER | MLAG |

### VLANs Device Configuration

```eos
!
vlan 11
   name VLAN11-Firewall_Linknet-borderleaf
!
vlan 1213
   name mobile-prod-device
!
vlan 1250
   name enterprisenet
!
vlan 4093
   name LEAF_PEER_L3
   trunk group LEAF_PEER_L3
!
vlan 4094
   name MLAG_PEER
   trunk group MLAG
```

## Interfaces

### Ethernet Interfaces

#### Ethernet Interfaces Summary

##### L2

| Interface | Description | Mode | VLANs | Native VLAN | Trunk Group | Channel-Group |
| --------- | ----------- | ---- | ----- | ----------- | ----------- | ------------- |
| Ethernet48 | palo2_Ethernet1/12 | *trunk | *11,1213,1250 | *- | *- | 48 |
| Ethernet49/1 | MLAG_PEER_vx-borderleaf2a_Ethernet49/1 | *trunk | *- | *- | *['LEAF_PEER_L3', 'MLAG'] | 491 |
| Ethernet50/1 | MLAG_PEER_vx-borderleaf2a_Ethernet50/1 | *trunk | *- | *- | *['LEAF_PEER_L3', 'MLAG'] | 491 |

*Inherited from Port-Channel Interface

##### IPv4

| Interface | Description | Type | Channel Group | IP Address | VRF |  MTU | Shutdown | ACL In | ACL Out |
| --------- | ----------- | -----| ------------- | ---------- | ----| ---- | -------- | ------ | ------- |
| Ethernet1 | P2P_LINK_TO_VX-SPINE1_Ethernet4 | routed | - | 10.100.54.45/31 | default | 9214 | False | - | - |
| Ethernet2 | P2P_LINK_TO_VX-SPINE2_Ethernet4 | routed | - | 10.100.54.47/31 | default | 9214 | False | - | - |

#### Ethernet Interfaces Device Configuration

```eos
!
interface Ethernet1
   description P2P_LINK_TO_VX-SPINE1_Ethernet4
   no shutdown
   mtu 9214
   no switchport
   ip address 10.100.54.45/31
!
interface Ethernet2
   description P2P_LINK_TO_VX-SPINE2_Ethernet4
   no shutdown
   mtu 9214
   no switchport
   ip address 10.100.54.47/31
!
interface Ethernet48
   description palo2_Ethernet1/12
   no shutdown
   channel-group 48 mode active
!
interface Ethernet49/1
   description MLAG_PEER_vx-borderleaf2a_Ethernet49/1
   no shutdown
   channel-group 491 mode active
!
interface Ethernet50/1
   description MLAG_PEER_vx-borderleaf2a_Ethernet50/1
   no shutdown
   channel-group 491 mode active
```

### Port-Channel Interfaces

#### Port-Channel Interfaces Summary

##### L2

| Interface | Description | Type | Mode | VLANs | Native VLAN | Trunk Group | LACP Fallback Timeout | LACP Fallback Mode | MLAG ID | EVPN ESI |
| --------- | ----------- | ---- | ---- | ----- | ----------- | ------------| --------------------- | ------------------ | ------- | -------- |
| Port-Channel48 | palo2_PortChannel palo2 | switched | trunk | 11,1213,1250 | - | - | - | - | 48 | - |
| Port-Channel491 | MLAG_PEER_vx-borderleaf2a_Po491 | switched | trunk | - | - | ['LEAF_PEER_L3', 'MLAG'] | - | - | - | - |

#### Port-Channel Interfaces Device Configuration

```eos
!
interface Port-Channel48
   description palo2_PortChannel palo2
   no shutdown
   switchport
   switchport trunk allowed vlan 11,1213,1250
   switchport mode trunk
   mlag 48
   spanning-tree portfast
!
interface Port-Channel491
   description MLAG_PEER_vx-borderleaf2a_Po491
   no shutdown
   switchport
   switchport mode trunk
   switchport trunk group LEAF_PEER_L3
   switchport trunk group MLAG
```

### Loopback Interfaces

#### Loopback Interfaces Summary

##### IPv4

| Interface | Description | VRF | IP Address |
| --------- | ----------- | --- | ---------- |
| Loopback0 | EVPN_Overlay_Peering | default | 10.100.160.6/32 |
| Loopback1 | VTEP_VXLAN_Tunnel_Source | default | 10.100.54.21/32 |

##### IPv6

| Interface | Description | VRF | IPv6 Address |
| --------- | ----------- | --- | ------------ |
| Loopback0 | EVPN_Overlay_Peering | default | - |
| Loopback1 | VTEP_VXLAN_Tunnel_Source | default | - |


#### Loopback Interfaces Device Configuration

```eos
!
interface Loopback0
   description EVPN_Overlay_Peering
   no shutdown
   ip address 10.100.160.6/32
!
interface Loopback1
   description VTEP_VXLAN_Tunnel_Source
   no shutdown
   ip address 10.100.54.21/32
```

### VLAN Interfaces

#### VLAN Interfaces Summary

| Interface | Description | VRF |  MTU | Shutdown |
| --------- | ----------- | --- | ---- | -------- |
| Vlan11 | VLAN11-Firewall_Linknet-borderleaf | default | 9214 | False |
| Vlan4093 | MLAG_PEER_L3_PEERING | default | 9214 | False |
| Vlan4094 | MLAG_PEER | default | 9214 | False |

##### IPv4

| Interface | VRF | IP Address | IP Address Virtual | IP Router Virtual Address | VRRP | ACL In | ACL Out |
| --------- | --- | ---------- | ------------------ | ------------------------- | ---- | ------ | ------- |
| Vlan11 |  default  |  -  |  10.100.54.226/27  |  -  |  -  |  -  |  -  |
| Vlan4093 |  default  |  10.100.54.117/31  |  -  |  -  |  -  |  -  |  -  |
| Vlan4094 |  default  |  10.100.54.101/31  |  -  |  -  |  -  |  -  |  -  |

#### VLAN Interfaces Device Configuration

```eos
!
interface Vlan11
   description VLAN11-Firewall_Linknet-borderleaf
   no shutdown
   mtu 9214
   ip address virtual 10.100.54.226/27
!
interface Vlan4093
   description MLAG_PEER_L3_PEERING
   no shutdown
   mtu 9214
   ip address 10.100.54.117/31
!
interface Vlan4094
   description MLAG_PEER
   no shutdown
   mtu 9214
   no autostate
   ip address 10.100.54.101/31
```

### VXLAN Interface

#### VXLAN Interface Summary

| Setting | Value |
| ------- | ----- |
| Source Interface | Loopback1 |
| UDP port | 4789 |
| EVPN MLAG Shared Router MAC | mlag-system-id |
| VXLAN flood-lists learning from data-plane | Enabled |

##### VLAN to VNI, Flood List and Multicast Group Mappings

| VLAN | VNI | Flood List | Multicast Group |
| ---- | --- | ---------- | --------------- |
| 11 | 11 | - | - |
| 1213 | 1213 | - | - |
| 1250 | 1250 | - | - |

##### VRF to VNI and Multicast Group Mappings

| VRF | VNI | Multicast Group |
| ---- | --- | --------------- |
| default | 1010 | - |

#### VXLAN Interface Device Configuration

```eos
!
interface Vxlan1
   description vx-borderleaf2b_VTEP
   vxlan source-interface Loopback1
   vxlan virtual-router encapsulation mac-address mlag-system-id
   vxlan udp-port 4789
   vxlan flood vtep learned data-plane
   vxlan vlan 11 vni 11
   vxlan vlan 1213 vni 1213
   vxlan vlan 1250 vni 1250
   vxlan vrf default vni 1010
   vxlan bridging vtep-to-vtep source-vtep tx disabled 10.100.54.19
```

## Routing

### Service Routing Protocols Model

Multi agent routing protocol model enabled

```eos
!
service routing protocols model multi-agent
```

### Virtual Router MAC Address

#### Virtual Router MAC Address Summary

##### Virtual Router MAC Address: 00:1c:73:00:00:99

#### Virtual Router MAC Address Configuration

```eos
!
ip virtual-router mac-address 00:1c:73:00:00:99
```

### IP Routing

#### IP Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | True |
| MGMT | False |

#### IP Routing Device Configuration

```eos
!
ip routing
no ip routing vrf MGMT
```

### IPv6 Routing

#### IPv6 Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | False |
| MGMT | false |

### Static Routes

#### Static Routes Summary

| VRF | Destination Prefix | Next Hop IP             | Exit interface      | Administrative Distance       | Tag               | Route Name                    | Metric         |
| --- | ------------------ | ----------------------- | ------------------- | ----------------------------- | ----------------- | ----------------------------- | -------------- |
| MGMT | 0.0.0.0/0 | 10.151.11.1 | - | 1 | - | - | - |
| default | 0.0.0.0/0 | 10.100.54.225 | - | 1 | - | - | - |

#### Static Routes Device Configuration

```eos
!
ip route vrf MGMT 0.0.0.0/0 10.151.11.1
ip route 0.0.0.0/0 10.100.54.225
```

### Router BGP

#### Router BGP Summary

| BGP AS | Router ID |
| ------ | --------- |
| 65102|  10.100.160.6 |

| BGP Tuning |
| ---------- |
| graceful-restart restart-time 300 |
| graceful-restart |
| update wait-install |
| no bgp default ipv4-unicast |
| maximum-paths 4 ecmp 4 |

#### Router BGP Peer Groups

##### EVPN-OVERLAY-PEERS

| Settings | Value |
| -------- | ----- |
| Address Family | evpn |
| Source | Loopback0 |
| BFD | True |
| Ebgp multihop | 3 |
| Send community | all |
| Maximum routes | 0 (no limit) |

##### IPv4-UNDERLAY-PEERS

| Settings | Value |
| -------- | ----- |
| Address Family | ipv4 |
| Send community | all |
| Maximum routes | 12000 |

##### MLAG-IPv4-UNDERLAY-PEER

| Settings | Value |
| -------- | ----- |
| Address Family | ipv4 |
| Remote AS | 65102 |
| Next-hop self | True |
| Send community | all |
| Maximum routes | 12000 |

#### BGP Neighbors

| Neighbor | Remote AS | VRF | Shutdown | Send-community | Maximum-routes | Allowas-in | BFD | RIB Pre-Policy Retain | Route-Reflector Client | Passive |
| -------- | --------- | --- | -------- | -------------- | -------------- | ---------- | --- | --------------------- | ---------------------- | ------- |
| 10.100.54.44 | 65100 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - |
| 10.100.54.46 | 65100 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - |
| 10.100.54.116 | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | default | - | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | - | - | - | - | - |
| 10.100.160.1 | 65100 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - |
| 10.100.160.2 | 65100 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - |

#### Router BGP EVPN Address Family

##### EVPN Peer Groups

| Peer Group | Activate | Encapsulation |
| ---------- | -------- | ------------- |
| EVPN-OVERLAY-PEERS | True | default |

#### Router BGP VLANs

| VLAN | Route-Distinguisher | Both Route-Target | Import Route Target | Export Route-Target | Redistribute |
| ---- | ------------------- | ----------------- | ------------------- | ------------------- | ------------ |
| 11 | 10.100.160.6:11 | 11:11 | - | - | learned |
| 1213 | 10.100.160.6:1213 | 1213:1213 | - | - | learned |
| 1250 | 10.100.160.6:1250 | 1250:1250 | - | - | learned |

#### Router BGP VRFs

| VRF | Route-Distinguisher | Redistribute |
| --- | ------------------- | ------------ |
| default | - | static |

#### Router BGP Device Configuration

```eos
!
router bgp 65102
   router-id 10.100.160.6
   graceful-restart restart-time 300
   graceful-restart
   maximum-paths 4 ecmp 4
   update wait-install
   no bgp default ipv4-unicast
   neighbor EVPN-OVERLAY-PEERS peer group
   neighbor EVPN-OVERLAY-PEERS update-source Loopback0
   neighbor EVPN-OVERLAY-PEERS bfd
   neighbor EVPN-OVERLAY-PEERS ebgp-multihop 3
   neighbor EVPN-OVERLAY-PEERS password 7 <removed>
   neighbor EVPN-OVERLAY-PEERS send-community
   neighbor EVPN-OVERLAY-PEERS maximum-routes 0
   neighbor IPv4-UNDERLAY-PEERS peer group
   neighbor IPv4-UNDERLAY-PEERS password 7 <removed>
   neighbor IPv4-UNDERLAY-PEERS send-community
   neighbor IPv4-UNDERLAY-PEERS maximum-routes 12000
   neighbor IPv4-UNDERLAY-PEERS route-map RM-BGP-UNDERLAY-PEERS-OUT out
   neighbor MLAG-IPv4-UNDERLAY-PEER peer group
   neighbor MLAG-IPv4-UNDERLAY-PEER remote-as 65102
   neighbor MLAG-IPv4-UNDERLAY-PEER next-hop-self
   neighbor MLAG-IPv4-UNDERLAY-PEER description vx-borderleaf2a
   neighbor MLAG-IPv4-UNDERLAY-PEER password 7 <removed>
   neighbor MLAG-IPv4-UNDERLAY-PEER send-community
   neighbor MLAG-IPv4-UNDERLAY-PEER maximum-routes 12000
   neighbor MLAG-IPv4-UNDERLAY-PEER route-map RM-MLAG-PEER-IN in
   neighbor 10.100.54.44 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.100.54.44 remote-as 65100
   neighbor 10.100.54.44 description vx-spine1_Ethernet4
   neighbor 10.100.54.46 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.100.54.46 remote-as 65100
   neighbor 10.100.54.46 description vx-spine2_Ethernet4
   neighbor 10.100.54.116 peer group MLAG-IPv4-UNDERLAY-PEER
   neighbor 10.100.54.116 description vx-borderleaf2a
   neighbor 10.100.160.1 peer group EVPN-OVERLAY-PEERS
   neighbor 10.100.160.1 remote-as 65100
   neighbor 10.100.160.1 description vx-spine1
   neighbor 10.100.160.2 peer group EVPN-OVERLAY-PEERS
   neighbor 10.100.160.2 remote-as 65100
   neighbor 10.100.160.2 description vx-spine2
   redistribute connected route-map RM-CONN-2-BGP
   !
   vlan 11
      rd 10.100.160.6:11
      route-target both 11:11
      redistribute learned
   !
   vlan 1213
      rd 10.100.160.6:1213
      route-target both 1213:1213
      redistribute learned
   !
   vlan 1250
      rd 10.100.160.6:1250
      route-target both 1250:1250
      redistribute learned
   !
   address-family evpn
      neighbor EVPN-OVERLAY-PEERS activate
   !
   address-family ipv4
      no neighbor EVPN-OVERLAY-PEERS activate
      neighbor IPv4-UNDERLAY-PEERS activate
      neighbor MLAG-IPv4-UNDERLAY-PEER activate
   !
   vrf default
      redistribute static
```

## BFD

### Router BFD

#### Router BFD Multihop Summary

| Interval | Minimum RX | Multiplier |
| -------- | ---------- | ---------- |
| 300 | 300 | 3 |

#### Router BFD Device Configuration

```eos
!
router bfd
   multihop interval 300 min-rx 300 multiplier 3
```

## Multicast

### IP IGMP Snooping

#### IP IGMP Snooping Summary

| IGMP Snooping | Fast Leave | Interface Restart Query | Proxy | Restart Query Interval | Robustness Variable |
| ------------- | ---------- | ----------------------- | ----- | ---------------------- | ------------------- |
| Enabled | - | - | - | - | - |

#### IP IGMP Snooping Device Configuration

```eos
```

## Filters

### Prefix-lists

#### Prefix-lists Summary

##### PL-LOOPBACKS-EVPN-OVERLAY

| Sequence | Action |
| -------- | ------ |
| 10 | permit 10.100.160.0/27 eq 32 |
| 20 | permit 10.100.54.16/28 eq 32 |

##### PL-SVI-VRF-DEFAULT

| Sequence | Action |
| -------- | ------ |
| 10 | permit 10.100.54.224/27 |

#### Prefix-lists Device Configuration

```eos
!
ip prefix-list PL-LOOPBACKS-EVPN-OVERLAY
   seq 10 permit 10.100.160.0/27 eq 32
   seq 20 permit 10.100.54.16/28 eq 32
!
ip prefix-list PL-SVI-VRF-DEFAULT
   seq 10 permit 10.100.54.224/27
```

### Route-maps

#### Route-maps Summary

##### RM-BGP-UNDERLAY-PEERS-OUT

| Sequence | Type | Match | Set | Sub-Route-Map | Continue |
| -------- | ---- | ----- | --- | ------------- | -------- |
| 10 | deny | ip address prefix-list PL-SVI-VRF-DEFAULT | - | - | - |
| 20 | permit | - | - | - | - |

##### RM-CONN-2-BGP

| Sequence | Type | Match | Set | Sub-Route-Map | Continue |
| -------- | ---- | ----- | --- | ------------- | -------- |
| 10 | permit | ip address prefix-list PL-LOOPBACKS-EVPN-OVERLAY | - | - | - |
| 30 | permit | ip address prefix-list PL-SVI-VRF-DEFAULT | - | - | - |

##### RM-EVPN-EXPORT-VRF-DEFAULT

| Sequence | Type | Match | Set | Sub-Route-Map | Continue |
| -------- | ---- | ----- | --- | ------------- | -------- |
| 10 | permit | ip address prefix-list PL-SVI-VRF-DEFAULT | - | - | - |

##### RM-MLAG-PEER-IN

| Sequence | Type | Match | Set | Sub-Route-Map | Continue |
| -------- | ---- | ----- | --- | ------------- | -------- |
| 10 | permit | - | origin incomplete | - | - |

#### Route-maps Device Configuration

```eos
!
route-map RM-BGP-UNDERLAY-PEERS-OUT deny 10
   match ip address prefix-list PL-SVI-VRF-DEFAULT
!
route-map RM-BGP-UNDERLAY-PEERS-OUT permit 20
!
route-map RM-CONN-2-BGP permit 10
   match ip address prefix-list PL-LOOPBACKS-EVPN-OVERLAY
!
route-map RM-CONN-2-BGP permit 30
   match ip address prefix-list PL-SVI-VRF-DEFAULT
!
route-map RM-EVPN-EXPORT-VRF-DEFAULT permit 10
   match ip address prefix-list PL-SVI-VRF-DEFAULT
!
route-map RM-MLAG-PEER-IN permit 10
   description Make routes learned over MLAG Peer-link less preferred on spines to ensure optimal routing
   set origin incomplete
```

## VRF Instances

### VRF Instances Summary

| VRF Name | IP Routing |
| -------- | ---------- |
| MGMT | disabled |

### VRF Instances Device Configuration

```eos
!
vrf instance MGMT
```

## EOS CLI

```eos
!
router bgp 65102
  no neighbor IPv4-UNDERLAY-PEERS route-map RM-BGP-UNDERLAY-PEERS-OUT out
  no vlan 11
  no vlan 1213
  no vlan 1250
interface Vxlan1
  no vxlan vlan 11 vni 11
  no vxlan vrf default vni 1010
interface ethernet45-46
  description Borderleaf cross-site trunk
  channel-group 45 mode active
  no shut
interface Port-Channel45
  description Borderleaf cross-site trunk
  switchport
  switchport trunk allowed vlan 11,1213,1250
  switchport mode trunk
  mlag 45
```
