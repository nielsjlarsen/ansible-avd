# vx-subleaf5a

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
  - [VLAN Interfaces](#vlan-interfaces)
- [Routing](#routing)
  - [Service Routing Protocols Model](#service-routing-protocols-model)
  - [IP Routing](#ip-routing)
  - [IPv6 Routing](#ipv6-routing)
  - [Static Routes](#static-routes)
- [Multicast](#multicast)
  - [IP IGMP Snooping](#ip-igmp-snooping)
- [Power Over Ethernet (PoE)](#power-over-ethernet-poe)
  - [PoE Summary](#poe-summary)
  - [PoE Device Configuration](#poe-device-configuration)
- [VRF Instances](#vrf-instances)
  - [VRF Instances Summary](#vrf-instances-summary)
  - [VRF Instances Device Configuration](#vrf-instances-device-configuration)

## Management

### Management Interfaces

#### Management Interfaces Summary

##### IPv4

| Management Interface | description | Type | VRF | IP Address | Gateway |
| -------------------- | ----------- | ---- | --- | ---------- | ------- |
| Management1/1 | oob_management | oob | MGMT | 10.151.11.12/26 | 10.151.11.1 |

##### IPv6

| Management Interface | description | Type | VRF | IPv6 Address | IPv6 Gateway |
| -------------------- | ----------- | ---- | --- | ------------ | ------------ |
| Management1/1 | oob_management | oob | MGMT | - | - |

#### Management Interfaces Device Configuration

```eos
!
interface Management1/1
   description oob_management
   no shutdown
   vrf MGMT
   ip address 10.151.11.12/26
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
| Management1/1 | MGMT |

##### NTP Servers

| Server | VRF | Preferred | Burst | iBurst | Version | Min Poll | Max Poll | Local-interface | Key |
| ------ | --- | --------- | ----- | ------ | ------- | -------- | -------- | --------------- | --- |
| utc1.corp.lego.com | MGMT | - | - | - | - | - | - | - | - |
| utc2.corp.lego.com | MGMT | - | - | - | - | - | - | - | - |
| utcd.corp.lego.com | MGMT | - | - | - | - | - | - | - | - |

#### NTP Device Configuration

```eos
!
ntp local-interface vrf MGMT Management1/1
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
| VX-SUBLEAF5 | Vlan4094 | 10.100.54.133 | Port-Channel13 |

Dual primary detection is disabled.

### MLAG Device Configuration

```eos
!
mlag configuration
   domain-id VX-SUBLEAF5
   local-interface Vlan4094
   peer-address 10.100.54.133
   peer-link Port-Channel13
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

- Spanning Tree disabled for VLANs: **4094**

### Spanning Tree Device Configuration

```eos
!
spanning-tree mode mstp
no spanning-tree vlan-id 4094
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
| 10 | VRF10_VLAN10 | - |
| 1202 | mgmt-accesspoint | - |
| 1213 | mobile-prod-device | - |
| 1250 | enterprisenet | - |
| 4094 | MLAG_PEER | MLAG |

### VLANs Device Configuration

```eos
!
vlan 10
   name VRF10_VLAN10
!
vlan 1202
   name mgmt-accesspoint
!
vlan 1213
   name mobile-prod-device
!
vlan 1250
   name enterprisenet
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
| Ethernet1/1 | VX-ACCESSLEAF3A_Ethernet46 | *trunk | *10,1202,1213,1250 | *- | *- | 11 |
| Ethernet1/2 | VX-ACCESSLEAF3B_Ethernet46 | *trunk | *10,1202,1213,1250 | *- | *- | 11 |
| Ethernet1/3 | MLAG_PEER_vx-subleaf5b_Ethernet1/3 | *trunk | *- | *- | *['MLAG'] | 13 |
| Ethernet1/4 | MLAG_PEER_vx-subleaf5b_Ethernet1/4 | *trunk | *- | *- | *['MLAG'] | 13 |
| Ethernet3/1 | ap3_LAN1 | *access | *10 | *- | *- | 31 |
| Ethernet3/2 | ap4_LAN1 | *access | *10 | *- | *- | 32 |

*Inherited from Port-Channel Interface

#### Ethernet Interfaces Device Configuration

```eos
!
interface Ethernet1/1
   description VX-ACCESSLEAF3A_Ethernet46
   no shutdown
   channel-group 11 mode active
!
interface Ethernet1/2
   description VX-ACCESSLEAF3B_Ethernet46
   no shutdown
   channel-group 11 mode active
!
interface Ethernet1/3
   description MLAG_PEER_vx-subleaf5b_Ethernet1/3
   no shutdown
   channel-group 13 mode active
!
interface Ethernet1/4
   description MLAG_PEER_vx-subleaf5b_Ethernet1/4
   no shutdown
   channel-group 13 mode active
!
interface Ethernet3/1
   description ap3_LAN1
   no shutdown
   speed auto 1000full
   switchport access vlan 1202
   switchport mode access
   channel-group 31 mode active
!
interface Ethernet3/2
   description ap4_LAN1
   no shutdown
   speed auto 1000full
   switchport access vlan 1202
   switchport mode access
   channel-group 32 mode active
```

### Port-Channel Interfaces

#### Port-Channel Interfaces Summary

##### L2

| Interface | Description | Type | Mode | VLANs | Native VLAN | Trunk Group | LACP Fallback Timeout | LACP Fallback Mode | MLAG ID | EVPN ESI |
| --------- | ----------- | ---- | ---- | ----- | ----------- | ------------| --------------------- | ------------------ | ------- | -------- |
| Port-Channel11 | ACCESSLEAF3_Po46 | switched | trunk | 10,1202,1213,1250 | - | - | - | - | 11 | - |
| Port-Channel13 | MLAG_PEER_vx-subleaf5b_Po13 | switched | trunk | - | - | ['MLAG'] | - | - | - | - |
| Port-Channel31 | ap3_PortChannel AP3 | switched | access | 10 | - | - | - | individual | 31 | - |
| Port-Channel32 | ap4_PortChannel AP4 | switched | access | 10 | - | - | - | individual | 32 | - |

#### Port-Channel Interfaces Device Configuration

```eos
!
interface Port-Channel11
   description ACCESSLEAF3_Po46
   no shutdown
   switchport
   switchport trunk allowed vlan 10,1202,1213,1250
   switchport mode trunk
   mlag 11
!
interface Port-Channel13
   description MLAG_PEER_vx-subleaf5b_Po13
   no shutdown
   switchport
   switchport mode trunk
   switchport trunk group MLAG
!
interface Port-Channel31
   description ap3_PortChannel AP3
   no shutdown
   switchport
   switchport access vlan 10
   port-channel lacp fallback individual
   mlag 31
   spanning-tree portfast
!
interface Port-Channel32
   description ap4_PortChannel AP4
   no shutdown
   switchport
   switchport access vlan 10
   port-channel lacp fallback individual
   mlag 32
   spanning-tree portfast
```

### VLAN Interfaces

#### VLAN Interfaces Summary

| Interface | Description | VRF |  MTU | Shutdown |
| --------- | ----------- | --- | ---- | -------- |
| Vlan4094 | MLAG_PEER | default | 9214 | False |

##### IPv4

| Interface | VRF | IP Address | IP Address Virtual | IP Router Virtual Address | VRRP | ACL In | ACL Out |
| --------- | --- | ---------- | ------------------ | ------------------------- | ---- | ------ | ------- |
| Vlan4094 |  default  |  10.100.54.132/31  |  -  |  -  |  -  |  -  |  -  |

#### VLAN Interfaces Device Configuration

```eos
!
interface Vlan4094
   description MLAG_PEER
   no shutdown
   mtu 9214
   no autostate
   ip address 10.100.54.132/31
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
| default | False |
| MGMT | False |

#### IP Routing Device Configuration

```eos
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

## Multicast

### IP IGMP Snooping

#### IP IGMP Snooping Summary

| IGMP Snooping | Fast Leave | Interface Restart Query | Proxy | Restart Query Interval | Robustness Variable |
| ------------- | ---------- | ----------------------- | ----- | ---------------------- | ------------------- |
| Enabled | - | - | - | - | - |

#### IP IGMP Snooping Device Configuration

```eos
```

## Power Over Ethernet (PoE)

### PoE Summary

#### PoE Global

| Reboot Action | Shutdown Action | LLDP Negotiation |
| ------------------- | -------------------- | ----------------------|
| power-off | power-off | - |

### PoE Device Configuration

```eos
!
poe
   interface shutdown action power-off
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
