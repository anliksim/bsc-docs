---
path: "/ccp1/storage1"
title: "CCP1 Storage 1 (Ephemeral, Cinder)"
date: "2020-01-03"
draft: true
author: Simon Anliker
tags:
  - ccp1
  - cloud
  - storage
---

<!-- CSTR1 -->

# Cloud Storage

## Block Storage

Data is represented as logical blocks over a complex physical layout. Concept of files is one more level of abstraction.

Storage Area Network (SAN)
- Dedicated network to access consolidated storage
- Array of storage devices is made available over high-speed network
- Only block-level, no file abstraction (block IO)
- Protocols: iSCSI, Fiber Channel, Encapsulated SCSI
- Not made of commodity hardware
- Only available as large units (unit level scalability)
- Single point of failure
- Not software-defined
- No native support for object storage

Network Attached Storage (NAS)
- Node connected to a network which has direct access to disk
- Offers file-based access
- Can be mounted locally as network drive
- Protocols: AFS, AFP, CIFS, FTP, HTTP, NFS
- No simple scale-out
- Slow
- Enterprise grade

Directly Attached Storage (DAS)
- Use disks in servers and provide as service
- Storage and compute scale together
- Block-level performance fully distibuted
- Requires middleware (Cinder, Swift)


SAN vs NAS
- SAN appears as a block level device that can be mounted and formatted
- SAN scales well in large deployments
- NAS usually used to directly expose a filesystem
- NAS usually as independent single boxes
- NAS is simpler for simple data sharing/file access
- NAS is less flexible/powerful as it already includes abstraction layers over block storage

Common requirement: (Cloud) Storage requirements and design principles


## Requirements

- Flexible volume management
- Multi-tenancy
- Thin-provisioning
- Accounting and user policies
- No operational down-time
- Privacy and security

### Data Placement

- Placement of data is handled transparently
- Data is allocated, migrated, rebalanced transparently
- Policies can be applied to increase usage efficiency of resources (same data on less components)
- Policies can be applied to increase I/O performance (same data on multiple components)

### Data Striping

- Segment sequential data into stripes
- Store stripes on separate disks/nodes
- R/W operations on stripes can be parallel
- Throughput increase by factor N (N=number of stripes stored on different storage units), upper bound is network bandwidth
- Failure of any of the N storage units implies data loss

### Data Replication

- Data is replicated across failure zones
- Replicas: Equivalent data replicas, minimum 2 - already has uncompressed efficiency of 50%
- Erasure coding: Algorithms to replicate data more flexibly and still provide fault tolerance (e.g. RAID-Z2 allows 2 disks in a pool to fail)

### Data Durability

Industrial metric to determine the quality of a cloud storage system
- Independent from cluster size but not from architecture
- Obtained by determining the annual expected data loss
- Likelihood that customer data get permanently lost

e.g. annual loss of 0.01% = durability of 9.99% => store 1000 objects, lose 0.01 means we loose 0.01*1000=10 objects per year

### Data Availability

Likelihood that customer data will not be accessible at a particular point in time
- Durability loss implies availability loss (not the other way)
- Availability is uptime

e.g. subtract from 100% the average of error rates from each X minutes of a billing cycle

Both durability and availability depend on data placement, replication, MTTR adn MTTF.

### Design Principles

Availability: Always-available, failures of individual components must not affect the overall health

Scalability/Efficiency: Distribute to enable scalability, data deduplication, compression

Consistency: Ensure data in a distributed system remain consistent, copy-on-write

### CAP Theorem

Can't have them all.

Consistency: at a certain time, all nodes see the same data

Availability: system is able to process and reply to client requests

Partition Tolerance: system remains operational if a single component fails (e.g. network link)


AC: can only be achieved on a single machine (e.g. non-distributed DB)

PA: always replies but data might be outdated (e.g. DNS, Amazon web store)

PC: may not always reply but provided data is always consistent (e.g. flight booking system)

CAP: ongoing research to improve PA/PC (with e.g. conflict-free Replicated Data Types)


#### Spanner

Spanner prioritizes CP, sacrificing availability when necessary. 
In a stable network with network redundancy it can deliver 5 9's internally, which is very close to CAP.

## OpenStack Storage

Cinder is a block storage service that provides storage via Nova
- Software infrastructure for volumes management
- Outside of the data path
- Supports DAS on the local compute node
- Supports remote SAN/NAS via cinder backend drivers
- Data persistence beyond compute instance lifetime


Compute with local storage
- Simple configuration
- Can deliver higher performance to VMs (e.g. SSD)
- Large footprint on compute hosts
- Requires local storage on compute hosts
- Can be less resilient

Compute with remote storage
- Nova supports multiple remote storage backends
- NAS: NFS
- SAN: iSCSI, Ceph RBD (widely used, good qemu support, scales well)
- DFS: NFS (simple, proven, bottleneck)

## iSCSI

Internet SCSI (iSCSI) is a protocol to access block devices over a network
- Different storage solutions behind interface
- Built on top of SCSI
- Supports non SCSI disks through iSCSI gateway
- Supports daisy chaining
- Encapsulates SCSI-3 instruction set (longer device names, timeouts, authentication)

iSCSI target: the server exposing storage resources

iSCSI initiator: the client accessing exported storage resources

iSCSI TPG: portal group form which block devices are exposed

iSCSI LUN: Logical unit identifying block device on TPG

## ZFS

Very powerful, mature, trusted local file system with advanced features
- Copy-on-write: on disk state always valid
- Transactional: everything fails or succeeds
- Checksummed: no silent corruption, bad metadata
- Snapshots
- Cloning


Software-defined storage resources
- Organize physical disks into vdevs
- Organize vdevs into pools
- Configure pool features and allocate volumes
- Expose volumes at block level or file level

Virtual devices (vdevs)
- Configuration is redundant
- Mirror (1:1 replica)
- Erasure codes: raidz1, raidz2, raidz3

zpool
- collection of vdevs

datasets
- can be created over zpools
- volume: block devices
- filesystem: storage unit for files
- snapshots of block devices or filesystem
- clones: writable volume or filesystem created from existing one


### Usage

Create zpool

    zpool create <poolname> <vdev> <vdev-resources>

Create zpool with 4 volumes and raidz2, two disks can fail, storage efficiency of 50% (raidz3 with 8 disks has efficiency of 62.5%)

    zpool create whirl raidz2 /dev/sda /dev/sdb /dev/sdc /dev/sdd
    
Query zpool status

    zpool status -v whirl
    
Configure pool compression to lz4

    zfs set compression=lz4 whirl
    
Add spare device to pool for increased availability

    zpool add whirl spare /dev/sde
    
Create volume

    sudo zfs create -V 30MB whirl/vol
    
Check existing datasets

    sudo zfs list
  
    


