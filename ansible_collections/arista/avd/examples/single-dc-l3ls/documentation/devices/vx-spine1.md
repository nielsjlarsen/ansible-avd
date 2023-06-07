# vx-spine1

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
- [Spanning Tree](#spanning-tree)
  - [Spanning Tree Summary](#spanning-tree-summary)
  - [Spanning Tree Device Configuration](#spanning-tree-device-configuration)
- [Internal VLAN Allocation Policy](#internal-vlan-allocation-policy)
  - [Internal VLAN Allocation Policy Summary](#internal-vlan-allocation-policy-summary)
  - [Internal VLAN Allocation Policy Configuration](#internal-vlan-allocation-policy-configuration)
- [Interfaces](#interfaces)
  - [Ethernet Interfaces](#ethernet-interfaces)
  - [Loopback Interfaces](#loopback-interfaces)
- [Routing](#routing)
  - [Service Routing Protocols Model](#service-routing-protocols-model)
  - [IP Routing](#ip-routing)
  - [IPv6 Routing](#ipv6-routing)
  - [Static Routes](#static-routes)
  - [Router BGP](#router-bgp)
- [BFD](#bfd)
  - [Router BFD](#router-bfd)
- [Filters](#filters)
  - [Prefix-lists](#prefix-lists)
  - [Route-maps](#route-maps)
- [VRF Instances](#vrf-instances)
  - [VRF Instances Summary](#vrf-instances-summary)
  - [VRF Instances Device Configuration](#vrf-instances-device-configuration)

## Management

### Management Interfaces

#### Management Interfaces Summary

##### IPv4

| Management Interface | description | Type | VRF | IP Address | Gateway |
| -------------------- | ----------- | ---- | --- | ---------- | ------- |
| Management1 | oob_management | oob | MGMT | 10.151.11.17/26 | 10.151.11.1 |

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
   ip address 10.151.11.17/26
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

## Spanning Tree

### Spanning Tree Summary

STP mode: **none**

### Spanning Tree Device Configuration

```eos
!
spanning-tree mode none
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

## Interfaces

### Ethernet Interfaces

#### Ethernet Interfaces Summary

##### L2

| Interface | Description | Mode | VLANs | Native VLAN | Trunk Group | Channel-Group |
| --------- | ----------- | ---- | ----- | ----------- | ----------- | ------------- |

*Inherited from Port-Channel Interface

##### IPv4

| Interface | Description | Type | Channel Group | IP Address | VRF |  MTU | Shutdown | ACL In | ACL Out |
| --------- | ----------- | -----| ------------- | ---------- | ----| ---- | -------- | ------ | ------- |
| Ethernet1 | P2P_LINK_TO_VX-ACCESSLEAF3A_Ethernet1 | routed | - | 10.100.54.48/31 | default | 9214 | False | - | - |
| Ethernet2 | P2P_LINK_TO_VX-ACCESSLEAF3B_Ethernet1 | routed | - | 10.100.54.52/31 | default | 9214 | False | - | - |
| Ethernet3 | P2P_LINK_TO_VX-BORDERLEAF2A_Ethernet1 | routed | - | 10.100.54.40/31 | default | 9214 | False | - | - |
| Ethernet4 | P2P_LINK_TO_VX-BORDERLEAF2B_Ethernet1 | routed | - | 10.100.54.44/31 | default | 9214 | False | - | - |
| Ethernet5 | P2P_LINK_TO_VX-BORDERLEAF1A_Ethernet1 | routed | - | 10.100.54.32/31 | default | 9214 | False | - | - |
| Ethernet6 | P2P_LINK_TO_VX-BORDERLEAF1B_Ethernet1 | routed | - | 10.100.54.36/31 | default | 9214 | False | - | - |
| Ethernet7 | P2P_LINK_TO_VX-WIFI1A_Ethernet27 | routed | - | 10.100.54.56/31 | default | 9214 | False | - | - |
| Ethernet8 | P2P_LINK_TO_VX-WIFI1B_Ethernet27 | routed | - | 10.100.54.60/31 | default | 9214 | False | - | - |

#### Ethernet Interfaces Device Configuration

```eos
!
interface Ethernet1
   description P2P_LINK_TO_VX-ACCESSLEAF3A_Ethernet1
   no shutdown
   mtu 9214
   no switchport
   ip address 10.100.54.48/31
!
interface Ethernet2
   description P2P_LINK_TO_VX-ACCESSLEAF3B_Ethernet1
   no shutdown
   mtu 9214
   no switchport
   ip address 10.100.54.52/31
!
interface Ethernet3
   description P2P_LINK_TO_VX-BORDERLEAF2A_Ethernet1
   no shutdown
   mtu 9214
   no switchport
   ip address 10.100.54.40/31
!
interface Ethernet4
   description P2P_LINK_TO_VX-BORDERLEAF2B_Ethernet1
   no shutdown
   mtu 9214
   no switchport
   ip address 10.100.54.44/31
!
interface Ethernet5
   description P2P_LINK_TO_VX-BORDERLEAF1A_Ethernet1
   no shutdown
   mtu 9214
   no switchport
   ip address 10.100.54.32/31
!
interface Ethernet6
   description P2P_LINK_TO_VX-BORDERLEAF1B_Ethernet1
   no shutdown
   mtu 9214
   no switchport
   ip address 10.100.54.36/31
!
interface Ethernet7
   description P2P_LINK_TO_VX-WIFI1A_Ethernet27
   no shutdown
   mtu 9214
   no switchport
   ip address 10.100.54.56/31
!
interface Ethernet8
   description P2P_LINK_TO_VX-WIFI1B_Ethernet27
   no shutdown
   mtu 9214
   no switchport
   ip address 10.100.54.60/31
```

### Loopback Interfaces

#### Loopback Interfaces Summary

##### IPv4

| Interface | Description | VRF | IP Address |
| --------- | ----------- | --- | ---------- |
| Loopback0 | EVPN_Overlay_Peering | default | 10.100.160.1/32 |

##### IPv6

| Interface | Description | VRF | IPv6 Address |
| --------- | ----------- | --- | ------------ |
| Loopback0 | EVPN_Overlay_Peering | default | - |


#### Loopback Interfaces Device Configuration

```eos
!
interface Loopback0
   description EVPN_Overlay_Peering
   no shutdown
   ip address 10.100.160.1/32
```

## Routing

### Service Routing Protocols Model

Multi agent routing protocol model enabled

```eos
!
service routing protocols model multi-agent
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

#### Static Routes Device Configuration

```eos
!
ip route vrf MGMT 0.0.0.0/0 10.151.11.1
```

### Router BGP

#### Router BGP Summary

| BGP AS | Router ID |
| ------ | --------- |
| 65100|  10.100.160.1 |

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
| Next-hop unchanged | True |
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

#### BGP Neighbors

| Neighbor | Remote AS | VRF | Shutdown | Send-community | Maximum-routes | Allowas-in | BFD | RIB Pre-Policy Retain | Route-Reflector Client | Passive |
| -------- | --------- | --- | -------- | -------------- | -------------- | ---------- | --- | --------------------- | ---------------------- | ------- |
| 10.100.54.33 | 65101 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - |
| 10.100.54.37 | 65101 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - |
| 10.100.54.41 | 65102 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - |
| 10.100.54.45 | 65102 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - |
| 10.100.54.49 | 65103 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - |
| 10.100.54.53 | 65103 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - |
| 10.100.54.57 | 65104 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - |
| 10.100.54.61 | 65104 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - |
| 10.100.160.3 | 65101 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - |
| 10.100.160.4 | 65101 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - |
| 10.100.160.5 | 65102 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - |
| 10.100.160.6 | 65102 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - |
| 10.100.160.7 | 65103 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - |
| 10.100.160.8 | 65103 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - |
| 10.100.160.9 | 65104 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - |
| 10.100.160.10 | 65104 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - |

#### Router BGP EVPN Address Family

##### EVPN Peer Groups

| Peer Group | Activate | Encapsulation |
| ---------- | -------- | ------------- |
| EVPN-OVERLAY-PEERS | True | default |

#### Router BGP Device Configuration

```eos
!
router bgp 65100
   router-id 10.100.160.1
   graceful-restart restart-time 300
   graceful-restart
   maximum-paths 4 ecmp 4
   update wait-install
   no bgp default ipv4-unicast
   neighbor EVPN-OVERLAY-PEERS peer group
   neighbor EVPN-OVERLAY-PEERS next-hop-unchanged
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
   neighbor 10.100.54.33 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.100.54.33 remote-as 65101
   neighbor 10.100.54.33 description vx-borderleaf1a_Ethernet1
   neighbor 10.100.54.37 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.100.54.37 remote-as 65101
   neighbor 10.100.54.37 description vx-borderleaf1b_Ethernet1
   neighbor 10.100.54.41 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.100.54.41 remote-as 65102
   neighbor 10.100.54.41 description vx-borderleaf2a_Ethernet1
   neighbor 10.100.54.45 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.100.54.45 remote-as 65102
   neighbor 10.100.54.45 description vx-borderleaf2b_Ethernet1
   neighbor 10.100.54.49 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.100.54.49 remote-as 65103
   neighbor 10.100.54.49 description vx-accessleaf3a_Ethernet1
   neighbor 10.100.54.53 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.100.54.53 remote-as 65103
   neighbor 10.100.54.53 description vx-accessleaf3b_Ethernet1
   neighbor 10.100.54.57 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.100.54.57 remote-as 65104
   neighbor 10.100.54.57 description vx-wifi1a_Ethernet27
   neighbor 10.100.54.61 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.100.54.61 remote-as 65104
   neighbor 10.100.54.61 description vx-wifi1b_Ethernet27
   neighbor 10.100.160.3 peer group EVPN-OVERLAY-PEERS
   neighbor 10.100.160.3 remote-as 65101
   neighbor 10.100.160.3 description vx-borderleaf1a
   neighbor 10.100.160.4 peer group EVPN-OVERLAY-PEERS
   neighbor 10.100.160.4 remote-as 65101
   neighbor 10.100.160.4 description vx-borderleaf1b
   neighbor 10.100.160.5 peer group EVPN-OVERLAY-PEERS
   neighbor 10.100.160.5 remote-as 65102
   neighbor 10.100.160.5 description vx-borderleaf2a
   neighbor 10.100.160.6 peer group EVPN-OVERLAY-PEERS
   neighbor 10.100.160.6 remote-as 65102
   neighbor 10.100.160.6 description vx-borderleaf2b
   neighbor 10.100.160.7 peer group EVPN-OVERLAY-PEERS
   neighbor 10.100.160.7 remote-as 65103
   neighbor 10.100.160.7 description vx-accessleaf3a
   neighbor 10.100.160.8 peer group EVPN-OVERLAY-PEERS
   neighbor 10.100.160.8 remote-as 65103
   neighbor 10.100.160.8 description vx-accessleaf3b
   neighbor 10.100.160.9 peer group EVPN-OVERLAY-PEERS
   neighbor 10.100.160.9 remote-as 65104
   neighbor 10.100.160.9 description vx-wifi1a
   neighbor 10.100.160.10 peer group EVPN-OVERLAY-PEERS
   neighbor 10.100.160.10 remote-as 65104
   neighbor 10.100.160.10 description vx-wifi1b
   redistribute connected route-map RM-CONN-2-BGP
   !
   address-family evpn
      neighbor EVPN-OVERLAY-PEERS activate
   !
   address-family ipv4
      no neighbor EVPN-OVERLAY-PEERS activate
      neighbor IPv4-UNDERLAY-PEERS activate
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

## Filters

### Prefix-lists

#### Prefix-lists Summary

##### PL-LOOPBACKS-EVPN-OVERLAY

| Sequence | Action |
| -------- | ------ |
| 10 | permit 10.100.160.0/27 eq 32 |

#### Prefix-lists Device Configuration

```eos
!
ip prefix-list PL-LOOPBACKS-EVPN-OVERLAY
   seq 10 permit 10.100.160.0/27 eq 32
```

### Route-maps

#### Route-maps Summary

##### RM-CONN-2-BGP

| Sequence | Type | Match | Set | Sub-Route-Map | Continue |
| -------- | ---- | ----- | --- | ------------- | -------- |
| 10 | permit | ip address prefix-list PL-LOOPBACKS-EVPN-OVERLAY | - | - | - |

#### Route-maps Device Configuration

```eos
!
route-map RM-CONN-2-BGP permit 10
   match ip address prefix-list PL-LOOPBACKS-EVPN-OVERLAY
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