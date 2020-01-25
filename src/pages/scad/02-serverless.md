---
path: "/scad/serverless"
title: "SCAD Serverless Application"
date: "2020-01-04"
draft: false
author: Simon Anliker
tags:
- scad
---

# Serverless Applications

## FaaS

- Inspired by functional programming
- Stateless
- Short-lived
- Single-function(ality) microservices
- Implied pricing model: memory + per-invocation

Programming model

- Heterogenous functions with specific signature (params, return values)
- order of invocation can expressed by workflows

Deployment model

- upload source or compiled binaries
- configure entry point, memory, limits

Execution model

- time and space limits
- local state limits (by time or no state at all)
- pay-per-use microbilling
- highly parallel autoscaled exeuction

Pricing

- Determined by provider
- Determined by application through service descriptions or annotations (emerging
  topic)

### Service model

Control plane

- Deployment
- Configuration
- Authorisation
- Logging
- Billing

Computing plane

- Platform
- Compute
- Storage
- Network

### Execution

Event -> trigger -> rules -> actions -> results

Events

- Monitoring event
- Sensor data
- Log entry
- ...

Triggers

- HTTP
- XMPP
- AMQP

Rules: e.g. max 1 per hour
Actions: Function invocation
Result: e.g. JSON return value

### Cloud Functions

Structure

- fixed or registered name
- parameter or via context
- single return value

Language

- in theory any
- in practice dominance of Node and Python

Configuration

- Runtime language, entry file
- Runtime parameters, memory, max duration, env variables

12factor alignment: declarative setup, config in environment, backing services
for state handling, disposability / fast startup & graceful shutdown

### System view

- Function: Developer-supplied code (app bundle)
- Function Encapsulation: Provider-supplied container image (runtime, libs)
- Function Execution: Provider-supplied infrastructure (hidden specs)

### Configuration

- runtime environment (from list)
- memory 128-512 MB; default 256
- timeout 0.1-300 s; default 60
- authoring: template or blank document
