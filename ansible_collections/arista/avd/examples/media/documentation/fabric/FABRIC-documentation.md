# FABRIC

# Table of Contents

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

# Fabric Switches and Management IP

| POD | Type | Node | Management IP | Platform | Provisioned in CloudVision |
| --- | ---- | ---- | ------------- | -------- | -------------------------- |
| FABRIC | leaf | blue-leaf1 | 192.168.0.21/24 | vEOS-lab | Provisioned |
| FABRIC | leaf | blue-leaf2 | 192.168.0.22/24 | vEOS-lab | Provisioned |
| FABRIC | leaf | blue-leaf3 | 192.168.0.21/24 | vEOS-lab | Provisioned |
| FABRIC | leaf | blue-leaf4 | 192.168.0.22/24 | vEOS-lab | Provisioned |
| FABRIC | spine | blue-spine1 | 192.168.0.11/24 | vEOS-lab | Provisioned |

> Provision status is based on Ansible inventory declaration and do not represent real status from CloudVision.

## Fabric Switches with inband Management IP
| POD | Type | Node | Management IP | Inband Interface |
| --- | ---- | ---- | ------------- | ---------------- |

# Fabric Topology

| Type | Node | Node Interface | Peer Type | Peer Node | Peer Interface |
| ---- | ---- | -------------- | --------- | ----------| -------------- |
| leaf | blue-leaf1 | Ethernet51/1 | spine | blue-spine1 | Ethernet1/1 |
| leaf | blue-leaf1 | Ethernet52/1 | spine | blue-spine1 | Ethernet1/2 |
| leaf | blue-leaf2 | Ethernet51/1 | spine | blue-spine1 | Ethernet1/3 |
| leaf | blue-leaf2 | Ethernet52/1 | spine | blue-spine1 | Ethernet1/4 |
| leaf | blue-leaf3 | Ethernet51/1 | spine | blue-spine1 | Ethernet3/1 |
| leaf | blue-leaf3 | Ethernet52/1 | spine | blue-spine1 | Ethernet4/1 |
| leaf | blue-leaf4 | Ethernet51/1 | spine | blue-spine1 | Ethernet5/1 |
| leaf | blue-leaf4 | Ethernet52/1 | spine | blue-spine1 | Ethernet6/1 |

# Fabric IP Allocation

## Fabric Point-To-Point Links

| Uplink IPv4 Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ---------------- | ------------------- | ------------------ | ------------------ |
| 10.255.2.0/24 | 256 | 16 | 6.25 % |

## Point-To-Point Links Node Allocation

| Node | Node Interface | Node IP Address | Peer Node | Peer Interface | Peer IP Address |
| ---- | -------------- | --------------- | --------- | -------------- | --------------- |
| blue-leaf1 | Ethernet51/1 | 10.255.2.1/31 | blue-spine1 | Ethernet1/1 | 10.255.2.0/31 |
| blue-leaf1 | Ethernet52/1 | 10.255.2.3/31 | blue-spine1 | Ethernet1/2 | 10.255.2.2/31 |
| blue-leaf2 | Ethernet51/1 | 10.255.2.5/31 | blue-spine1 | Ethernet1/3 | 10.255.2.4/31 |
| blue-leaf2 | Ethernet52/1 | 10.255.2.7/31 | blue-spine1 | Ethernet1/4 | 10.255.2.6/31 |
| blue-leaf3 | Ethernet51/1 | 10.255.2.9/31 | blue-spine1 | Ethernet3/1 | 10.255.2.8/31 |
| blue-leaf3 | Ethernet52/1 | 10.255.2.11/31 | blue-spine1 | Ethernet4/1 | 10.255.2.10/31 |
| blue-leaf4 | Ethernet51/1 | 10.255.2.13/31 | blue-spine1 | Ethernet5/1 | 10.255.2.12/31 |
| blue-leaf4 | Ethernet52/1 | 10.255.2.15/31 | blue-spine1 | Ethernet6/1 | 10.255.2.14/31 |

## Loopback Interfaces (BGP EVPN Peering)

| Loopback Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ------------- | ------------------- | ------------------ | ------------------ |
| 10.255.0.0/27 | 32 | 1 | 3.13 % |
| 10.255.1.0/27 | 32 | 4 | 12.5 % |

## Loopback0 Interfaces Node Allocation

| POD | Node | Loopback0 |
| --- | ---- | --------- |
| FABRIC | blue-leaf1 | 10.255.1.1/32 |
| FABRIC | blue-leaf2 | 10.255.1.2/32 |
| FABRIC | blue-leaf3 | 10.255.1.3/32 |
| FABRIC | blue-leaf4 | 10.255.1.4/32 |
| FABRIC | blue-spine1 | 10.255.0.1/32 |

## VTEP Loopback VXLAN Tunnel Source Interfaces (VTEPs Only)

| VTEP Loopback Pool | Available Addresses | Assigned addresses | Assigned Address % |
| --------------------- | ------------------- | ------------------ | ------------------ |

## VTEP Loopback Node allocation

| POD | Node | Loopback1 |
| --- | ---- | --------- |
