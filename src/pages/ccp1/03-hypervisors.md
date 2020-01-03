---
path: "/ccp1/hypervisors"
title: "CCP1 Cloud Compute (Hypervisors)"
date: "2020-01-03"
draft: false
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


## Virtualizing X86

X86 architecture supports resource management and access rights to memory and hardware via privileged CPU instructions.
- Ring 3: Applications
- Ring 2: Device drivers
- Ring 1: Device drivers
- Ring 0: Kernel

Virtualizing privileged instructions is complicated. An OS within a VM needs to be prevented from directly executing privileged instructions i.e. not execute instructions in Ring 0.

### Binary Translation

Traps sensitive instructions and translated kernel code in real-time to replace non-virtualizable instructions with new instructions that have the intended effect on the virtual hardware.

- User level code is executed on the processor
- VMM provides VM all needed service (virtual BIOS, devices, memory management)
- The guest OS is not aware of the virtualization
- Very heavy


### Hardware Extension

Privileged instructions run with a new CPU execution mode feature. Virtualization with Hardware Extension is known as Hardware Virtual Machines (HVM).

- VMM runs in a new root mode below Ring 0
- Privileged calls are trapped
- Requires not binary translation or paravirtualisation
- Guest state is stored in Virtual Machine Control Structures (on Intel VT) or Virtual Machine Control Blocks (on AMD-V)

### Memory virtualization

VMM maintains Shadow Page Table. MMU looks directly into the Shadow Page Table for virtual address accesses from the guest OS.

Two types of memory address translations:
- Virtual to Physical: VM Page Table of virtual machines OS
- Physical to Host: Managed by VMM (Extended/Nested Page Table)

Virtual and physical addresses are per virtual machine. Host addresses are per physical host.

Hardware Extensions:
- Intel Extended Page Tables (EPT)
- AMD Rapid Virtualization Indexing (RVI)


### Full-Virtualization

Complete simulation of the underlying hardware. Largely dependent on computer architecture.


### Para-Virtualization

Execute Hypercalls (sensitive instructions) via new interface (HCI) between VMM and OS. That way the guest kernel knows that it runs in a VM and can directly access hardware features.

AWS uses PV on HVM drivers (i.e mix para with full) for same or better performance than with paravitualisation.


## VMM Overview

|Name|Virt. Type|Installation|Guest Arch|Openstack|
|:---|:---|:---|:---|:---|
|KVM|Full|Bare metal|Same as host|Default|
|QEMU|Emulation|Hosted|x86(-64),ARM|Yes|
|Xen|Para/Full(HVM mode)|Bare metal|Same as host|Yes|
|VMware ESXi|Para/Full(HVM mode)|Bare metal|x86(-64)|Yes|
|Microsoft Hyper-V|Full|Hosted|x86-64|Yes|

### QEMU

Quick Emulator (QEMU) is a generic and open source machine emulator and virtualizer based on Binary Translation and SoftMMU
- Full-system emulation
- User-mode emulation
- Virtualization (runs KVM and Xen VMs)

### KVM

Kernel-based Virtual Machine (KVM) is a Linux based open source hypervisor that was built as a cheap alternative to Intel and AMD vurtualization extensions.

Provides an interface to the Linux kernel via kernel module. CPU and Memoy access is exposed via `/dev/kvm`. The VM is implemented as regular Linux process. 

KVM does not perform any emulation or virtualization. 

#### KVM-QEMU

Combining KVM and QEMU gives the guest OS:
- Resource scheduling: Through the host OS scheduler
- Huge Page table (HPT) support: Handle large amounts of (v)RAM
- Non-uniform Memory Architecture (NUMA): Optimized RAM access
- Kernel Same-page Merging (KSM): Memory deduplication for VMs
- Integrates CGroups: Controlled access and allocation of system resources
- Integrates network namespaces: Controlled visibility and minimum network guarantees per VM

To start a VM:
    
    qemu-system-x86_64 -enable-kvm [diskimage]

### Libvirt

Libvirt is a hypervisor agnostic, multi-language virtualization library managing VMs.

Virsh is the virtual shell for for managing VMs based on Libvirt

Libvirtd is a deamon service for managing guests and virtual networks based on Libvirt

Virtual Machine Manager (virt-manager) is a desktop interface for managing VMs based on Libvirt

Libvirt is used extensively by OpenStack Nova


Creating a VM with Virsh (requires a definition file e.g. `my_vm.xml`):

    virsh create my_vm.xml
    virsh start my_vm.xml
    virsh define my_vm.xml

    