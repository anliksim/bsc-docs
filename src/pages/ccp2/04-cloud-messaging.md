---
path: "/ccp2/cloudmessaging"
title: "CCP2 Cloud Messaging"
date: "2020-06-21"
draft: false
author: Simon Anliker
tags:
  - ccp2
  - cloud
---

<!-- CNA3 -->

# Messaging in the cloud

Message passing - integral for distributed systems

CNA rely on application-level messaging

Synchronous vs asynchronous messaging: to block or not to block

Unidirectional vs bi-directional communication: Sender to receiver vs both ways like TCP

Paradigms:
* Unicast: One sender to one receiver
* Multicast: One sender to many receiver
* Anycast: One sender to any receiver
* Broadcast: One sender to all receivers


## MOM

Message oriented middlewares (MoM)
* language and platform independent dev
* communication features beyond byte-/packet-level

Channels,logical pathways to transport messages
* Point-to-point channel: one consumer to one provider
* Publish-subscribe channel: message delivered to all subscribers
* Datatype channel: all messages in a channel are of same data type
* Dead letter channel: Separate channel to track bad messages
* Guaranteed delivery: built in data store to persist until delivered


Brokered, centralized:
* Orchestrated by central broker
* Simple to comprehend
* Broker can be bottleneck
* e.g. RabbitMQ

Brokerless (distributed)
* p2p messaging
* distributed caching to store messages for reliable delivery
* e.g. DDS, ZeroMQ

## Protocols

### AMQP

Advanced message queuing protocol

Wire level protocol, stream of bytes, frames

Delivery guarantees:
* at most once
* at least once
* exactly once

Layered:
* Messaging
* Transport/Framing (Frame + Extended Header, Frame Body)
* Network transport layer

Frame types:
* Protocol header (ctrl plane)
* Content header (data plane)
* Content body (data plane)
* Method frame (ctrl plane)
* Heartbeat (mgmt plane)

Entities:
* Nodes: Addressable, storage and delivery of messages, communication entities (brokers, queues, senders, receivers)
* Containers: distributed application, nodes live in a container (client app with all consumers, broker with storage and queuing entities)

Connection: full duplex, ordered, between containers

Session: 2 correlated unidirectional channels for bidirectional sequential conversation between two containers, may have multiple links

Link: unidirectional route between 2 nodes, belongs to only one session

### STOMP

Simple/streaming text oriented messaging protocol

Simplicity and interoperability

Text-based, can transmit binary too

Frame:
* Command
* Set of optional headers
* Optional body

Assumes reliable 2-way network protocol such as TCP

Stomp client can be producer (SEND) or consumer (SUBSCRIBE)


### MQTT

MQ telemetry transport

Simple, TCP based, async, pub-sub messaging protocol based on a broker

Minimal network footprint, smallest message is 2 bytes

Binary protocol

Ideal for devices with power and bandwidth constraints

Delivery-QoS ensured by the broker:
* QoS0: at most once
* QoS1: at least once
* QoS2: exactly once

### XMPP

Extensible messaging and presence protocol

Based on XML, dos not support QoS, decentralized


## Technologies

## RabbitMQ

Message broker

Routing types:
* Direct: routing based on match of routing keys (unicast)
* Fanout: sent to all queues bound to the exchange (broadcast)
* Topic: routing based on wildcard match  (multicast)
* Headers: routing based on header attributes (multicast)

Persistence types
* Durable: messages persist between restart
* Exclusive: only one consumer, queue deleted once disconnected
* Auto-delete: queue gets deleted if no consumers

## DDS

Data Distribution Service

OMG standard to use data centric publish-subscribe (DCPS)

Distributed, no central point of failure, no broker

Domains for grouping of applications

Domain participants allow application access to a domain

Data writers and publishers allow publishing of data, publishers are groups of writers with common parameters for QoS, rate, etc

Data readers and subscribers allow access, notifications via listeners, polling, subscribers are groups of readers with common params

Unique topics within a domain, used as connection point between pubs and subs

DDS QoS parameters:
* Deadline
* Destination order
* Durability
* Liveness
* Ownership



