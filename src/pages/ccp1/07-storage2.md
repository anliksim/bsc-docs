---
path: "/ccp1/storage2"
title: "CCP1 Storage 2 (Swift, Glance)"
date: "2020-01-03"
draft: true
author: Simon Anliker
tags:
  - ccp1
  - cloud
  - storage
---

<!-- CSTR2 -->

# Cloud Storage

## Object Storage

- No hierarchical structure, only containers and objects
- Good for unstructured data
- Object is data blob with unique ID (e.g. hash)
- Can hold metadata (key-value pairs)
- Access via REST, CRUD
- Update as delete and create
- Stored in buckets (containers)
- Data represented as binary, opaque objects

## OpenStack Swift

Distributed, eventually consistent, redundant object storage
- On commodity hardware
- Fault tolerant
- Scales to petabytes
- Preserves data integrity
- User limits and ACLs
- REST APIs
- Every objects has URI and metadata
- Data location transparent to end user
- Resilience by adoption of data replication
- Concept of failre domain
- Maintenance without downtime / infrastructure elasticity

### Data Model

Flat hierarchy of accounts, containers, objects.

Account
- Top-level hierarchy
- Owns all associated resources
- Defines namespace for containers
- Synonymous with project or tenant

Container
- Defines namespace for objects
- Controls access to objects with an ACL
- Storage policies on container level

Object
- Data and metadata
- Each can be 5GB by default (configurable)
- Compress files using content-encoding
- Auto-extract archive files
- Schedule deletion
- Generate URL for time limited GET access


Access via endpoint and api version plus object path

    http://swift.examle.com/v1/container/object

Show account info and list all containers

    GET http://swift.examle.com/v1
    
Create new container

    PUT http://swift.examle.com/v1/container_id
    
List all objects of a container

    GET http://swift.examle.com/v1/container_id/
    
Create new object

    PUT http://swift.examle.com/v1/container_id/object_id
    
Delete an object 

    DELETE http://swift.examle.com/v1/container_id/object_id

### Architecture

Proxy Server
- Communication endpoint for clients
- Responsible for coordination between client and storage system
- Stateless design and implementation
- Supports high-availability and high-performance configuration

Storage Servers
- Management entities: Account and container server
    - Namespace partitioning
    - Implemented as SQLite DB
    - Holds management related metadata
- Data storage: Object storage server
    - Pool of storage nodes
    - On-disk storage
    - Big files are divided into segments and stored with manifest
    - Object related metadata stored in file system attributes

### Ring Concept

Ring represents a mapping between data entities and physical location in the cluster. Maintains the mapping between data, partitions, zones, devices.
- Account server: uses the account ring to maintain a list of containers
- Container server: uses container ring to maintain a list of objects
- Object server: uses the object ring to maintain a list of object locations, one ring per storage policy 

Failure domains
- Region: Geographical, WAN
- Zone: Local, power/network separation

Partitions
- Across single device: speed
- Across multiple devices: recover from failures

Replicas
- Replica count (3 by default)
- Balance: Achieve capacity distribution
- Dispersion: Isolate replicas across failure domains
- Device weight: Balance distribution of partition according to storage capacity and speed

Consistent hashing for location
- Stateless algorithm to select correct partition
- C = partition power, number of bits from MD5 hash of the item path
- N = total number of partitions in the ring
    
    
    partition index = (hash(item path) % C) % N 

### Storage Policies

One object ring per storage policy to segregate object storage within a single cluster

- Levels of durability: Multiple replication policies can be setup for the same cluster
- Performance: SSD-only object ring can be created for low-latency, high performance policy
- Grouping nodes: Objects rings may have different physical servers
- Storage implementations: Nodes that use different Diskfile and use policy to direct traffic to only those

### CAP

Designed as AP (Availability, Partition Tolerance) system with EC (eventual consistency)
- A: Always returns a value reply
- P: Scalable, resilient, performance
- EC: may provide outdated data

### Request Flow

Resource creation
- HTTP PUT request to proxy server
- Proxy server determines rings and partitions for provided data
- Success if majority of partitions respond

Resource retrieval
- HTTP GET request to proxy server
- Proxy server determines rings and partitions for requested data
- Requests data from partitions
- Returns object data from responding partitions

Keystone token as header mandatory: X-Auth-Token

### Extensibility

DiskFile API enabled pluggable backends

Storage backends
- Swift-on-File: POSIX filesystem
- swift-ceph-backend: Ceph RADOS
- kinetic-swift: Seagate Kinetic Drive
- swift-scality-backend: Scality (sproxyd)

## OpenStack Ceph

### Architecture

### Interfaces

### CRUSH

