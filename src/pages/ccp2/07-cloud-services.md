---
path: "/ccp2/cloudservices"
title: "CCP2 Cloud Services"
date: "2020-06-22"
draft: false
author: Simon Anliker
tags:
  - ccp2
  - cloud
---

<!-- CSRV -->

# Cloud Services

Web/cloud service
* discoverable endpoints
* encapsulated implementation
* well-defined interface
* messaging protocol plus respective informtion model

Internal services: can be discovered and enactet with simple API calls

External services: need more effort to signup, authentication, usage tracking

Service orientation: decomposition into services plus process of describing , publishing, finding and binding services 

Service registry: entity to publish and find services via their descriptions

Phases:
* Service registration: publish
* Service discovery: find, kind of service (functional) and service ranking (non-functional)
* Service consumption: bind

Service measurement index (SMI)
* accuracy
* functionality
* suitability
* interoperability
* response time

Registries in practice:
* Broker: registry + service enactment (open broker API)
* Repository: service impl + registry (Docker Hub)
* Catalog: registry + presentation UI (Programmable Web)
* Marketplace: catalog + accounting

Service Broker Categories
* Global level: globally across providers
* Provider level: within one platform
* Tenant level: shared, user provided

Service broker only manages service instances (control plane), communication between apps and service instances is direct (data plane)

## Open Service Broker API

Entities:
* Marketplace: platform component manging the services
* Service Broker: manages the associated services
* Service Class: service impl providing the functionality
* Service Instance: running instance of the service
* Service Binding: info to access the service instance

Functions:
* Catalog: list of service descriptions
* Service Plan: manifestation of the service regarding attributes
* Provision: create and configure service instance
* Bind: make service instance available to app
* Unbind: disconnect
* Deprovision: delete