---
path: "/ccp1/networking1"
title: "CCP1 DC Networking"
date: "2020-01-10"
draft: false
author: Simon Anliker
tags:
  - ccp1
  - cloud
  - network
---

<!-- CNET1 -->

# DC Networking

DC Layout
- Top or rack switch
- Server
- Storage devices
- Power shelf

Networks in data centers
- Internally or externally oriented
- Hierarchical
- Complex
- Static

## Network Architecture

### Internal

- Core layer
- Distribution layer
- Aggregation layer: Gateways, centralized, contains network logic
- Access layer: Web, app, DB servers


Link Aggregation (LAG): Requires capacity over-subscription

Fault tolerance: Equal Cost Multi-Pathing (ECMP)

Rack-to-Rack: Inefficient, no direct link, cross connect wiring over L2

L2 Broadcast Domains: Storms

Spanning Tree: Convergence, blocked links


Oversubscription: Ingress capacity exceeds egress capacity
- Ingress: Input/incoming
- Egress: Output/outgoing

E.g. rack of 24x10G servers and access device of at least 240G: 1:1 oversubscription (means NO oversubscription, non-blocking, required to allow the entire network to operate at linerate)

E.g. 24x10G server and 2x10G uplink: 12:1 oversubscription


### External

Internet connectivity

Co-location center carrier neutrality
- Redundant connectivity providers for fault tolerance
- DC operator provides access points for multiple-ingress/-egress

E.g. Ready to use layer 3 platform that any customer can access within 24 hours, for redundancy all routers and switches are interconnected to each other

Site-to-Site: Pre-VLAN virtual circuit


### Requirements

- Support different stakeholders (providers, customers)
- Robust and redundant: Expect that components will fail
- Modular and support heterogeneity: Hardware from different vendor, with different link speed, protocol,...
- Simple in scaling characteristics: Easy to scale up and down, non-complex and automated configs
- Flexible in the topology: Flexible to support connectivity service (Rack-to-Rack, Rack-to-Internet, Rack-to-Enterprise, Site-to-Site,...)
- Efficient and effective: Traffic engineering within a site or between sites
- Isolate tenants: Host and isolate different tenants (organisations/companies/users)


## Network Technologies

L2 virtualization
- VLAN
- MPLS/GMPLS

Beyond L2
- GRE
- VXLAN (encapsulate layer 2 ethernet frames in layer 4 UDP datagrams)

### VLAN

A VLAN is a logical subnet/division (broadcast domain) inside one - or spread over multiple - Layer 2 devices

Port-based VLAN
- Simplest mode
- Hard-wires any number of ports into one broadcast domain
- Very fast since no packet extension/header inspection requires
- Device port centric

Tagged VLAN
- Adds a VLAN-field to the ethernet header
- Allows for dynamic configuration
    - Tag on departure, when the frame leaves the host
    - Tag on arrival, when the frame arrives and has no tag
- Govern tagging
    - DYNAMIC/REMOTE: Device can request information on tags from central policy server
    - STATIC/LOCAL: Device has static config that tags all untagged frames
- VLAN Header is 4 Bytes
    - Byte 1,2: Tag Protocol Identifier (TPID), type of VLAN e.g. 0x8100
    - Byte 3,4: Tag Control Information (TCI) \
          - 3 bits for priority (ToS) \
          - 1 bit for Canonical Format Indicator (CPI) \
          - 12 bits for VLAN-ID (VID)

e.g. RedHat Linux VLAN Config (in `/etc/sysconfig/network-scripts/ifcfg-eth0.192`)

    DEVICE=eth0.192
    BOOTPROTO=none
    ONBOOT=yes
    IPADDR=192.168.1.1
    NETMASK=255.255.255.0
    USERCTL=no
    NETWORK=192.168.1.0
    VLAN=yes


VLAN Trunks
- Trunk is a link between two layer 2 devices, via connected (trunk) ports
- In port-base VLANs the trunk port adds information to the frame 
- In tagged-based VLANs the trunk port just forwards the frame
- Managed devices allow config of ports, a port can allow multiple VLANs
- Modern switches allow automatic trunk mode if frames with different VIDs arrive at the same interface


### MPLS

Multi-Protocol Label Switching (MPLS) is part of the packed-switched-networks
- Forwarding is fast, because no IP lookup is needed
- Not depending on ethernet

A MPLS Network forwards any frame based on a label. Device roles:

Label Switch Router (LSR)
- Within the MPLS network
- Decides based on labels which is the next hop
- Performs label operations
    - push (new label)
    - pop (remove label)
    - swap (reorder label)

Label Edge Router (LER)
- At the edge of the MPLS network
- Entry (ingress) or exit (egress) point
- Adds labels based on the forwarding equivalence class (FEC)
- Pops the last label and forwards it based on IP routing tables
- Ingress point labels a packet when entering the MPLS network

Labels
- Can be stacked, encapsulation of multiple MPLS labels in a row(important delta to VLAN)
- Bit 0-31
    - 20-bits: Label value used by LSR to lookup either next-hop, operation to perform or outgoing data-link encapsulation
    - 3-bits: Reserved for experimental use
    - 1-bit: Bottom of label stack flag, if set to 1 then this is the last label in the stack
    - 8-bits: TTL decremented by each LSR


Used for Site-to-Site DC connectivity


### GRE

Generic Routing Encapsulation (GRE) connects hosts across networks
1. Hosts are connected with static IP to a network e.g. internet
1. Every host gets a tunnel interface with a tunnel address
1. Tunnel can now be established
1. Add further routes from/to the tunnel interface to the network

GRE is a logical tunnel where IP packets are encapsulated by an IP frame (IP-over-IP) followed by the GRE header

GRE Header
- Bit 0: Checksum
- Bits 1-12: Reserved but currently unused
- Bits 13-15: GRE version
- Bits 16-31: Type of protocol that is encapsulated

The tunnel adds the enclosing IP header with protocol type 47=GRE (vs e.g. 6=TCP), which is removed again at the end of the tunnel


## Cloud

- Need for Multi-tenancy: Physical network end-to-end becomes a shared resource
- Need for Isolation: Different tenants share the same links
- Tenants need flexible network
    - Self-service
    - On-demand
    - Scalable
    - QoS support
    - Pay-as-you-go


### Flat Networking

Virtual Networking
    
    VMs -> Open vSwitch -> Physical Network

Private cloud
- Full connectivity
- e.g. enterprise environment


    Multiple tenants/VMs -> Shared Net -> Physical router

Private and public domains
- Replication or multi-master configuration of services possible over a private network
- e.g. OpenLDAP


    TenantA-Private Net <- TenantA VMs -> Shared Net -> Physical router
    TenantA-Private Net <- TenantB VMs -> 
                           TenantC VMs -> 
                

Public cloud hosting multiple tenants
- Tenants with multi-network requirements
- Tenants require control over network topology
- e.g. enterprise customer migrating infrastructure, on-demand servers/services for customers


        TenantA VMs -> TenantA-Private Net  <- TenantA router -> External Net -> Physical router
        TenantB VM1 -> TenantB-Private Net1 <- TenantB router -> External Net -> Physical router
        TenantB VM2 -> 
        TenantB VM3 -> TenantB-Private Net2 <- 
        TenantB VM4 -> 


IaaS means network also has to be offered as a service (NaaS)



