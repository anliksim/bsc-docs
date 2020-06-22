---
path: "/ccp2/contdeploy"
title: "CCP2 Continuous Deployment"
date: "2020-06-22"
draft: false
author: Simon Anliker
tags:
  - ccp2
  - cloud
---

<!-- DVOP2 -->

# Continuous Deployment

Continuous - to deploy frequently and in smaller increments
* Faster time to market
* Minimize risks
* Improve product quality

Main driver: automation

Phases of automation pipeline:
* Build automation
* Continuous integration
* Continuous delivery
* Continuous deployment
* DevOps

Multi-stage pipeline:
* Dev: check, build, package
* Test: automated and user acceptance testing
* Staging: pre-production testing
* Production

Tooling:
* Version control system
* Artifact repository
* Build server
* Automation agent
* Monitoring infrastructure
* Secure store


## Jenkins

Traditional CI-server

Concepts:
* Pipeline: user-defined model of a pipeline
* Node/Agent: worker executing a pipeline
* Stage: conceptual distinct subset of tasks
* Step: a single task to execute

