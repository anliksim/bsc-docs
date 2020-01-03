---
path: "/ccp1/containers"
title: "CCP1 Cloud Compute (Containers)"
date: "2020-01-03"
draft: false
author: Simon Anliker
tags:
  - ccp1
  - cloud
---

<!-- CCMP2 -->

# Compute Resource Virtualization

## OS-Level Virtualization

Share resources managed by the host OS kernel with guest processes. 
- Host OS provides guest isolation (visibility and access to resources)
- Applications use individual run-times (on top of host OS kernel resources)

Kernel needs to address security concerns and provide isolation on visibility, access and use of hsared resources.

### Control Groups

Linux cgroups control access and use of resources, provide resource usage monitoring and are dynamic and persistent.

- Cgroups are herarchically organised (support many different hierarchies simultaneously) 
- Child cgroups inherit certain attributes from parent cgroups.


Create a hierarchy:

    mkdir /cgroup/cpu_and_mem
    
List all subsystems:

    lssubsys -am cpu cpuset memoery net_cls ns cpuacct devices freezer blkio7
    
Mount the hierarchy and attach subsystems:

    mount -t cgroup -o cpu,cpuset,memory cpu_and_mem /cgroups/cpu_and_mem
    
Remount the hierarchy and include subsystem `cpuacct`:

    mount -t cgroup -o remount,cpu,cpuset,cpuacct,memory cpu_and_mem /cgroup/cpu_and_mem
    
Unmount the hierarchy:

    umount /cgroup/cpu_and_mem
    
Create a child in the hierarchy:

    mkdir /cgroup/cpu_and_mem/cgroup1
    
Configure a cgroup setting:

    echo 0-1 > /cgroup/cpu_and_mem/cgroup1/cpuset.cpus
    
Move a process into a cgroup:

    echo 1701 > /cgroup/cpu_and_mem/cgroup1/tasks
    
Start a process in a cgroup:

    echo $$ > /sys/fs/cgroup/cpu_and_mem/cgroup1/tasks && dd if=/dev/urandom | bzip2 -9 >> /dev/null
   
Find a process:

    ps -0 cgroup
    
Find a process with known PID:

    cat /proc/PID/cgroup
    
Find subsystems:

    cat /proc/cgroups
    
    
### Namespaces

Linux namespaces provide:
- Isolation: Wraps system resources so that it appears to the processes in a namespace that they have their own isolated instance of the global resource
- Visibility: Changes to the resources are only visible to other processes from the same namespace

Linux namespaces:
- Cgroup
- IPC
- Network
- Mount
- PID
- User
- UTS

Namespaces are used to implement containers.

## LXC

Linux Native Container Technology (LXC) is a technology enabled via kernel features
- Limits and prioritizes access to resources with cgroups (account for the usage)
- Limits visibility of resources using namespaces (operate in isolation)
- Provides a different and isolated root-fs for each container with `chroot`
- Secure isolation between containers with LSM & MAC

## Docker

Open source software engine to commoditize Linux Containers.
- Portability: Portable deployments across machines
- Application-centric: Geared towards app development, DevOps
- Build automation: Create from dockerfiles
- Versioning support: Tags, versions and hashes
- Component reuse: Containers can be used as a base
- Sharing: Public/private repositories for images
- Tools: CLI/REST API for interaction


### Container

A container is a runnable instance of an image and can be created, run, stopped, moved or deleted.
- Well isolated from each other/from the host machine
- Can be attached to networks and storage
- Defined by its image and configuration options
- Loses all state when removed
- Combination of read-only image (image layers) and a thin writable layer (container layer)

#### Environment Variables

Automatically added by default:
- HOME: defaults to `/` as default user is root
- HOSTNAME: hostname associated with the container
- PATH: defaults to `/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/`
- TERM: xterm if run with -i

User specified variables are added when running a container

    docker run -e LAB=Docker


#### Linking

Send envrionment variable from one container to another one at runtime by establishing a unidirectional link.

    docker run -d --name DB my_db
    docker run -d --link DB --name WEB my_app
    
Docker automatically exposes a set of environment variables of the form:

    <name>_PORT_<port>_<protocol>
    
In the above case it might be:

    DB_PORT=tcp://172.12.0.2:27017
    DB_PORT_27017_TCP_ADDR=172.12.0.2
    DB_PORT_27017_TCP_PORT=27017
    DB_PORT_27017_TCP_PROTO=tcp

### Images

An image is a collection of read-only content for creating a container. 
- Stored in a Registry
- Built with a Dockerfile that defines the steps needed to create the image
- Series of read-only layers
- Each instruction in the Dockerfile create a layer
- Each layer has its own UUID
- Lightweight, small and fast compared to virtualization technologies

Image layers are stored in the Docker host local storage area (typically in `/var/lib/docker`).


### Services

Docker Services appear to be a single application to the consumer and allow to scale collaborating containers across multiple Docker daemons. A service defines its desired state (resilience, elasticity).

### Dockerfile

Recommended, portable way to create Docker images.

`FROM` existing image \
`RUN` command \
`ADD` file from host machine \
`EXPOSE` a port by default \
`CMD` run command by default on start if not overridden \
`ENTRYPOINT` always run command on start

Nginx example:

    FROM ubuntu:16.04
    RUN apt-get update && apt-get install -y nginx
    EXPOSE 80 443
    ENTRYPOINT ["nginx"]
    CMD ["-g", "daemon off;"]

Build the image for the folder with the Dockerfile:

    docker build -t <my-container-image-name>


### Registry

Docker registry is open source code to store images. A public Docker registry is provided: Docker Hub.

Registry -> Repositories -> Images

Image naming convention: `<user-name>/<image-name>:<tag>`

Commands:

    docker login
    docker pull <image-name>:<tag>
    docker tag <tag> <image-name>
    docker push <image-name>:<tag>
    docker search <keyword>





