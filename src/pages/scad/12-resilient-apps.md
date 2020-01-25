---
path: "/scad/resiliency"
title: "SCAD Resilient Cloud Apps"
date: "2020-01-19"
draft: false
author: Simon Anliker
tags:
- scad
---

# Resilient Cloud Application

Service Lifecycle

- Service Offering: Implementation, Description, Registration
- Service Discovery: Request, Matchmaking, Negotiation
- Service Usage: Activation, Monitoring, Optimization
- Service Termination: Withdrawal, Deactivation, Feedback

## Quality dimensions

SQuaRE (Software product Quality Requirements and Evaluation - ISO/IEC 25000)

- Functional suitability
- Performance efficiency
- Compatibility
- Usability
- Maintainability
- Reliability
- Security
- Portability

Formula:
_Reliability = Maturity + Availability + Fault tolerance + Recoverability + Compliance_

### Reliability (IEEE)

The portability of failure-free software operation for a specified period of
time in a specified environment.

### Resiliency

The ability of an app to recover from certain types of failure and yet remain
functional from the customer perspective.

### Service Quality Model

Non-functional characteristics (performance, cost, compliance to standards), QoS

Quality specifications

- UML with OCL (Object Constraint Language)
- CQML+ (Component Quality Modelling Language)
- OASIS/TOSCA
- SLA (Service Level Agreement) as languages (LegalXML, XACML, WSAG, WSLA, SLAng)

Quality dimensions

- Relevance
- Added value
- Confidence
- Objectivity
- Timeliness
- Consistency
- Precision
- Completeness
- Correctness

Functionality: what a service is doing

Non-functional property: How a service is doing what it is doing

Measurable quality

- Described metrics: Authoritative vs. non-authoritative
- Measured metrics: Monitoring, attestation services
- Estimated metrics: Prediction algorithms
- Static analysis: Descriptions (completeness, timeliness)
- Dynamic analysis: Measurements (correctness)

Service metaquality (SMQ): how well is a service described

SQM caclulation and decision: Table with quality dimensions and NFPs - weighted
quality dimension per metric

## Replication

Replica: Repeated software component or data

- Faster response time through multiplexing
- Improved safety through backup
- Increased storage/computation/bandwidth

Taxonomy

- partial (decomposed/erasures)
- full (n-fold)

Placement

- Equal distribution
- Proportional distribution
- Absolute distribution

Optimal algorithm

- Staggered determination
- Guarantee of availability, price, capacity
- Deadline for calculation

Data

- Singular copies
- Incremental copies, backup with deltas, Hanoi strategy
- Distributed versioned replication
- Erasure coding

### Erasure coding

c = a XOR b

- if a fails, recover a = c XOR b
- if b fails, recover b = c XOR a
- if c fails, recalc  c = a XOR b
- 50% redundancy (2 significant, 1 redundant)

d = a XOR b XOR c

- 33% redundancy (3 significant, 1 redundant)

z = erasure(a,b,...) in Galois Field GF(2)

- arbitrary redundancy

## Resilience

### Fault injection

Test before releases, find issues ->
Chaos engineering, live system, simulation/emulation

Software level

- Fuzzing: Invalid data input
- Data modifications
- e.g. jFuzz concolic whitebox fuzzer

System level

- Resource exhaustion
- Limits exhaustion
- Random process signaling/termination
- e.g. Chaosmonkey

Network level

- Artificial slowdown
- Random port blocking
- Protocol mutation
- e.g. Chaosmonkey

#### Emulation

MC-EMU: Multi cloud emulator

Behavioural models

- convergence
- incident
- random
- replay/library

Properties

- availability
- slowness
- popularity

Targets

- No-op
- Web/file server (storage)
- OS container (compute)
- L4 proxy (network)
