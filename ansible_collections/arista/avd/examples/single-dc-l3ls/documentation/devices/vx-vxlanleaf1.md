# vx-vxlanleaf1

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
- [VLANs](#vlans)
  - [VLANs Summary](#vlans-summary)
  - [VLANs Device Configuration](#vlans-device-configuration)
- [Interfaces](#interfaces)
  - [Ethernet Interfaces](#ethernet-interfaces)
  - [Port-Channel Interfaces](#port-channel-interfaces)
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
- [EOS CLI](#eos-cli)

## Management

### Management Interfaces

#### Management Interfaces Summary

##### IPv4

| Management Interface | description | Type | VRF | IP Address | Gateway |
| -------------------- | ----------- | ---- | --- | ---------- | ------- |
| Management1 | oob_management | oob | MGMT | 10.151.11.25/26 | 10.151.11.1 |

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
   ip address 10.151.11.25/26
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

STP mode: **mstp**

#### MSTP Instance and Priority

| Instance(s) | Priority |
| -------- | -------- |
| 0 | 4096 |

### Spanning Tree Device Configuration

```eos
!
spanning-tree mode mstp
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
| 11 | VLAN11-Firewall_Linknet | - |
| 1213 | mobile-prod-device | - |
| 1250 | enterprisenet | - |

### VLANs Device Configuration

```eos
!
vlan 11
   name VLAN11-Firewall_Linknet
!
vlan 1213
   name mobile-prod-device
!
vlan 1250
   name enterprisenet
```

## Interfaces

### Ethernet Interfaces

#### Ethernet Interfaces Summary

##### L2

| Interface | Description | Mode | VLANs | Native VLAN | Trunk Group | Channel-Group |
| --------- | ----------- | ---- | ----- | ----------- | ----------- | ------------- |
| Ethernet1/1 | VX-BORDERLEAF1A_Ethernet4 | *trunk | *11,1213,1250 | *- | *- | 11 |
| Ethernet1/2 | VX-BORDERLEAF1B_Ethernet4 | *trunk | *11,1213,1250 | *- | *- | 11 |

*Inherited from Port-Channel Interface

#### Ethernet Interfaces Device Configuration

```eos
!
interface Ethernet1/1
   description VX-BORDERLEAF1A_Ethernet4
   no shutdown
   channel-group 11 mode active
!
interface Ethernet1/2
   description VX-BORDERLEAF1B_Ethernet4
   no shutdown
   channel-group 11 mode active
```

### Port-Channel Interfaces

#### Port-Channel Interfaces Summary

##### L2

| Interface | Description | Type | Mode | VLANs | Native VLAN | Trunk Group | LACP Fallback Timeout | LACP Fallback Mode | MLAG ID | EVPN ESI |
| --------- | ----------- | ---- | ---- | ----- | ----------- | ------------| --------------------- | ------------------ | ------- | -------- |
| Port-Channel11 | BORDERLEAF1_Po4 | switched | trunk | 11,1213,1250 | - | - | - | - | - | - |

#### Port-Channel Interfaces Device Configuration

```eos
!
interface Port-Channel11
   description BORDERLEAF1_Po4
   no shutdown
   switchport
   switchport trunk allowed vlan 11,1213,1250
   switchport mode trunk
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

## EOS CLI

```eos
!
interface Vxlan1
  description vx-vxlanleaf1_VTEP
  vxlan source-interface Loopback1
  vxlan udp-port 4789
  vxlan flood vtep learned data-plane
  vxlan vlan 1213 vni 1213
  vxlan vlan 1250 vni 1250
  vxlan bridging vtep-to-vtep source-vtep tx disabled 10.100.54.26
interface Port-Channel11
  switchport trunk allowed vlan 101,1213,1250
no vlan 11
vlan 101
  name BORDER1-LINKNET
interface vlan 101
  ip address 10.100.55.4/29
interface Loopback0
  ip address 10.100.160.9/32
interface Loopback1
  ip address 10.100.160.10/32
router bgp 65104
  router-id 10.100.160.9
  graceful-restart restart-time 300
  graceful-restart
  maximum-paths 4 ecmp 4
  update wait-install
  no bgp default ipv4-unicast
  neighbor IPv4-UNDERLAY-PEERS peer group
  neighbor IPv4-UNDERLAY-PEERS password 7 7x4B4rnJhZB438m9+BrBfQ==
  neighbor IPv4-UNDERLAY-PEERS send-community
  neighbor IPv4-UNDERLAY-PEERS maximum-routes 12000
  neighbor IPv4-UNDERLAY-PEERS route-map RM-BGP-UNDERLAY-PEERS-OUT out
  neighbor 10.100.55.3 peer group IPv4-UNDERLAY-PEERS
  neighbor 10.100.55.3 remote-as 65101
  neighbor 10.100.55.3 description vx-borderleaf1
  redistribute connected
  address-family ipv4
      neighbor IPv4-UNDERLAY-PEERS activate
```
