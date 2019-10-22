---
path: "/ads/datenstrukturen"
title: "ADS Datenstrukturen"
date: "2018-02-14"
draft: false
author: Simon Anliker
tags:
  - ads
  - de
---

## Abstrakt (ADT)

* Schnittstelle durch Interfaces
* Implementierung durch Klasse

## Stack

* LIFO - push,pop,isEmpty
* optional peek,removeAll,isFull
* Anwendung: Methoden (param, var, address), alter Taschenrechner, Klammersetzung

## Listen

* add,add(index),get,remove,size,isEmpty
* LinkedList: Mutation schnell, Zugriff langsam 
* ArrayList: Mutation langsam (je nach Besetzung), Zugriff schnell, Array
* Sortierte Liste - Elemente werden sortiert eingefügt

## Queue

* FIFO - enqueue,dequeue,isEmpty
* optional peek,removeAll,isFull
* Anwendung: Drucker, Warteschlangen, gestaffelter Zugriff

### Ringbuffer

### Priority Queue

* Höhere Prio nach vorne, dann gleiche Prio in Reihenfolge
* Anwendung: Scheduling von Prozessen, Prioritisierung (Taskliste, Rechnungen, etc.)


## Set

* ungeordnete Menge ohne Duplikate
* add, contains, containsAll (subset), addAll (union), retainAll (intersect), removeAll (difference)
* TreeSet -> natürliche Anordnung
