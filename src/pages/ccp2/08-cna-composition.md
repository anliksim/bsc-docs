---
path: "/ccp2/cnacomposition"
title: "CCP2 Cloud Native Application Composition"
date: "2020-06-22"
draft: false
author: Simon Anliker
tags:
  - ccp2
  - cloud
---

<!-- DVOP1 -->

# Cloud Native Application Composition

Decomposition is key enabled of SOA and microservice architecture: reduce apps into a set of independent functional services

Domain driven design (DDD)
* Context: setting in which a concept appears
* Domain: specific area of knowledge or activity
* Model: system of abstraction that describes aspects of a domain
* Ubiquitous language: structure around domain
* Bounded context: explicit definition of the context

Define services corresponding to sub-domains:
* Core: key differentiator for the business
* Supporting: related to business but not a differentiator (in-house or outsourced)
* Generic: not specific to business (ideally off-the-shelf software)


## Application Composition

Centrasilded:
* Common
* TOSCA, Docker compose
* Great for management, global control, oversight
* Poor for scalability

Decentralised:
* Less common
* Habitat.sh, DNS, BGP, P2P
* Great scalability
* Difficult to implement correctly and to control

Orchestrated:
* centralised 
* one participant
* global config

Choreographed:
* decentralised
* multiple participants
* local config

Declarative:
* Runtime modification difficult
* Debugging difficult
* Language independent
* Requires document structure
* Typical approach

Imperative:
* Runtime flexible
* Can be debugged
* Explicit language, DSL
* Not very common


## Standards

Industry standards: defined and driven by industry groups, slow process (TOSCA, CAMP; WS-BPEL)

De facto standards: defined by entities and quickly, widely  adapted by community (Docker Compose, Helm, Kubernetes)

De jure standards: driven by government related entities, industry or de facto can become de jure (TCP/IP, ASCII)


### TOSCA

who cares

### Helm

Package manager for Kubernetes, bundles K8s manifests into charts

Key concepts:
* Chart: a bundle of K8s resources
* Release: chart instance loaded into K8s
* Repository: charts repository like Docker Hub
* Template: K8s config mixed with Go/Sprig templates


### Docker Compose

Define multi-container application in a single file

Can be used with a single Docker engine or a clustered Docker Swarm

Model:
* yaml file
* version
* services
* volumes
* networks