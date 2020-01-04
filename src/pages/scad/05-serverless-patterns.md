---
path: "/scad/serverlesspatterns"
title: "SCAD Serverless Patterns"
date: "2020-01-04"
draft: false
author: Simon Anliker
tags:
  - scad
---

# Serverless Patterns

- Common language -> improves collaboration
- Common workspace -> enables best-of-breed solutions
- Common implementation -> libs, frameworks
- Common foundation -> build higher-level systems
- Simpler experience -> good practices, easy to adopt

Serverless patterns: less mature on average

## Patterns

### Basic patterns

Periodic invocation
- scheduler, cron-equivalent
- for continuous compliance at a cost

Event-driven
- reactive
- const minimisation with risk to miss events

Data transformation
- ETL-/shell-equivalent
- modularity for data augmentation

Data streaming
- load balancer-equivalent
- data partitioning (split)
- data aggregation (join)

State machine
- workflow-equivalent
- reduced data loss risk by restarts + try/except

Bundled
- combination of any patterns

### Composition patterns

Implemented only as client-side scheduling, not composable

ECA
- event-condition-action
- take next step depending on condition

Forward
- monad-style forwarding of data

Retry
- in conjunction with eventual consistency

## Antipatterns

Async calls (or rather sync?)
- Cost when A is running while waiting for B
- What if B does not reply
- Solution: use timeouts or queues

Functions calling functions
- Complex debugging, loose isolation, double cost
- Solution: merge function, if not possible push data to store or queue which in turn triggers a function

Shared code
- maintenance issue, still monolithic
- risk to hit image size limit
- warm-up time takes longe with bigger image
- Solution: write independent, decoupled functions

Shared libs as functions
- functions should do only one thing/have one purpose
- Solutions: avoid horizontal decomposition (e.g. DB connection functions)

Too many libs
- image size grows

Too many technologies
- maintenance (requires skills)

Too many functions
- maintenance, complexity

Distributed monolith

## Pitfalls

Serverless trilemma (choose 2):
- Blackbox: Functions should be considered as black boxes
- Substitution: Compositions should obey a substitution principle
- Double-billing: Invocations should not be double-billed

Composition via Fusion (f3 = {f2 + f1}) -> violates Blackbox

Composition via Asyncs (f3 => f1) -> violates Substitution (return of f3 is not return of f2)

Composition via Reflection (f3 = f2 o f1) -> violates Double-billing






