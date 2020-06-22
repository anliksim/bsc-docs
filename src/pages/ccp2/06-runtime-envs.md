---
path: "/ccp2/runtimeenvs"
title: "CCP2 Runtime Environments"
date: "2020-06-22"
draft: false
author: Simon Anliker
tags:
  - ccp2
  - cloud
---

<!-- RUNT -->

# Runtime Environments

Containers as units of management
* Relieves Dev and Ops from worrying about machine and OS details
* Flexibility to roll out new hardware and upgrade OS with minimal impact on apps
* Ties telemetry to apps rather than machines

Runtime: component executing app (e.g. Interpreter - Python, Node, Virtual machine - JVM, .Net core)

Runtime environment: contains required components to run app

Staging: process of packaging application in runtime environments

Builder tools:
* Heroku buildpacks
* Docker
* Source to image (s2i)

Buildpack API:
* bin/detect (analysis): determines if buildpack is applied
* bin/compile (droplet): transformation steps, compile and build
* bin/release (metadata): provide metadata back to the runtime, return yaml manifest

Staging workflow:
* Prepare environment
* Analysis 
* Droplet
* Metadata


Advantage using Docker
* Very flexible
* Very extensible
* A lot of base images exist
* Easy to extend and adopt

Disadvantages using Docker
* Difficult to control image sources
* Allows to do dangerous things
* A lot of work to build generic base images
* Images can get huge



