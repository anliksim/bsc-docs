---
path: "/ccp1/networking3"
title: "CCP1 Container Networking"
date: "2020-01-10"
draft: false
author: Simon Anliker
tags:
  - ccp1
  - cloud
  - network
---

<!-- CNET3 -->

# Container Networking

## Neutron

Project networks categorized as:

Provider Networks
- Integrated into DC network infrastructure
- Layer 2 connectivity only
- Admin/provider level
- Higher performance due to hardware based implementation
- Rely on external L3 services

Routed Provider Networks
- Support greater scalability by dividing provider network into segments
- Segments allocated to groups of nodes
- L3 services to enable connectivity beyond segment
- From user perspective all segments are on L3 networks

Self-Service Networks
- Provides flexibility to users
- Higher level of abstraction, beyond L2
- Enables users to create L3 overlay networks with L3-network objects (routers, gateways, dhcp server)
- Mostly software based
- L3 network entities created on demand and per project
- Limited by quotas
- External router provided by admin/provider
- User must specify subnet used by network
- Traffic isolation between tenant network using GRE or VXLAN tunnelling


VLAN segmentation policies
- Network deployment depends on cloud type (public, private, hybrid) and hardware choice (SDN, HA)
- Internal infrastructure level isolation
    - By traffic categories, admin concern
    - By tenants, user concern
- tenant separation at the overlay-level

e.g. management network, internal network, storage network

Networking node
- Run deployment-global networking logic
- Manages network namespaces / multi-tenancy
- Requires external connectivity
- Entities within networking node connected wwith `br-int`

Internode tunnel
- Static tunnels: Older, more stable, suffers from broadcast storms, neutron-ovs-agent inits VXLAN
- Dynamic tunnel configuration: VXLAN created on demand, neutron_openvswitch_agent manages

HA: L3-HA using OVS with Virtual Router Redundancy Protocol (VRRP) or Distributed Virtual Routing


## High-Availability

Stateless
- No dependency between requests
- No state migration on backup taking over

Stateful
- Request comprises several interactions
- Requires state migration on failure

Active/Active
- All providers are providing service
- Providers need to share state consistently if stateful
- Operates in degraded mode until recovered during failure

Active/Passive
- Only primary is providing service
- Secondary in standby mode
- Secondary activates in case of failure
- Needs migration of state on stateful services

Failover: Migration from one to another provider (migration of state on Active/Passive, capacity take-over on Active/Active)

Failback: Reestablishment of initial configuration

Switchover: Manually initiated change of roles


### VRRP

Virtual Router Redundancy Protocol (VRRP) 
- RFC2338
- Enable hosts to make use of redundant routing platforms

VRRP routing platforms
- Share IP address corresponding to the default route configured on the host
- At any time one is the master (active) and others are backups (passive)
- Backup router can take over within a few seconds
- Dynamically elect master based on priority on each router
- Master router sends advertisements to backup routers at regular intervals (heartbeat)
- Heartbeat timeout to elect new master
- Higher IP address wins on same priority

Keepalived
- keepalived is routing software written in C
- Provide LB and HA
- Linux Virtual Server (IPVS) kernel module providing layer 4 LB
- Health checks to adaptively maintain and manage routing server
- keepalived implements a set of hooks to the VRRP finite state machine 




### DVR

Distributed Virtual Routing (DVR) architecture provides direct connectivity to external networks on compute nodes
- With floating IP address and routing between project and external networks
- Routing resides completely on compute node
- Routing resilience scales with compute nodes
- Routing function itself not HA but failure has limited impact


## Docker Container Networking

Default networks (like provider networks)

User-defined networks (like self-service networks)

Standard Docker is host centric, Docker Swarm or Kubernetes for cluster management

Three networks provided automatically
- bridge: docker0, connected by default
- none: for containers without specific network interfaces
- host: adds container on the hosts network stack

List networks

    docker network ls
    
Define a network

    docker run --network=<network>
    
Inspect a network

    docker network inspect <network>
    
    
### User-defined networks

For more complex network configurations use user-defined bridge networks
- IP allocation
- Built-in DNS resolution
- Control which containers can communicate with each other

Docker provides network drivers for
- bridge network
- overlay network
- MACVLAN network
- Custom plugins for 3rd-party

Network lifecycle is independent from containers
- Create as many as required
- Connect a container to zero or more networks
- Connect and disconnect while container is running


Create a network

    docker network create --driver bridge isolatednet


### Iptables/NAT

SNAT implemented in POSTROUTING chain: Used to map from container IP address to host IP address for external access to container

DNAT mechanism can be used to expose ports: IP address substitution for specific port on host, mapped to container IP and port

Iptable rules are applied at host level in docker
- Not in container namespaces
- Docker adds docker-isolation chain to enable isolation between docker networks



