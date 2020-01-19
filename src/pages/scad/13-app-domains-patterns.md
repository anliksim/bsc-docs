---
path: "/scad/domains-and-patterns"
title: "SCAD App Domains and Patterns"
date: "2020-01-19"
draft: false
author: Simon Anliker
tags:
  - scad
---

# Application Domains and Patterns

## Service domains

Robotic services 
- Device constraints, lifecycles, messaging models
- Platform and runtime differences
- Non-standardised integration of cloud and non-cloud parts

Mobile service
- Sweetspot through controlled mobile -> coud offloading
- Increasing support by cloud providers

Multimedia services
- Long history of standardisation including complex communication patterns such as sessions or broadcast
- Limited but increasing support by cloud providers


## Microservices and container patterns

1-Node, 1-Container
- Upward: metrics, profiling
- Downward: lifecycle adherence
- Containers: resource accounting and allocation, packaging, deployment, reuse, failure containment boundary

1-Node, n-Containers
- Sidecar
    - Pickup car: Log transportation
    - Parcel car: Serving sync content
    - ACS car: Repair
    - Toll car: Service discovery
- Ambassador: proxy and represent
- Adapter: normalise and present

m-Nodes, n-Containers
- Leader election: voting and determining leader
- Work queue: coordinator and grabbers
- Scatter/gather: distribution of requests

Operator pattern
- Container managing aspects of other containers (extension of sidecar)
- Commission, decommission, upgrade, downgrade, configure, coordination, backup
- e.g. Kubernetes operator framework