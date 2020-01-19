---
path: "/scad/serverlessdev"
title: "SCAD Serverless Development"
date: "2020-01-04"
draft: false
author: Simon Anliker
tags:
  - scad
---

# Serverless Development

## Tools

- AWS Lambda
- Google Cloud Functions
- Azure Functions
- IBM OpenWhisk
- hook.io
- OVH Functions

- Snafu
- Podilizer, Termite (FaaSification of Java)
- Lambada (FaaSification of Python)


## Overlays

## PyWren

No explicit deployment prior to execution, deploy while executing

    def my_fun(b):
        ...
        
    pwex = pywren.default_executor()
    res = pwex.map(my_fun, params)
    
## Gee's Lambada

Deployment with dependencies (requirements.txt references Lambada framework)

    def my_fun(b):
        ...
        
    tune = Lambada(role=..., region=..., memoery=128)
    
    @tune.dancer
    def my_fun_lambda(e, c):
        my_fun(e['stddev'])
        

## FaaSification

Process of automated decomposition of software into a set of function-level services: code analysis + transformation + deployment + on-demand activation

### Transformation rules

Entry points: no transformation of main function

Function definitions:
- Adapt to conventions: parameters, return value
- Scan recursively for function calls
- Export as function unit including dependencies

Function calls:
- if internal, rewire
- if I/O, replace
- otherwise, leave as is

Monads:
- Functional programming with side effects / side channels


Open research problems
- clustering
- code inspection (deep FaaSification)

## Deployment

* Implicit: web IDE
* Explicit: provider tools

Provider specific
- Authorisation (e.g. function calling another function)
- Packaging


### Abstraction

Abstraction approach: Serverless FW

    serverless create --template spotinst-nodejs --path P
    
    serverless config credentials --provider sptinst --token T --account A
    
    serverless deploy function -f hello
    serverless invoke -f hello
    
serverless.yml:

    service: hello
    provider:
        name: spotinst
        spotinst:
            environment:...
    plugins:
        - serverless-spotinst-functions
        
    functions:
        hello:
            handler: index.main
            runtime: nodejs8
            memory: 128
            timeout: 20
            access: public



### Prototyping

Prototyping approach: Localstack

Emulation of commercial FaaS offerings and includion into CI

    localstack start [--docker]
    
    alias aws="aws --endpoint-url=http://localhost:4574"
    aws lambda list-functions ...


## Debugging

Problems
- Crash
- Missing dependency
- Slowness
. Undeterministic behaviour

Tools
- Insight
- Visualisation
- Debugging/Profiling tools (wskdebug for OpenWhisk)
- Feedback 
- Autotuning

K-rules: Analyse -> Plan -> Execute -> Monitor

Analyse
- Stacktraces
- Profiles
- Anomalies over time

Plan
- Recombination of function clusters
- Memory reconfiguration

Execute
- Hot deployment
- Canary testing
- Step/Breakpoint execution

Monitor
- Invocation counts
- Invocation time
- Throttling
- Faults
- Variables

Feeds into:
- FaaS Debugging, Profiling and Autotuning Catalogue
- Management functions











