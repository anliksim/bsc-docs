---
path: "/scad/dependencies"
title: "SCAD Service Dependencies"
date: "2020-01-05"
draft: false
author: Simon Anliker
tags:
- scad
---

# Service Dependencies

dependee A depends on dependency B

dependency B is depended on dependee A

- static vs dynamic
- visible vs hidden
- explicit vs implicit
- strict vs tolerant

Dependencies in software

- code (executable)
- configuration (declarative)
- data (binary)

Importance

- if B goes down, A goes down too
- hidden cost

Dependency manifestation

- tight coupling / fixed binding
- loose coupling / late binding

Dependency type

- inter-service
- intra-service (e.g. libs)

Dependency consideration

- transitivity (recursive algorithms)
- data sources (DaaS model)
- deployment (dependency affinity)
- security
- consistency (strict, sequential, casual, eventual)

## Dependency analysis

### Matrix

Matrix structure for binary or qualified binary depends/not-depends relationship:

```
  1 2 3 n
1 .   .
2   . . .
3 .   . .
m       .
```

### Graph

Associative graph structures: Directed acyclic, weighted or unweighted graph:

```
ServiceA -> ServiceB -> ServiceD
            ServiceC ->
```

Slower but more space-efficient than matrix and sub-structure capable.

### Dependency Resolution Techniques

A depends on B if

- declared as such
- A invoked B (traceable)
- logic of A depends on state withing B (non-traceable)

Analyse statically (description) or dynamically (execution).

Dynamic analysis requires

- correlation vs causation
- small data sets
- clustering of services

#### Peddycord

Passive network monitoring and analysis -> Network service dependency miner

Logarithm based ranking scheme -> Ratio weight(A->B)/weight(A) or logarithm log(weight(A))*weight(A->B)

Frequency inference -> confidence in dependency candidates

## Dependency resolution

Immediate resolution

- simple transitive inclusion
- solution not guaranteed

Interactive resolution

- with feedback
- iterative improvements
- may complement immediate resolution upon escalation

## Composite services

Offer a single service interface and distribute requests to multiple services
within the composition (parallel, serial or more complex routing): requires
knowledge of dependencies.

Advantages of services

- higher availability (QoService)
- flexibility to switch (QoExperience)

### Techniques

Service Orchestration

- Multiple services form another one
- Centrally managed and executed
- Based on workflows that require service interfaces
- Examples:
  - Business Process Execution Language (BPEL)
  - Yet Another Workflow Language (YAWL)
  - Dynamic Service Orchestration Language (DSOL)
  - AWS Step Functions

Resource Orchestration

- Workflows require resources
- Resource allocation performed centrally
- Examples:
  - Heat Orchestration Template (HOT)
  - Docker Compose
  - Kubernetes Descriptors

Choreography

- Interaction protocol between autonomous services
- No central point of control
- Difficult decentralised enactment
- Example:
  - BPMN 2.0
  - Chor
  - CHOReVOLUTION

Bundling

- Multiple services used in a bundle
- Less administrative overhead
- Less flexibility for exchanging single services
- Example:
  - USDL

Multiplexing

- Multiple services used in parallel handling partial requests
- Flexible redundancy systems
- More administrative overhead
- Higher cost due to multiple candidates

Mashup
Service Bus/Mash

## Artefacts

Bundles: WAR, SAR, BPR, ZIP, Helm charts

Executable code: `.class`, `.py`

Service descriptions: WSDL, ...

SLA templates: WSLA, WS-Agreement

Deployment descriptors: HOT, PDD, web.xml, k8s yaml/json, Help templates

Dependencies by reference (e.g. Docker Compose): need resolution before execution

Dependencies by embedding (e.g. Helm charts): no resolution needed but transitivity
