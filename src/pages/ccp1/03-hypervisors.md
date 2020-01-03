---
path: "/ccp1/hypervisors"
title: "CCP1 Cloud Compute (Hypervisors)"
date: "2020-01-03"
draft: true
author: Simon Anliker
tags:
  - ccp1
  - cloud
---

<!-- CCMP1 -->

# Compute Resource Virtualization

Primariy goal is improving utilisation. Physical resources are used as a pool for logical resources.
- Consolidate to reduce hardware cost
- Flexible and responsive
- Optimisation of workloads

Logical resources:
- Compute
- Storage
- Networking

Compute resources: CPU, RAM, Caches, Interrupts, Timers, motherboard facilities, IO facilities


## Basic Concepts

Forms of abstracting logical resources from a physical resource pool.

- Simulation: Model of the actual system with no direct link to the actual system (model system behaviour in code)
- Emulation: Approximate behaviour of actual system but not necessarily based on its implementation (converting code to hardware architecture)
- Virtualization: Approximate the behaviour of the actual system by using pars of the implementation of the actual system (slicing resources)


### Simulation

Simulate what a system does, not how

### Emulation

- User-mode/environment emulation: Portability of processes e.g. Wine (run Windows software on Linux)
- System-mode emulation: Portability of architectures e.g. Quick Emulator (run PowerPC systems on X86 architecture)

### Virtualization

Uses different approaches to provide logical IT resources: Slicing, partitioning, aggregation, emulation, hardware extension

Hypervisor/Virtual Machine Monitor (VMM): Software component to provide and manage an envrionment that offers logical resources. An envrionment must have these properties:
- Equivalence/Fidelity: Behaviour should be identical with running on a machine directly
- Resource control/Safety: VMM must be in control of the virtualized resources
- Efficiency/Performance: A statistically dominant fraction of instructions must be executed without VMM intervention 

Virtual Machine (VM): Instance of logical resource

Virtual Machine Image (VMI): Image with virtual disk and bootable OS installed on it

Virtualization levels:
- Full-Virtualization
- Para-Virtualization
- OS-level Virtualization: Containers
- Application-level Virtualizarion: JVM


#### Type I

Baremetal Hypervisors - runs on top of host hardware, full controll of hardware

Hardware -> VMM -> Guest OS (VM1..n)

Pros:
- Better performance, scalability, stability
- Ho host OS to maintain

Cons:
- Hardware support limited, hypervisors have limited device drivers built-in
- Not suitable for workstations

Examples: VMware ESX and ESXi, Microsoft Hyper-V


#### Type II

Runs on top of the host OS

Hardware -> Host OS -> VMM -> Guest OS (VM1..n)

Pros: 
- Host OS provides hardware drives
- Flexible deployments
- On-demand start/stop
- Parallel host OS
- Parallel VMMs

Cons:
- Host OS as performance bottleneck
- Wider system-breach surface
- Anything that relies on host OS

Examples: Oracle VM VirtualBox, Microsoft Virtual PC
