# FABRIC

## Table of Contents

- [Fabric Switches and Management IP](#fabric-switches-and-management-ip)
  - [Fabric Switches with inband Management IP](#fabric-switches-with-inband-management-ip)
- [Fabric Topology](#fabric-topology)
- [Fabric IP Allocation](#fabric-ip-allocation)
  - [Fabric Point-To-Point Links](#fabric-point-to-point-links)
  - [Point-To-Point Links Node Allocation](#point-to-point-links-node-allocation)
  - [Loopback Interfaces (BGP EVPN Peering)](#loopback-interfaces-bgp-evpn-peering)
  - [Loopback0 Interfaces Node Allocation](#loopback0-interfaces-node-allocation)
  - [VTEP Loopback VXLAN Tunnel Source Interfaces (VTEPs Only)](#vtep-loopback-vxlan-tunnel-source-interfaces-vteps-only)
  - [VTEP Loopback Node allocation](#vtep-loopback-node-allocation)

## Fabric Switches and Management IP

| POD | Type | Node | Management IP | Platform | Provisioned in CloudVision | Serial Number |
| --- | ---- | ---- | ------------- | -------- | -------------------------- | ------------- |
| FABRIC | l3leaf | vx-accessleaf3a | 10.151.11.19/26 | 7050X3 | Provisioned | JMX2304A1QJ |
| FABRIC | l3leaf | vx-accessleaf3b | 10.151.11.20/26 | 7050X3 | Provisioned | JMX2303A6SE |
| FABRIC | l3leaf | vx-borderleaf1a | 10.151.11.23/26 | 7050X3 | Provisioned | JMX2304A20L |
| FABRIC | l3leaf | vx-borderleaf1b | 10.151.11.24/26 | 7050X3 | Provisioned | JMX2304A22D |
| FABRIC | l3leaf | vx-borderleaf2a | 10.151.11.21/26 | 7050X3 | Provisioned | JMX2304A1PE |
| FABRIC | l3leaf | vx-borderleaf2b | 10.151.11.22/26 | 7050X3 | Provisioned | JMX2304A235 |
| FABRIC | spine | vx-spine1 | 10.151.11.17/26 | 7050X3 | Provisioned | JMX2304A4RT |
| FABRIC | spine | vx-spine2 | 10.151.11.18/26 | 7050X3 | Provisioned | JMX2304A237 |
| FABRIC | l2leaf | vx-subleaf4a | 10.151.11.15/26 | 722XPM | Provisioned | HBG231201ET |
| FABRIC | l2leaf | vx-subleaf4b | 10.151.11.16/26 | 722XPM | Provisioned | HBG231201J4 |
| FABRIC | l2leaf | vx-subleaf5a | 10.151.11.12/26 | 755XP | Provisioned | HNN20505070 |
| FABRIC | l2leaf | vx-subleaf5b | 10.151.11.11/26 | 755XP | Provisioned | HNN21045125 |

> Provision status is based on Ansible inventory declaration and do not represent real status from CloudVision.

### Fabric Switches with inband Management IP

| POD | Type | Node | Management IP | Inband Interface |
| --- | ---- | ---- | ------------- | ---------------- |

## Fabric Topology

| Type | Node | Node Interface | Peer Type | Peer Node | Peer Interface |
| ---- | ---- | -------------- | --------- | ----------| -------------- |
| l3leaf | vx-accessleaf3a | Ethernet1 | spine | vx-spine1 | Ethernet1 |
| l3leaf | vx-accessleaf3a | Ethernet2 | spine | vx-spine2 | Ethernet1 |
| l3leaf | vx-accessleaf3a | Ethernet45 | l2leaf | vx-subleaf5b | Ethernet1/1 |
| l3leaf | vx-accessleaf3a | Ethernet46 | l2leaf | vx-subleaf5a | Ethernet1/1 |
| l3leaf | vx-accessleaf3a | Ethernet47 | l2leaf | vx-subleaf4b | Ethernet56 |
| l3leaf | vx-accessleaf3a | Ethernet48 | l2leaf | vx-subleaf4a | Ethernet56 |
| l3leaf | vx-accessleaf3a | Ethernet49/1 | mlag_peer | vx-accessleaf3b | Ethernet49/1 |
| l3leaf | vx-accessleaf3a | Ethernet50/1 | mlag_peer | vx-accessleaf3b | Ethernet50/1 |
| l3leaf | vx-accessleaf3b | Ethernet1 | spine | vx-spine1 | Ethernet2 |
| l3leaf | vx-accessleaf3b | Ethernet2 | spine | vx-spine2 | Ethernet2 |
| l3leaf | vx-accessleaf3b | Ethernet45 | l2leaf | vx-subleaf5b | Ethernet1/2 |
| l3leaf | vx-accessleaf3b | Ethernet46 | l2leaf | vx-subleaf5a | Ethernet1/2 |
| l3leaf | vx-accessleaf3b | Ethernet47 | l2leaf | vx-subleaf4b | Ethernet55 |
| l3leaf | vx-accessleaf3b | Ethernet48 | l2leaf | vx-subleaf4a | Ethernet55 |
| l3leaf | vx-borderleaf1a | Ethernet1 | spine | vx-spine1 | Ethernet5 |
| l3leaf | vx-borderleaf1a | Ethernet2 | spine | vx-spine2 | Ethernet5 |
| l3leaf | vx-borderleaf1a | Ethernet49/1 | mlag_peer | vx-borderleaf1b | Ethernet49/1 |
| l3leaf | vx-borderleaf1a | Ethernet50/1 | mlag_peer | vx-borderleaf1b | Ethernet50/1 |
| l3leaf | vx-borderleaf1b | Ethernet1 | spine | vx-spine1 | Ethernet6 |
| l3leaf | vx-borderleaf1b | Ethernet2 | spine | vx-spine2 | Ethernet6 |
| l3leaf | vx-borderleaf2a | Ethernet1 | spine | vx-spine1 | Ethernet3 |
| l3leaf | vx-borderleaf2a | Ethernet2 | spine | vx-spine2 | Ethernet3 |
| l3leaf | vx-borderleaf2a | Ethernet49/1 | mlag_peer | vx-borderleaf2b | Ethernet49/1 |
| l3leaf | vx-borderleaf2a | Ethernet50/1 | mlag_peer | vx-borderleaf2b | Ethernet50/1 |
| l3leaf | vx-borderleaf2b | Ethernet1 | spine | vx-spine1 | Ethernet4 |
| l3leaf | vx-borderleaf2b | Ethernet2 | spine | vx-spine2 | Ethernet4 |
| l2leaf | vx-subleaf4a | Ethernet49 | mlag_peer | vx-subleaf4b | Ethernet49 |
| l2leaf | vx-subleaf4a | Ethernet50 | mlag_peer | vx-subleaf4b | Ethernet50 |
| l2leaf | vx-subleaf5a | Ethernet1/3 | mlag_peer | vx-subleaf5b | Ethernet1/3 |
| l2leaf | vx-subleaf5a | Ethernet1/4 | mlag_peer | vx-subleaf5b | Ethernet1/4 |

## Fabric IP Allocation

### Fabric Point-To-Point Links

| Uplink IPv4 Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ---------------- | ------------------- | ------------------ | ------------------ |
| 10.100.54.32/27 | 32 | 24 | 75.0 % |

### Point-To-Point Links Node Allocation

| Node | Node Interface | Node IP Address | Peer Node | Peer Interface | Peer IP Address |
| ---- | -------------- | --------------- | --------- | -------------- | --------------- |
| vx-accessleaf3a | Ethernet1 | 10.100.54.49/31 | vx-spine1 | Ethernet1 | 10.100.54.48/31 |
| vx-accessleaf3a | Ethernet2 | 10.100.54.51/31 | vx-spine2 | Ethernet1 | 10.100.54.50/31 |
| vx-accessleaf3b | Ethernet1 | 10.100.54.53/31 | vx-spine1 | Ethernet2 | 10.100.54.52/31 |
| vx-accessleaf3b | Ethernet2 | 10.100.54.55/31 | vx-spine2 | Ethernet2 | 10.100.54.54/31 |
| vx-borderleaf1a | Ethernet1 | 10.100.54.33/31 | vx-spine1 | Ethernet5 | 10.100.54.32/31 |
| vx-borderleaf1a | Ethernet2 | 10.100.54.35/31 | vx-spine2 | Ethernet5 | 10.100.54.34/31 |
| vx-borderleaf1b | Ethernet1 | 10.100.54.37/31 | vx-spine1 | Ethernet6 | 10.100.54.36/31 |
| vx-borderleaf1b | Ethernet2 | 10.100.54.39/31 | vx-spine2 | Ethernet6 | 10.100.54.38/31 |
| vx-borderleaf2a | Ethernet1 | 10.100.54.41/31 | vx-spine1 | Ethernet3 | 10.100.54.40/31 |
| vx-borderleaf2a | Ethernet2 | 10.100.54.43/31 | vx-spine2 | Ethernet3 | 10.100.54.42/31 |
| vx-borderleaf2b | Ethernet1 | 10.100.54.45/31 | vx-spine1 | Ethernet4 | 10.100.54.44/31 |
| vx-borderleaf2b | Ethernet2 | 10.100.54.47/31 | vx-spine2 | Ethernet4 | 10.100.54.46/31 |

### Loopback Interfaces (BGP EVPN Peering)

| Loopback Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ------------- | ------------------- | ------------------ | ------------------ |
| 10.100.160.0/27 | 32 | 8 | 25.0 % |

### Loopback0 Interfaces Node Allocation

| POD | Node | Loopback0 |
| --- | ---- | --------- |
| FABRIC | vx-accessleaf3a | 10.100.160.7/32 |
| FABRIC | vx-accessleaf3b | 10.100.160.8/32 |
| FABRIC | vx-borderleaf1a | 10.100.160.3/32 |
| FABRIC | vx-borderleaf1b | 10.100.160.4/32 |
| FABRIC | vx-borderleaf2a | 10.100.160.5/32 |
| FABRIC | vx-borderleaf2b | 10.100.160.6/32 |
| FABRIC | vx-spine1 | 10.100.160.1/32 |
| FABRIC | vx-spine2 | 10.100.160.2/32 |

### VTEP Loopback VXLAN Tunnel Source Interfaces (VTEPs Only)

| VTEP Loopback Pool | Available Addresses | Assigned addresses | Assigned Address % |
| --------------------- | ------------------- | ------------------ | ------------------ |
| 10.100.54.16/28 | 16 | 6 | 37.5 % |

### VTEP Loopback Node allocation

| POD | Node | Loopback1 |
| --- | ---- | --------- |
| FABRIC | vx-accessleaf3a | 10.100.54.23/32 |
| FABRIC | vx-accessleaf3b | 10.100.54.23/32 |
| FABRIC | vx-borderleaf1a | 10.100.54.19/32 |
| FABRIC | vx-borderleaf1b | 10.100.54.19/32 |
| FABRIC | vx-borderleaf2a | 10.100.54.21/32 |
| FABRIC | vx-borderleaf2b | 10.100.54.21/32 |
