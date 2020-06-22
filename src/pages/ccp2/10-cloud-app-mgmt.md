---
path: "/ccp2/cloudappmgmt"
title: "CCP2 Cloud Application Management"
date: "2020-06-22"
draft: false
author: Simon Anliker
tags:
  - ccp2
  - cloud
---

<!-- DVOP3 -->

# Cloud Application Management

Goal: zero-downtime deployments

## Feature flags

Features can be enabled or disabled at runtime based on config

Toggles can be boolean or encode logic:
* user/role based for piloting, A/B testing
* clients for mobile, web, fat client
* instances for canary
* geo-location for coordinated rollout
* data schema / API versions for seamless migration to new versions

Decoupling of deployment and enablement of software functionality, beta features, disable in case of problems

Types:
* Release toggles: for decoupling, temporary
* Experiment toggles: for scenario testing, temporary
* Ops toggles: for ops aspects, e.g. reduce functionality during DB migrations, temporary or long-living
* Permission toggles: enable features for limited users only, usually long-living

## Blue-Green

Two identical environments are used, green is productive, blue is used for new version

Deploy new version to blue and warm up, smoke test, if good change routing to blue, if something goes wrong route back to green, repeat.

## Canary

Rolling update - instead of switching instantly start routing some requests to new version instances.

Benefits: Easier rollbacks, A/B testing by routing some users, feedback on sanity of change before everything is routed to it

Disadvantages: Harder to mange, imposes further constraints on DB schema upgrades


## State transitions

Zero-downtime state transition is hard, e.g. database

Not an option: Stop old, migrate, start new

Impacts customer: Reduce app functionality for migration

Design the application to allow migration

Decoupling the data model:
* Use DB per microservice
* Use event sourcing and CQRS