---
path: "/scad/serverlessworkflows"
title: "SCAD Serverless Workflows"
date: "2020-01-04"
draft: false
author: Simon Anliker
tags:
  - scad
---

# Serverless Workflows

## Concepts

- Formalised processes
- Connection of two or more steps

Connection patterns
- Sequence / chain
- Parallelism
- Conditions
- Split and join
- Complex state machine

Description language
- BPMN
- BPEL

Primitives (explicit)
- ordering
- control flow
- selectors
- data transformation

Features (implicit)
- DAGs (validation, optimisation, visualisation, editing)
- checkpointing
- monitoring, notification
- connectors, integration


### Models

Traditional model: Engine -> Queue -> Worker VMs -> Storage

Queue model: Engine -> Queue -> Worker Cloud Functions -> Storage

Direct executor model: Engine -> Worker Cloud Functions -> Storage

Bridge model: Engine -> Queue -> Worker VMs + Bridge to Worker Cloud Functions -> Storage

Decentralized model: Cloud Functions -> Storage


## Systems and languages

- devlarative vs imperative
- JSON vs YAML vs JavaScript
- indetification of functions
- handling of errors, I/O

|Metrics|Amazon Step Functions|IBM Composer|Azure Druable Functions|
|:---|:---|:---|:---|
|ST-safe (composition as functions)|No|Yes|Yes|
|Programming model|DSL(JSON)|Composition library (JS)|async/await (C#)|
|Reflective API|Yes(limited)|No|Yes|
|Parallel execution support|Yes(limited)|No|Yes(limited)|
|Software packaging and repositories|Yes|Yes|Yes(no repo)|
|Billing model|$0.025 per 1000 state transitions|Orchestrator function execution|Orchestrator function execution + storage cost|
|Architecture|Synchronous client scheduler|Reactive scheduler|Reactive scheduler|

## Optimisation

- User (workflow author) optimisation: Code
- System (workflow engine) optimisation: Processing

Workflow preparation/rewriting: Archieve series-parallel structure for polynomial execution time from directed asyclic graphs

Function execution
- Resource re-use 
    - Keep function alive (full function reuse) vs spawn new functions (full isolation)
    - Long keep-alive -> more warm executions / short keep-alives -> less idle resources
- Runtime pooling
    - large pool / handle high concurrency -> increases resource overhead / better performance
    - minial pool / fast pool exhaustion -> minimize pool; less idle resources / minimize cost


Workflow execution-specific
- Prefetching: Fetch function source upfront
    - Higher latency -> less storage cost
    - Lower latency -> more storage cost
- Prewarning: Predictive function execution (via runtime analysis and function stats)
    - Optimistic prewarning / low treshold -> more performance
    - Pessimistic prewarning / high threshold -> less cost












