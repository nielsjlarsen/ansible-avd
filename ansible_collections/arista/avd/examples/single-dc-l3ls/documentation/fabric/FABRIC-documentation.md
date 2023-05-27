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
| FABRIC | l3leaf | leaf1 | 172.16.60.101/24 | 720XP | Provisioned | - |
| FABRIC | l3leaf | leaf2 | 172.16.60.102/24 | 720XP | Provisioned | - |
| FABRIC | spine | spine1 | 172.16.60.100/24 | 7050T | Provisioned | - |
| FABRIC | l2leaf | subleaf1 | 172.16.60.103/24 | 710P | Provisioned | - |

> Provision status is based on Ansible inventory declaration and do not represent real status from CloudVision.

### Fabric Switches with inband Management IP

| POD | Type | Node | Management IP | Inband Interface |
| --- | ---- | ---- | ------------- | ---------------- |

## Fabric Topology

| Type | Node | Node Interface | Peer Type | Peer Node | Peer Interface |
| ---- | ---- | -------------- | --------- | ----------| -------------- |
| l3leaf | leaf1 | Ethernet1 | spine | spine1 | Ethernet1 |
| l3leaf | leaf2 | Ethernet1 | spine | spine1 | Ethernet2 |
| l3leaf | leaf2 | Ethernet2 | l2leaf | subleaf1 | Ethernet1 |

## Fabric IP Allocation

### Fabric Point-To-Point Links

| Uplink IPv4 Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ---------------- | ------------------- | ------------------ | ------------------ |
| 10.100.54.32/27 | 32 | 4 | 12.5 % |

### Point-To-Point Links Node Allocation

| Node | Node Interface | Node IP Address | Peer Node | Peer Interface | Peer IP Address |
| ---- | -------------- | --------------- | --------- | -------------- | --------------- |
| leaf1 | Ethernet1 | 10.100.54.33/31 | spine1 | Ethernet1 | 10.100.54.32/31 |
| leaf2 | Ethernet1 | 10.100.54.35/31 | spine1 | Ethernet2 | 10.100.54.34/31 |

### Loopback Interfaces (BGP EVPN Peering)

| Loopback Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ------------- | ------------------- | ------------------ | ------------------ |
| 10.100.160.0/27 | 32 | 3 | 9.38 % |

### Loopback0 Interfaces Node Allocation

| POD | Node | Loopback0 |
| --- | ---- | --------- |
| FABRIC | leaf1 | 10.100.160.3/32 |
| FABRIC | leaf2 | 10.100.160.4/32 |
| FABRIC | spine1 | 10.100.160.1/32 |

### VTEP Loopback VXLAN Tunnel Source Interfaces (VTEPs Only)

| VTEP Loopback Pool | Available Addresses | Assigned addresses | Assigned Address % |
| --------------------- | ------------------- | ------------------ | ------------------ |
| 10.100.54.16/28 | 16 | 2 | 12.5 % |

### VTEP Loopback Node allocation

| POD | Node | Loopback1 |
| --- | ---- | --------- |
| FABRIC | leaf1 | 10.100.54.19/32 |
| FABRIC | leaf2 | 10.100.54.20/32 |
