---
path: "/ads/graphen"
title: "ADS Graphen"
date: "2018-02-14"
draft: false
author: Simon Anliker
tags:
  - ads
---



# Graphen

* Shortest Path: Kürzeste Verbindung von A nach B
* Topological Sort: Reihenfolge von Tätigkeiten aus einem Netzplan erstellen
* Critical Path: Minial benötigte Zeit bei optimaler Reihenfolge
* Maximal Flow: Maximaler Durchsatz von A nach B
* Traveling Salesman: Kürzester Weg um alle Punkte anzufahren

## Graph

Definition: Ein Graph $$G=(V,E)$$ besteht aus einer endlichen Menge von Knoten $$V$$ und einer Menge von Kanten $$E \subseteq V \times V$$

* Knoten: Objekte mit Namen und anderen Attributen
* Kanten: Verbindungen zwischen zwei Knoten u.U. mit Attributen  

* Adjacent: benachbart, falls eine Kante gibt
* Verbundener/Kompletter Graph: Jeder Knoten ist mit jedem verbunden
* Verbundener Teilgraph: Graph aus untereinander nicht verbundenen Teilgraphen
* Ungerichteter Graph: Kanten ohne Pfeile -> in beide Richtungen
* Gerichteter Graph: Kanten mit Pfeile -> bestimmt Richtung
* Gewichteter Graph: Gewichtungsattribut auf Kante
* Dichter Graph (dense): Nur wenige Kanten fehlen
* Dünner Graph (sparse): Nur wenige Kanten sind vorhanden
* Einfacher Pfad: Sequenz von benachbarten, einmaligen Knoten
* Pfadlänge: Anzahl Kanten des Pfads
* Zyklus: Anfangs- und Endknoten beim Pfad gleich
* Zyklischer Graph: Graph mit zyklischen Pfaden
* Azyklischer Graph: Graph ohne zyklische Pfade

### Adjazenzliste

* Knoten mit Liste der ausgehenden Kanten
* Kante mit Zielknoten und Attributen

* Vorteil: Platzbedarf proportional zu #Knoten + #Kanten
* Nachteil: Effizienz der Kanzensuche abhängig von mittlerer Anzahl ausgehender Kanten pro Knoten

### Adjazenzmatrix

* Matrix mit boolean Werten -> true heisst eine Verbindung existiert
* Gewicht anstatt boolean falls Gewichtet (0 für keine Verbindung)
* $$ N \times N $$ (N = #Knoten)
* Dreiecksmatrix falls ungerichtet (die hälfte reicht)

* Vorteil: Berechnung der Adjazenz sehr effizient, einfach zu implementieren, speichereffizient falls dichter Graph
* Nachteil: Speicheroverhead, teure Initialisierung,  wächst quadratisch


### Baum

Zusammenhängender, gerichteter, zyklenfreier Graph mit genau einer Wurzel wobei zu jedem Knoten genau ein Pfad existiert.


## Traversierung

### Tiefensuche

* Vorwärts/Tiefer gehen bis alle Knoten besucht sind, dann Backtracking und weiter
* Depth-first entspricht der Preorder Traversierung (Stack)


### Breitensuche

* Zuerst alle benachbarten Knoten, danach nächstes Level
* Breadth-first entspricht der Levelorder Traversierung (Queue)


## Algorithmen

### Kürzester Pfad

* Vom Startpunkt aus Knoten mit Distanz markieren und Knoten von dem er erreicht wurde
* Vom Endpunkt aus Rückwärts den kürzesten Pfad bilden
* Gewichtete Kanten: Gewichtung als Distanz, kürzeste Distanz zu einem Knoten eintragen

#### Dijkstra

* 3 Gruppen: besuchte, benachbart zu besuchtem und unbesuchte Knoten
* Suche unter benachbarten Knoten denjenigen Pfad mit dem kleinsten Gewicht
* Besuche diesen und bestimme die benachbarten Knoten
* Breadth-first mit PriorityQueue

### Spanning Tree

* Baum eines Graphen der alle Knoten enthält
* Minimaler Spannbaum: Spannbaum eines gewichteten Graphen -> Summe aller Kanten minimal
* Prim-Jarnik Algorithmus mit PriorityQueue für minimalen Spannbaum (MST)
* Anwendung: Routing, Steckennetz für Städte mit minimaler Länge

### Topologisches Sortieren

* Sortierung eines gerichteten Graphen
* Knoten eines gerichteten, unzyklischen Graphs in korrekter Reigenfolge auflisten
* Anwendung: Abhängigkeiten in einem Programm, Compilation-Reihenfolge

### Maximaler Fluss

* Kanten geben maximalen Fluss zwischen Knoten an
* 3 Graphen, Original, Vorläufiger-, Rest Fluss
* Pfad in vorläufiger Fluss eintragen und im Rest löschen
* Fortfahren bis maximaler Fluss gefunden

## Traveling Salesman

* Finde die kürzeste Reiseroute wobei jede Stadt nur einmal besucht wird
* Kann nur durch probieren aller möglichen Wege gefunden werden



