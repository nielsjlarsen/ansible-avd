# blue-spine1
# Table of Contents

- [Management](#management)
  - [Management Interfaces](#management-interfaces)
  - [PTP](#ptp)
  - [Management API HTTP](#management-api-http)
- [Authentication](#authentication)
  - [Local Users](#local-users)
- [Monitoring](#monitoring)
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
- [Multicast](#multicast)
  - [Router Multicast](#router-multicast)
  - [PIM Sparse Mode](#pim-sparse-mode)
- [Filters](#filters)
- [ACL](#acl)
- [VRF Instances](#vrf-instances)
  - [VRF Instances Summary](#vrf-instances-summary)
  - [VRF Instances Device Configuration](#vrf-instances-device-configuration)
- [Quality Of Service](#quality-of-service)

# Management

## Management Interfaces

### Management Interfaces Summary

#### IPv4

| Management Interface | description | Type | VRF | IP Address | Gateway |
| -------------------- | ----------- | ---- | --- | ---------- | ------- |
| Management1 | oob_management | oob | MGMT | 192.168.0.11/24 | 192.168.0.1 |

#### IPv6

| Management Interface | description | Type | VRF | IPv6 Address | IPv6 Gateway |
| -------------------- | ----------- | ---- | --- | ------------ | ------------ |
| Management1 | oob_management | oob | MGMT | - | - |

### Management Interfaces Device Configuration

```eos
!
interface Management1
   description oob_management
   no shutdown
   vrf MGMT
   ip address 192.168.0.11/24
```

## PTP

### PTP Summary

| Clock ID | Source IP | Priority 1 | Priority 2 | TTL | Domain | Mode | Forward Unicast |
| -------- | --------- | ---------- | ---------- | --- | ------ | ---- | --------------- |
| 00:1C:73:14:00:01 | - | 20 | 1 | - | 127 | boundary | - |

### PTP Device Configuration

```eos
!
ptp clock-identity 00:1C:73:14:00:01
ptp priority1 20
ptp priority2 1
ptp domain 127
ptp mode boundary
ptp monitor threshold offset-from-master 250
ptp monitor threshold mean-path-delay 1500
ptp monitor sequence-id
ptp monitor threshold missing-message announce 3 sequence-ids
ptp monitor threshold missing-message delay-resp 3 sequence-ids
ptp monitor threshold missing-message follow-up 3 sequence-ids
ptp monitor threshold missing-message sync 3 sequence-ids
```

## Management API HTTP

### Management API HTTP Summary

| HTTP | HTTPS | Default Services |
| ---- | ----- | ---------------- |
| False | True | - |

### Management API VRF Access

| VRF Name | IPv4 ACL | IPv6 ACL |
| -------- | -------- | -------- |
| MGMT | - | - |

### Management API HTTP Configuration

```eos
!
management api http-commands
   protocol https
   no shutdown
   !
   vrf MGMT
      no shutdown
```

# Authentication

## Local Users

### Local Users Summary

| User | Privilege | Role | Disabled |
| ---- | --------- | ---- | -------- |
| admin | 15 | network-admin | False |
| ansible | 15 | - | False |

### Local Users Device Configuration

```eos
!
username admin privilege 15 role network-admin nopassword
username ansible privilege 15 secret sha512 $6$7u4j1rkb3VELgcZE$EJt2Qff8kd/TapRoci0XaIZsL4tFzgq1YZBLD9c6f/knXzvcYY0NcMKndZeCv0T268knGKhOEwZAxqKjlMm920
```

# Monitoring

# Spanning Tree

## Spanning Tree Summary

STP mode: **none**

## Spanning Tree Device Configuration

```eos
!
spanning-tree mode none
```

# Internal VLAN Allocation Policy

## Internal VLAN Allocation Policy Summary

| Policy Allocation | Range Beginning | Range Ending |
| ------------------| --------------- | ------------ |
| ascending | 1006 | 1199 |

## Internal VLAN Allocation Policy Configuration

```eos
!
vlan internal order ascending range 1006 1199
```

# Interfaces

## Ethernet Interfaces

### Ethernet Interfaces Summary

#### L2

| Interface | Description | Mode | VLANs | Native VLAN | Trunk Group | Channel-Group |
| --------- | ----------- | ---- | ----- | ----------- | ----------- | ------------- |

*Inherited from Port-Channel Interface

#### IPv4

| Interface | Description | Type | Channel Group | IP Address | VRF |  MTU | Shutdown | ACL In | ACL Out |
| --------- | ----------- | -----| ------------- | ---------- | ----| ---- | -------- | ------ | ------- |
| Ethernet1/1 | P2P_LINK_TO_BLUE-LEAF1_Ethernet51/1 | routed | - | 10.255.2.0/31 | default | 1500 | False | - | - |
| Ethernet1/2 | P2P_LINK_TO_BLUE-LEAF1_Ethernet52/1 | routed | - | 10.255.2.2/31 | default | 1500 | False | - | - |
| Ethernet1/3 | P2P_LINK_TO_BLUE-LEAF2_Ethernet51/1 | routed | - | 10.255.2.4/31 | default | 1500 | False | - | - |
| Ethernet1/4 | P2P_LINK_TO_BLUE-LEAF2_Ethernet52/1 | routed | - | 10.255.2.6/31 | default | 1500 | False | - | - |
| Ethernet3/1 | P2P_LINK_TO_BLUE-LEAF3_Ethernet51/1 | routed | - | 10.255.2.8/31 | default | 1500 | False | - | - |
| Ethernet4/1 | P2P_LINK_TO_BLUE-LEAF3_Ethernet52/1 | routed | - | 10.255.2.10/31 | default | 1500 | False | - | - |
| Ethernet5/1 | P2P_LINK_TO_BLUE-LEAF4_Ethernet51/1 | routed | - | 10.255.2.12/31 | default | 1500 | False | - | - |
| Ethernet6/1 | P2P_LINK_TO_BLUE-LEAF4_Ethernet52/1 | routed | - | 10.255.2.14/31 | default | 1500 | False | - | - |

### Ethernet Interfaces Device Configuration

```eos
!
interface Ethernet1/1
   description P2P_LINK_TO_BLUE-LEAF1_Ethernet51/1
   no shutdown
   mtu 1500
   no switchport
   ip address 10.255.2.0/31
   pim ipv4 sparse-mode
   ptp enable
   ptp sync-message interval -3
   ptp announce interval 0
   ptp transport ipv4
   ptp announce timeout 3
   ptp delay-req interval -3
!
interface Ethernet1/2
   description P2P_LINK_TO_BLUE-LEAF1_Ethernet52/1
   no shutdown
   mtu 1500
   no switchport
   ip address 10.255.2.2/31
   pim ipv4 sparse-mode
   ptp enable
   ptp sync-message interval -3
   ptp announce interval 0
   ptp transport ipv4
   ptp announce timeout 3
   ptp delay-req interval -3
!
interface Ethernet1/3
   description P2P_LINK_TO_BLUE-LEAF2_Ethernet51/1
   no shutdown
   mtu 1500
   no switchport
   ip address 10.255.2.4/31
   pim ipv4 sparse-mode
   ptp enable
   ptp sync-message interval -3
   ptp announce interval 0
   ptp transport ipv4
   ptp announce timeout 3
   ptp delay-req interval -3
!
interface Ethernet1/4
   description P2P_LINK_TO_BLUE-LEAF2_Ethernet52/1
   no shutdown
   mtu 1500
   no switchport
   ip address 10.255.2.6/31
   pim ipv4 sparse-mode
   ptp enable
   ptp sync-message interval -3
   ptp announce interval 0
   ptp transport ipv4
   ptp announce timeout 3
   ptp delay-req interval -3
!
interface Ethernet3/1
   description P2P_LINK_TO_BLUE-LEAF3_Ethernet51/1
   no shutdown
   mtu 1500
   no switchport
   ip address 10.255.2.8/31
   pim ipv4 sparse-mode
   ptp enable
   ptp sync-message interval -3
   ptp announce interval 0
   ptp transport ipv4
   ptp announce timeout 3
   ptp delay-req interval -3
!
interface Ethernet4/1
   description P2P_LINK_TO_BLUE-LEAF3_Ethernet52/1
   no shutdown
   mtu 1500
   no switchport
   ip address 10.255.2.10/31
   pim ipv4 sparse-mode
   ptp enable
   ptp sync-message interval -3
   ptp announce interval 0
   ptp transport ipv4
   ptp announce timeout 3
   ptp delay-req interval -3
!
interface Ethernet5/1
   description P2P_LINK_TO_BLUE-LEAF4_Ethernet51/1
   no shutdown
   mtu 1500
   no switchport
   ip address 10.255.2.12/31
   pim ipv4 sparse-mode
   ptp enable
   ptp sync-message interval -3
   ptp announce interval 0
   ptp transport ipv4
   ptp announce timeout 3
   ptp delay-req interval -3
!
interface Ethernet6/1
   description P2P_LINK_TO_BLUE-LEAF4_Ethernet52/1
   no shutdown
   mtu 1500
   no switchport
   ip address 10.255.2.14/31
   pim ipv4 sparse-mode
   ptp enable
   ptp sync-message interval -3
   ptp announce interval 0
   ptp transport ipv4
   ptp announce timeout 3
   ptp delay-req interval -3
```

## Loopback Interfaces

### Loopback Interfaces Summary

#### IPv4

| Interface | Description | VRF | IP Address |
| --------- | ----------- | --- | ---------- |
| Loopback0 | Router-id | default | 10.255.0.1/32 |

#### IPv6

| Interface | Description | VRF | IPv6 Address |
| --------- | ----------- | --- | ------------ |
| Loopback0 | Router-id | default | - |


### Loopback Interfaces Device Configuration

```eos
!
interface Loopback0
   description Router-id
   no shutdown
   ip address 10.255.0.1/32
```

# Routing
## Service Routing Protocols Model

Multi agent routing protocol model enabled

```eos
!
service routing protocols model multi-agent
```

## IP Routing

### IP Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | True |
| MGMT | false |

### IP Routing Device Configuration

```eos
!
ip routing
no ip routing vrf MGMT
```
## IPv6 Routing

### IPv6 Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | False |
| MGMT | false |

## Static Routes

### Static Routes Summary

| VRF | Destination Prefix | Next Hop IP             | Exit interface      | Administrative Distance       | Tag               | Route Name                    | Metric         |
| --- | ------------------ | ----------------------- | ------------------- | ----------------------------- | ----------------- | ----------------------------- | -------------- |
| MGMT | 0.0.0.0/0 | 192.168.0.1 | - | 1 | - | - | - |

### Static Routes Device Configuration

```eos
!
ip route vrf MGMT 0.0.0.0/0 192.168.0.1
```

## Router BGP

### Router BGP Summary

| BGP AS | Router ID |
| ------ | --------- |
| 65100|  10.255.0.1 |

| BGP Tuning |
| ---------- |
| maximum-paths 4 ecmp 4 |

### Router BGP Peer Groups

#### IPv4-UNDERLAY-PEERS

| Settings | Value |
| -------- | ----- |
| Address Family | ipv4 |
| Send community | all |
| Maximum routes | 12000 |

### BGP Neighbors

| Neighbor | Remote AS | VRF | Shutdown | Send-community | Maximum-routes | Allowas-in | BFD | RIB Pre-Policy Retain | Route-Reflector Client |
| -------- | --------- | --- | -------- | -------------- | -------------- | ---------- | --- | --------------------- | ---------------------- |
| 10.255.2.1 | 65101 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - |
| 10.255.2.3 | 65101 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - |
| 10.255.2.5 | 65102 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - |
| 10.255.2.7 | 65102 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - |
| 10.255.2.9 | 65103 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - |
| 10.255.2.11 | 65103 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - |
| 10.255.2.13 | 65104 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - |
| 10.255.2.15 | 65104 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - |

### Router BGP Device Configuration

```eos
!
router bgp 65100
   router-id 10.255.0.1
   maximum-paths 4 ecmp 4
   neighbor IPv4-UNDERLAY-PEERS peer group
   neighbor IPv4-UNDERLAY-PEERS password 7 7x4B4rnJhZB438m9+BrBfQ==
   neighbor IPv4-UNDERLAY-PEERS send-community
   neighbor IPv4-UNDERLAY-PEERS maximum-routes 12000
   neighbor 10.255.2.1 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.255.2.1 remote-as 65101
   neighbor 10.255.2.1 description blue-leaf1_Ethernet51/1
   neighbor 10.255.2.3 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.255.2.3 remote-as 65101
   neighbor 10.255.2.3 description blue-leaf1_Ethernet52/1
   neighbor 10.255.2.5 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.255.2.5 remote-as 65102
   neighbor 10.255.2.5 description blue-leaf2_Ethernet51/1
   neighbor 10.255.2.7 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.255.2.7 remote-as 65102
   neighbor 10.255.2.7 description blue-leaf2_Ethernet52/1
   neighbor 10.255.2.9 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.255.2.9 remote-as 65103
   neighbor 10.255.2.9 description blue-leaf3_Ethernet51/1
   neighbor 10.255.2.11 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.255.2.11 remote-as 65103
   neighbor 10.255.2.11 description blue-leaf3_Ethernet52/1
   neighbor 10.255.2.13 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.255.2.13 remote-as 65104
   neighbor 10.255.2.13 description blue-leaf4_Ethernet51/1
   neighbor 10.255.2.15 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.255.2.15 remote-as 65104
   neighbor 10.255.2.15 description blue-leaf4_Ethernet52/1
   redistribute connected
   !
   address-family ipv4
      neighbor IPv4-UNDERLAY-PEERS activate
```

# Multicast

## Router Multicast

### IP Router Multicast Summary

- Routing for IPv4 multicast is enabled.

### Router Multicast Device Configuration

```eos
!
router multicast
   ipv4
      routing
```


## PIM Sparse Mode

### Router PIM Sparse Mode

#### IP Sparse Mode Information

BFD enabled: False

##### IP Rendezvous Information

| Rendezvous Point Address | Group Address | Access Lists | Priority | Hashmask | Override |
| ------------------------ | ------------- | ------------ | -------- | -------- | -------- |
| 10.255.0.1 | - | - | - | - | - |

#### Router Multicast Device Configuration

```eos
!
router pim sparse-mode
   ipv4
      rp address 10.255.0.1
```

### PIM Sparse Mode enabled interfaces

| Interface Name | VRF Name | IP Version | DR Priority | Local Interface |
| -------------- | -------- | ---------- | ----------- | --------------- |
| Ethernet1/1 | - | IPv4 | - | - |
| Ethernet1/2 | - | IPv4 | - | - |
| Ethernet1/3 | - | IPv4 | - | - |
| Ethernet1/4 | - | IPv4 | - | - |
| Ethernet3/1 | - | IPv4 | - | - |
| Ethernet4/1 | - | IPv4 | - | - |
| Ethernet5/1 | - | IPv4 | - | - |
| Ethernet6/1 | - | IPv4 | - | - |

# Filters

# ACL

# VRF Instances

## VRF Instances Summary

| VRF Name | IP Routing |
| -------- | ---------- |
| MGMT | disabled |

## VRF Instances Device Configuration

```eos
!
vrf instance MGMT
```

# Quality Of Service
