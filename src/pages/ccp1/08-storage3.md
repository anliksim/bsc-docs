---
path: "/ccp1/storage3"
title: "CCP1 Container Storage"
date: "2020-01-10"
draft: false
author: Simon Anliker
tags:
  - ccp1
  - cloud
  - storage
  - docker
  - kubernetes
---

<!-- CSTR3 -->

# Container Storage

Docker storage drivers
- Manage filesystem for images
- Provide ephemeral storage for writable containers

Docker volumes
- Non-persistent or persistent storage
- Sharing among multiple containers

Docker image layers
- Represent an instruction
- Only a set of deltas
- Read-only image layers
- Writable layer on top (container layer)
- All modifications are made to thin writable container layer
- Writable layer is deleted with the container

Pulling down an image
- e.g. `docker pull ubuntu:latest`
- Each layer is pulled down separately
- Each layer is stored separately in Docker storage area (i.e. `/var/lib/docker`)

Check out layers of an image

    docker history <image-id>


Storage drivers to unite layers

|Storage driver|Filesystems|
|---|---|
|overlay, overlay2|ext4, xfs|
|aufs|ext4, xfs|
|devicemapper|direct-lvm|
|btrfs|btrfs|
|zfs|zfs|

## AUFS

Uniting container layers with union mount. Each layer is an AUFS branch.

    sudo mount -t aufs -o dirs=/home/ubuntu/layers/writable_layer:/home/ubuntu/layers/base_layer none /aufs
    

Uses copy-on-write strategy
- Uses existing files for read access
- Copy-up operation to layer that needs write access to the file, read-only copy not seen by the container anymore (file is obscured)
- Minimizes I/O overhead and size (only relative delta to lower layers)
- On removal from lower layer, file is marked as removed from union (whiteout) but still exists in lower layers

## ZFS

Uniting container layers with clones and snapshots.
- The base layer is a ZFS filesystem
- Each child layer is a ZFS clone based on the ZFS snapshot of the layer below
- Container is a ZFS clone of the ZFS snapshot of the top layer of an image
- Containers unified filesystem is mounted on a mount point in `/var/lib/docker/zfs/graph`

Operations
- Read operations are performed on the writable ZFS clone that contains all data from lower layers, fast
- Writing a new file allocates space from underlying zpool and writes directly into writable ZFS clone
- Modifying a file writes into writable ZFS clone using copy-on-write
- On delete in a lower layer the ZFS driver masks the existence
- On delete in a writable layer the freed blocks are reclaimed by the zpool

Prepare a zpool for docker

    sudo zpool create -f zpool-docker -m /var/lib/docker /dev/sda1 /dev/sdb1
    

## Data management

Downsides of storing data in a writeable layer
- No persistence
- Difficult to get data out of the container
- Layer is tightly coupled to the host machine
- Requires storage drivers to interact with host filesystem

### Docker volumes

- Persistent storage
- Directories withing the docker area of the filesystem
- Managed by Docker
- Isolated from the core functionality of the host machine
- Can be mounted into multiple containers
- Exists independently of containers
- Support the use of volume drivers
- Will be created if it does not exist


Create a volume

    docker volume create volname
    
List volumes

    docker volume list
    
Inspect a volume

    docker volume inspect volname
    
Remove a volume

    docker volume rm volname
    
Starting a container with a volume

    docker run -d -it --mount source=volname, target=/app nginx:latest
    
Populate a volume with container-internal data, for sharing

    docker run -d -it --mount source=nginx-vol, destination=/usr/share/nginx/html nginx:latest
    
    
Docker volume drivers: Flocker, Ceph RBD, EMC Rexray, Convoy, Glusterfs, NFS, Azure File Service, iSCSI, FUSE derivatives


### Docker bind mounts

- Persistent storage
- May be stored anywhere on the host system
- File/dir mounted by full or relative path on host machine
- No Docker CLI commands to manage bind mounts
- Will be created if it does not exist
- Very good performance
- Rely on directory structure of host machine

Using a bind mount

    docker run -d -it --name devtest --mount type=bind, source=/docker-host/target, target=/app nginx:latest
    
Short for for bind-mounted volume

    docker run -v /my/host/path:/container/path
    

### Docker tmpfs

- Temporary file system
- Ephemeral storage
- Stored in the host systems memory (or swap)
- High-speed
- Non-persistent
- Secure access
- No disk IO compared to volumes/bind mounts
- Removed if container stops
- Not saved if container is committed

Use a tmpfs

    docker run -d -it --name tmptest --mount type=tmpfs, destination=/app nginx:latest


## Docker Swarm

Service
- Container image, network config, number of replicas, volume mounts
- Differs from Kubernetes services
- Can be scheduled on any worker node

Create single replica on a worker

    docker service create --replicas 1 --name pg postgres
    
Create replica with attached volume

    docker service create --replicas 1 --name pg --mount type=volume,source=mypgdata,target=/var/lib/postgresql,volume-driver=rexray postgres

## Kubernetes

K8 has more evolved notion of storage than Docker that just passes storage characteristics to a driver but capabilities not mature
- No snapshotting
- No backup management
- Resizing of volumes in alpha (as of v1.8)

 
StorageClass
- Level of abstraction
- Combines storage backend with characteristics (backup policy, speed/IOPS, encryption, replication)
- Can be used as basis of a service for users of resources
- Specifies provisioner (driver)


    kind: StorageClass
    apiVersion: storage.k8s.io/v1
    metadata:
        name: slow
    provisioner: kubernetes.io/gce-pd
    parameters:
        type: pd-standard (or pd-ssd for fast SSD storage)
        zone: us-central1-a
        zones: us-central1-a, us-central1-b
        fsType: ext4

PersistentVolume (PV)
- Supports one or more StorageClass
- Can be claimed by user with PersistentVolumeClaim
- Mapped to claim 1:1

PersistentVolumeClaim (PVC)
- Issues by user
- Can be attached to a container
- Specifies StorageClass
- Remains unbound if no appropriate PV available
- Bound to individual namespaces

    
    kind: PersistentVolumeClaim
    apiVersion: v1
    metadata:
        name: someclaim
    spec:
        storageClassName: slow
        accessMode:
        - ReadWriteOnce/ReadOnlyMany/ReadWriteMany (only single mode allowed)
        resources:
            requests:
                storage: 3Gi


### Static provisioning

PersistentVolume crated manually by an admin.

1. Admin creates StorageClass
1. Admin creates PersistentVolume
1. User creates PersistentVolumeClaim
1. PersistentVolume associated with claim
1. User issues instruction to launch pod
1. Scheduler selects compute node and storage resources mount to node
1. Pod launched and bound to storage resource


### Dynamic provisioning

PersistentVolume created on demand. Mechanism to create PersistentVolume depends on provisioner (Cinder, GCP, EBS, Azure).

1. Admin creates StorageClass
1. User creates PersistentVolumeClaim
1. Cluster sends request to provisioner to create PersistentVolume
1. Provioner provides PersistentVolume
1. User notified that claim has been satisfied
1. User issues instruction to launch pod
1. Scheduler selects compute node and storage resources mount to node
1. Pod launched and bound to storage resource

Characteristics of PersistentVolumes on revoked claim
- Retained: PV released and unavailable but still available to an admin
- Recycled: PV released, data removed and made available to other users
- Deleted: PV removed and deallocated


## Using Ceph

Prerequisites:
- Ceph storage cluster
- Ceph RDB available on all K8s nodes
- Ceph RDB available in kubernetes-controller-manager

1. Add ceph admin key to secrets in namespace kube-system (ceph-secret)
1. Create pool on ceph cluster
1. Create kube user and give it access to ceph pool
1. Add kube user password to the secrets
1. Create StorageClass, PersistentVolumeClaim

    
    kind: StorageClass
    apiVersion: storage.k8s.io/v1
    metadata:
        name: cephrdb
    provisioner: kubernetes.io/rdb
    parameters:
        monitors: <ip1>:6789, <ip2>:6789, <ip3>:6789
        adminId: admin
        adminSecretName: ceph-secret
        adminSecretNamespace: "kube-system"
        pool: kube
        userId: kube
        userSecretName: ceph-secret-kube
        
        
Workflow
1. User creates PVC
1. kubernetes-controller-manager creates block device
1. Claim satisfied
1. User triggers pod creation
1. Scheduler schedules pod to specific host
1. rdb kernel driver on host attaches block device
1. Block device is formatted on host and mapped to host filesystem (if not already)
1. Block device is bind-mounted to the container