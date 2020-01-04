---
path: "/scad/containerised"
title: "SCAD Containerised"
date: "2020-01-04"
draft: false
author: Simon Anliker
tags:
  - scad
---

# Containerised Applications

- Supported by most FaaS, PaaS and all CaaS systems.
- Portability through images
- Integration of arbitrary executables, processes, protocols
- Isolation of long-runing processes

## Source to Container

Division of hardware resources by implementing many instances with isolation properties. 

Standard image spec: Open Container Initiative (OCI)

LXC
- Linux specific, vendor neutral
- boot from directory
- namespacing of system resources in the Linux kernel
- integrate with python lib

Rocket
- CoreOS / RedHat
- book from AppC or Docker container image
- namespacing of system resources as processes without daemon
- integrate with systemd

Docker
- Docker Inc
- boot from Docker container image
- interpretation from Dockerfile
- namespaceing of system resources as processes through central daemon
- integrate with composition and computing tools (k8)


## Container Composition



## Container Ecosystem