---
path: "/ccp1/storage2"
title: "CCP1 Storage 2 (Swift, Glance)"
date: "2020-01-08"
draft: false
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

Unified object, block, file store on top of object-based system. All data is stored as objects in a flat namespace.

RADOS: Reliable Autonomic Distributed Object Store

Intergrates with OpenStack Services via python library librdb


### Architecture

Monitor daemon (MON):
- Deploy in small, odd number of instances
- Does not serve stored objects to client
- Maintains state and cluster membership
- Provides consensus for distributed decision making

Object storage device daemon (OSD):
- Minimum three in a cluster
- One per disk or RAID group
- Serves stored objects to clients
- Intelligently peer to perform replication tasks

Meta Data Service (MDS):
- Only requires for CephFS/ shared filesystems
- Does not server file data to clients
- Manages metadata for POSIX-compliant shared filesystems
- Stores metadata in RADOS

### Interfaces

Native
- Provides direct access to RADOS for apps
- librados, native support for C, Java etc
- Local, no HTTP overhead

Object
- REST-based interface to RADOSGW daemon
- Supports buckets and user management
- Provides S3 and Swift compatible interfaces

Block
- RADOS Block device (RBD, librdb)
- Storage of virtual disks in RADOS
- Images are striped across cluster
- Support by QEMU/KVM, OpenStack

File
- Provided by CephFS (libcephfs)
- Linux kernel client (`mount -t ceph 192.168.1.5:/ /mnt/tank`)
    - Can be exported with NFS or samba
- CephFS library for apps (libcephfs.so)


### Concepts

No central gateway for data client operations
- Operation with MONs require only latest version of cluster map
- Direct data exchange with OSDs
- OSDs handle replication of objects to other nodes for availability

Objects placement is dynamically computed
- No central lookup table unlike Swift rings
- Data localisation with CRUSH
- Theoretically unlimited scalability


Devices: Physical location of objects

Placement groups (PG): Logical concept to group devices

Pools: Logical partitions with specific settings (ownership, replicas, PG number, CRUSH ruleset per PG, contains PGs)

CRUSH rulesets: Algorithm to select physical OSD where to store data, uses specific map configuration of DCs/buckets

Buckets: Physical hardware element in the hierarchy


Client runs CRUSH algorithm to determine where to write the object to. Primary OSD runs the algorithm too to determine the PGs for replicas. Same for reading and rebalancing.

#### CRUSH

Controlled Replication Under Scalable Hashing (CRUSH) 
- Decides where to store and retrieve data in a RADOS cluster.
- Enabled clients to directly communicate with OSDs

Two parts:

CRUSH Algorithm
- Stateless, only needs CRUSH Map
- Statistically uniform distribution (hash-based)
- Rule-based configuration
- Similar to consistent mapping in Swift Rings
- Operates upon CRUSH Map, aware of cluster topology

CRUSH Map
- Location and topology of OSDs
    - defines devices
    - defines bucket types
    - defines buckets
    - defines rules
- Input to the CRUSH algorithm
- Defined in a plain text file, freely defined by the operator
- Can be retrieved and edited


File -> Objects -> Placement Groups -> OSDs

1. File is broken into objects
2. Objects are mapped into PGs using simple hash function with adjustable bit-mask to control nr of PGs (`hash(objId) & mask = pgid`)
3. PGs are assigned to OSDs using the CRUSH algorithm/map (`CRUSH(pgid, CRUSHMAP) = [osd_x, osd_y]`)


