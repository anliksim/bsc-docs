---
path: "/ccp2/cloudpatterns"
title: "CCP2 Cloud Patterns"
date: "2020-03-28"
draft: false
author: Simon Anliker
tags:
  - ccp2
  - cloud
---

<!-- CNA2 -->

# Cloud Native Patterns

Cloud-native application design (CNA) - consequence from cloud computing principles and distribution

CNA can be applied with architectural patterns

Service and service instances

## Service Registry

Maintains service instances and state info - where to reach the service

Example: Netflix Eureka

Offers REST-based interface and Java based server and client library

Example: etcd

Distributed key value store
Highly available, raft consensus algorithm


## Circuit Break

Deal with unavailable or unresponsive services, fail fast

Client side or standalone

Closed (healthy)
* passes on every request
* increase counter on failure
* change to open if threshold reached

Half-open (recovering)
* pass some requests
* if request fails - back to open
* after a certain amount of successful requests - close circuit

Open (faulty)
* immediately return an error
* switch to half-often
  * either after some time
  * or send health requests periodically 

## Load Balancer

Implement elasticity

Horizontal scaling, add or remove parallel resources based on load

Distribute load over pool of services

Server-side LB:
* Service in separate process
* Can be hardware or software
* e.g. HAProxy, nginx, F5

Client-side LB:
* Library within client
* Requires service registry
* e.g. Netflix Ribbon

Distribution algorithms goals:
* Fairness, equal load
* Speed, unequal in relation to capacity planning
* Economics, unequal to save expenses

Distribution algorithms:
* Round-Robin: fairness, assign in turns
* Least-Connection: performance, assign to least on-going request
* Source: stickiness, same as former request

Theoretical limit - Amdahl's Law, Universal Scaling Law (Gunter)

## API Gateways

Present API consistent to consumers

Internal structure of CNA should not be exposed

Use a gateway service that provides an unified interface and forwards request to internal service (GOF facade)

Advanced: client-specific variants (browser, mobile)


## Endpoint Monitoring

Track operational status

Typical checks:
* Resource consumption, load
* External backing services
* Latency, processing time
* Expiration of certs

## Health manager

How to make sure desired number of instances are operational

Desired vs actual state

Monitor health status and automatically restart failed components


## Queue load-leveling

Deal with variable levels of load

Requests received at variable rate, messages processes at a more consistent rate

If peaks are excessive, request can be dropped (policing)

Competing consumers/producers - consumer service pool processing messages from message queue

## Event-Sourcing

Track state and provide transactional features in a highly distributed system

Store stream of events instead of directly updating the current state

Represent the current state by replaying events, use materialized view to aggregate

Provides eventual consistency for transactional data

Gives full audit trails and history


## CQRS

Command query response segregation, to optimize read and write operations

Provide separate paths for write (commands) and reads (query)

Overload of one path should not influence the other, can be scaled independently

