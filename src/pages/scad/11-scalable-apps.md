---
path: "/scad/scalability"
title: "SCAD Scalable Cloud Apps"
date: "2020-01-18"
draft: false
author: Simon Anliker
tags:
  - scad
---

# Scalable Cloud Application


Cloud-Native Application (CNA): Fully exploiting cloud services and platform features
- elastic scalability
- resilience
- confidentiality
- portability
- cost-efficiency / pay-per-use

## Scalability

Adapt to demand

Manual- vs. auto- vs. prescaling

Properties
- Elasticity (time): degree of adaption
- Transformativity (space/behaviour): self-similarity, bottlenecks
- Boundedness (space): upper/lower bounds

Mismatches
- Billing cycle
- Memory allocation
- Dynamic profile

Scaling for app packages/functions
- Indirect through runtime
- Configurable

Vertical Scaling 
- Hypervisor memory (e.g. KVM `setmem <xG> --live`)
- Hypervisor processor (e.g. `cpu_set <n+1> online`, DVFS tuning)

Vertical Scaling for Containers
- Static resource limits (Docker, Kubernetes descriptors)
- ElasticDocker: Feedback loop for memory + CPU assignment (MAPE-K)

Horizontal Scaling 
- Parallel, single host: Multiple processes and threads, limited by # CPU cores
- Distributed, multiple hosts: Multiple instances over the network, limited by # nodes.

Horizontal Scaling for Containers
- Amdahl's law: Decreasing effectiveness of scaling factor
- Optimal scale detection: Beforehand (prescaling) vs later (costly)
- Optimality: Determine sweetspots (minimum cost , minimum makespan -> minimum weighted utility)


Diagonal Scaling
- Solution to issue in public clouds for limited granularity x slow scaling speeds x flat pricing schemes
- Increase in savings vs application loading


## Autoscaling

- Realisation of on-demand provisioning
- More resouces/processes on one node for vertical scaling
- More instances/replication for horizontal scaling

Taxonomy: Autoscaling
- reactive
    - app--agnostic
    - app-specific
- proactive
    - app-specific

Reactive
- chaotic over-/underprovisioning
- risk of SLA violation

Proactive
- controlled
- SLAs maintained


### Prediction

AR: Autoregression

MA: Moving Average

ARMA: Combined AR/MA

Polynomial & Spline Interpolation

Time series data -> _predictor_ -> user prediction -> _resource planner_ -> resource demand -> _action plan generator_ -> action plan for scaling

Predictors (users/time)
- Baseline: Use current value
- Nearest neighbor: Look back to nearest neighbor bit same value and repeat pattern for the given timeframe
- Lagged neighbor: Look back to lagged neighbor defined by a period and repeat pattern for given timeframe


## Scalable App Design

### Multi-Tier

Characteristics
- Popular for web apps and client-server
- 3-tier: frontend, backend, DB
- 4-tier: client/frontend, delivery, aggregation/logic, services

Advantages
- Simple monolithic blocks
- Established standards for protocols
- Sufficient flexibility for complementary blocks

Disadvantages
- Inequal loads and bottlenecks
- Only applicable in isolated applications


### Service-oriented

Characteristics
- Service orientation
    - Uniform description
    - Communication
    - Encapsulation
    - Reuse
- Discovery
- Flexible dynamic bindings

Advantages
- Consequent evolution of complex software design
- Viable basis for rapid prototyping of services

Disadvantages
- Waning support for classical SOA technologies
- No guidance for how to design, build, run services


### Cloud-native

Characteristics
- Scalability 
- Resilience
- Refines service orientation with microservices
- Exploits capabilities of cloud environments
- Stateless microservices: self-management, self-healing, autoscaling
- DBaaS

Advantages
- Closest to ideal scalability
- Elastic, hardly bounded/transformative
- Matched by contemporary technologies
- Conversion of legacy applications possible

Disadvantages
- Technological immaturity and volatility
- Emerging tools and languages
- Incompatibilities
- Insufficiencies


### Messaging

Diverse types of messaging systems
- Message queues
- Message brokers
- Stream processing
- Pub/sub
- Gateways
- Service buses
- Microservice mesh
- with billing by # messages, payload size, mgmt calls


#### Kafka

- Sharding + threading: Inverse of Amdahl's law
- High availability if partition replias are on different fault and update domain.


### Coordination

Consensus, voting, leader election, synchrony
- Coordination faults
- Byzantine faults/attacks

Majorities
- None 
- Relative (n > m for all n,m in N)
- Absolute (n > N/2)
- Byzantine (n > 2N/3)
- Consensus (n == N)

Paxos Consensus
- Requires absolute majority
- Extension: Byzantine Paxos
- Roles: Voters/acceptors, learners, proposers, leader
- Depends on leader election
- Leader elected by successful proposal
- Otherwise battling leaders
- Depends on redundancy
- Ignores failures of redundant participants

Swedish Leader Election
- Based on rounds of elimination of candidates
- Flop coin - winner proceed into next round, otherwise play again
- Last remaining candidate is leader
- Good characteristics

Raft
- Recent, modular algorithm
- Uses log replication
- Random timeout for followers -> become candidates
- Absolute majority of votes -> leader
- Heartbeats define election term

Implementations
- Zookeeper: Zab algorithm (Zookeeper atomic broadcast), similar to Paxos but with stricter ordering guarantees
- Etcd/Consul: Distributed service discovery and configuration with Raft
- Microsoft Distributed Mutex (C#): Leader election, custom algorithm

CAP theorem: focus on CA microservices

BAC theorem: when backing up microservices, it it not possible to have both availability and consistency

Summary
- Coordination, data distribution, autoscaling
- Self-* characteristics
- Cloud-native components


### Distributed Microservice Scalability

Performance benchmark
- Performance must remain high
- Not entirely linear (Amdahls law)
- FaaS 1:1 event:instance
- CaaS n:m (many events, few instances)

OpenShift / APPUiO

    oc scale --replicas=3 dc app
    oc autoscale app --min 1 --max 10 --cpu-percent=80
    

IBM Cloud

    cf aasp app policy.json
    

GCP

    gcloud compute instance-groups managed set-autoscaling app-igroup --max-num-replicas 10 -target-cpu-utilization 0.92 --cool-down-period 30
    
    
### Fallacies

- The network is reliable: App needs error handling and retry
- Latency is zero: App should minimize # of requests
- Bandwidth is infinite: App should send small payloads
- The network is secure: App must secure its data/authenticate requests
- Topology doesn't change: Changes affect latency, bandwidth & endpoints
- There is one administrator: Changes affect ability to reach destination
- Transport cost is zero: Cost must be budgeted
- The network is homogeneous: Affects reliability, latency & bandwidth