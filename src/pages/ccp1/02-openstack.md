---
path: "/ccp1/openstack"
title: "CCP1 OpenStack"
date: "2020-01-03"
draft: false
author: Simon Anliker
tags:
  - ccp1
  - cloud
  - openstack
---

<!-- OSTK -->

# Open Stack

- Keystone: Identity service
- Nova: Compute
- Glance: Image service
- Neutron: Networking
- Horizon: Dashboard
- Cinder: Block storage
- Heat: Orchestration
- Telemetry: Celiometer
- Swift: Object storage
- Rally: Benchmark service
- Ironic: Bare metal
- Designate: DNS service
- Manila: Shared file systems


## Nova

Nova provides virtual servers on demand.

- Scheduler: manages virtual server instances on hosts 
- Network: manages IP forwarding, bridges, vlans
- Compute: manages communication with hypervisors and virtual servers
- Conductor: manages requests that need coordination, acts as db proxy


## Neutron

Standalone service that deploys processes across nodes.

- Neutron-server: services the networking API, enforces network-model and IP addressing of each port
- Plugin agent: runs on each compute node and manages local virtual switch configs
- DHCP agent: provides DHCP services to tenant networks
- L3 agent: provides L3/NAT forwarding to external network access of VMs
- SDN services: provides additional networking services to tenant networks


An OpenStack setup usually needs for distinct networks:
- Management network
- Guest network
- External network
- API network


## Glance

Virtual machine image registration and storage with pluggable storage backends.


## Cinder

Block level storage for VMs with puggable backends. 

## Swift

Object storage to store and retrieve data. Objects are stored in buckets. Eventually consistent design. 


## Keystone

Identity provider for authentication and authorization used by all OpenStack services. Pluggable front- and backends. Supports federated identity management.

## Horizon

Modular web based user interface / dashboard to manage OpenStack core functionality. Specialised services require the use of the command line.


## Ceilometer

Optional service for telemetry data. Complete monitoring environment for services, resources and infrastructure. Also provides metrics for billing and scaling decisions.

## Heat

Optional service for orchestration. Orchestrates using Heat Orchestration Template (HOT).

Main components:
- Heat-cfn (cloudformation)
- Heat-api
- Heat-engine
- Heat-metadata


 


