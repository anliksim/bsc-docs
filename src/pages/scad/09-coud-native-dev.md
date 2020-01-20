---
path: "/scad/cloudnativedev"
title: "SCAD Cloud-Native Development"
date: "2020-01-05"
draft: false
author: Simon Anliker
tags:
- scad
---

# Development for Clouds

## Approaches and Tools

Conventional approach with separate dev and ops:

- Long feedback cycles
- Source-target incompatibilities
- Slow upgrade of dev envs
- Slow adaption of new cloud-native frameworks
- Inconsistencies
- Poor transaction support or complex deployments

Development in and on the Cloud - shorten time to first dev cycle:

- Always-online requirement alleviated with distributed sync (git)
- Higher risk of vendor lock-in thus results may not be portable
- Dev/ops separation of concerns lifted

New trends: Immutable infra, NoOps, DevSecOps, Web IDEs

## Models

Service Description Languages

- WSDL
- WADL
- USDL
- RAML
- CPL: Cloud Platform Language
- ABS: Abstract Behavioural Specification
- OpenAPI/Swagger
- Jolie: Language for microservices that allows you to centrally manage interfaces
- Ballerina: Another language for cloud stuff with a focus on http services

Top-down: generate code from description
Bottom-up: generate description from code

## Publishing

Bundle for deployment

- Service implementation
- Service description
- Service configuration
- Service contract template
- Composition description
- Client package

Deployment pipeline: Local -> Dev -> Test -> QA -> Prod

Service registration: Enabler for reuse (AWS API Gateway, Azure API Management)
