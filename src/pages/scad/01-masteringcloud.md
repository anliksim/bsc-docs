---
path: "/scad/masteringcloud"
title: "SCAD Mastering Cloud"
date: "2020-01-04"
draft: false
author: Simon Anliker
tags:
  - scad
---

# Mastering Cloud

## Computing Paradigms

Computing paradigms (___ Computing)
- Distributed
    - Client-Server
    - Federated
    - Decentralised
    - Disconnected
- Service
- Cloud 
- Utility
- Serverless

Application-level paradigms
- Cloud-native
- Serverless


## Services

Technical characteristics
- Visibility: Well-defined, unified description (declarative, interactive)
- Interaction: Well-defined, unified interface/protocol  (message exchange patterns, layer)
- Effect: Invocation semantics (idempotent, reversible on system, irreversible in real-world)
- Encapsulation: Encapsulate software and resources for re-use
- Interoperability: Loose coupling (late/dynamic binding)

Lifecycle characteristics
- Find (search, rank, select)
- Bind (signup, subscribe, pay, use)

```
    Service    
    ├── private
    ├── public
    │   ├── non-profit
    │   ├── commercial
    │   │   ├── single-user
    │   │   │   ├── standardised
    │   │   │   ├── proprietary
    │   │   ├── multi-user
```


### Ecosystem

Service-oriented architecture
- Service orchestration or choreography
- Services, clients, brokers, marketplaces, messaging systems
- Service platforms, development tools

Web services (HTTP)
- RESTful, resource-centric (CRUD)
- SOAP, operation-centric (POST)

Content types and schemas
- Structured (JSON, YAML, XML)
- Plain text
- Binary documents
- Streams

Cloud services
- On-demand
- Pay-per-use
- Elasticity
- Robustness

Microservices
- Lightweight (e.g. single-function)
- Tangible & portable implementation

Scale
- Y-Axis: Scale by using microservices (functional decomposition)
- X-Axis: Scale by cloning services (horizontal duplication)
- Z-Axis: Scale by sharding databases (data partitioning)


## Cloud-native & Server-less

Cloud-Native Computing: Computing paradigm optimised for modern distibuted systems
- ultra-scaling
- self-healing
- multi-tenant
- containerised
- dynamically-managed
- microservices-oriented
- resilient
- elastic
- virtualised
- loosely coupled
- abstracted
- adaptive


### FaaS

- running functions in the cloud
- real pay-per-use


