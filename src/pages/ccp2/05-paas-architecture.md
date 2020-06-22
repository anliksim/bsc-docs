---
path: "/ccp2/paasarchitecture"
title: "CCP2 PaaS Architecture"
date: "2020-06-22"
draft: false
author: Simon Anliker
tags:
  - ccp2
  - cloud
---

<!-- ARCH -->

# PaaS Architecture

Functional separation of concerns:
* Extended: app model, life-cycle, testing, analytics
* Core: application, backing service, networking, monitoring, multi-tenancy
* Operation: performance, availability
* Infrastructure: IaaS

## Core architecture

Runtime management:
* Manages creation, caching, scheduling, placement, disposal

Networking: 
* Routing, load balancing
* Handles all traffic (data plane)
* Routes incoming traffic to internal service

Cloud controller:
* Provides mgmt API to the user
* Mgmt and control plane
* Interface for tooling
* Manages lifecycle of apps
* Handles state transitions

Health manager:
* Monitors state of apps
* Compares desired vs actual state (listens to state messages from runtime mgmt)
* Orchestrates, corrects state if different (through the cloud controller)

Messaging bus:
* Central communication system
* Pub-sub based
* Protects itself, relied upon by the whole system

Backing services:
* Marketplace / service broker
* Bind, unbind services to apps

Logs and metrics:
* Log-aggregation from all app instances
* Emits system events
* Emits metrics

Ops support and mgmt system:
* Access control
* Rating, charging, billing


## Principles

SOA/CNA principles:
* Loose coupling
* Event-driven
* Idempotent
* Asynchronous, non-blocking
* Eventually consistent
* Language-agnostic

Control loops: observe, rectify, repeat

Declarative vs imperative

Think big, economies of scale

Cattle vs pets

Open vs closed

Legacy compatible


## Requirements

No single point of failure

Distributed state

Self healing

Horizontally scalable

Dynamically discoverable components

Loose coupling, distributed components

Monitor all components using defined endpoints


## Openshift

Runtime based on Kubernetes with additional mgmt functionality
* Multi-tenancy
* Build-tooling: Git SCM, app staging
* Service-layer: Docker container registry, service catalog

Persistent storage with volumes

Networking for tenant based isolation and routing strategies

Components:
* K8s master: manage state of the system
* Kubelet: agent to control K8s nodes, starting/stopping containers based on master definitions
* Kube proxy: access between containers
* OpenShift master: REST endpoint to interact with the system
* etcd server: store system config and state
* Controllers: watch and make sure system matches desired state in etcd, e.g. DeploymentController, BuildController


## CloudFoundry

BOSH: automation tool to manage lifecycle of complex distributed systems
Container Runtime: Kubernetes cluster managed by BOSH
Application Runtime: Elastic runtime environment for CNA based on Diego

(looking at application runtime)

Diego container runtime management system:
* Diego brains: management
* Diego cells: running apps

Cloud controller:
* Ctrl plane for app lifecycle
* Mgmt plane for backing services
* Multi-tenancy

Storage and messaging:
* Consul for long living metadata
* Diego BBS (bulletin board system) for real-time state
* BlobStore for buildpacks
* NATS for messaging between components

Services:
* Marketplace and service broker to provision, bind, destroy backing services

Types of processes in Diego:
* Tasks: run at most once with a finite duration
* LRP (long running process): runs continuously and may have multiple instances

Process mgmt:
* Rep: API to cell, manages lifecycle, participates in auctions
* Executor: runs processes based on specific recipes
* Garden: API to container runtime
* Runtime backend: container runner implementations (Garden-Linux, Garden-Windows,...)

Components:
* Brain: master node
* Cell: executor nodes running processes
* BBS (Bulletin Board System): keep state
* Access (VM): provides external access

Scheduling and placement:
* Auction-based
* Task-auction: run a one time task
* LRPStart-auction: start additional LRP instance
* LRPStop-auction: stop existing LRP instance
* BBS asks Brain-Auctioneer asks Cell_reps about current capacity, Cell-Reps reply with bid to start/stop instances, Auctioneer uses Reps response to make decision, Reps wining the auction start/stop the instances

CF binary lifecycle:
* Buildpack apps, Docker apps, Windows apps
* Builder: staging process
* Launcher: runs LRPs
* Healthcheck: status checks for running LRPs

Diego integration in CF
* doppler & traffic controller: log aggregation and metrics
* cc-bridge: adapter between cloud controller and Diego BBS
* route-emitter: adapter between gorouter and Diego BBS 
