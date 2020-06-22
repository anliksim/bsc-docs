---
path: "/ccp2/appmonitoring"
title: "CCP2 Application Monitoring"
date: "2020-06-22"
draft: false
author: Simon Anliker
tags:
  - ccp2
  - cloud
---

<!-- AMON -->

# Application Monitoring

Monitoring requirements:
* Instrastructure: hosts, networks
* Application: focused on app health
* User level: user experience
* Log: warnings, errors published

App monitoring params should relate to business KPIs

Frontend
* Page load time
* Real user monitoring (RUM): javascript inserted in page
* Synthetic monitoring: generate synthetic traffic loads

Backend
* App instrumentation

## Methodologies

### USE

General methodology for assessing performance of different systems

Utilization, Saturation, Error
* U: percentage over time
* S: typically queue length
* E: count per time interval

Focuses on infrastructure monitoring


### RED

Rate, Errors, Duration

Focuses on monitoring microservice requests


## Tracing 

Distributed tracing focuses on how individual requests flow through microservices application

Opentracing/opentelemetry - standard for distributed tracing
* Works with DAG of spans

Jaeger - opentelemetry compatible implementation


## Service Mesh

Istio/envoy

Sidecar pattern, controls incoming and outgoing requests




