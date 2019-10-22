---
path: "/ads/speicher"
title: "ADS Speicherverwaltung"
date: "2018-02-14"
draft: false
author: Simon Anliker
tags:
  - ads
  - de
---

* Speicheraufbau: Stack, Heap, Statische Daten, Argumente, Programm-Code, Laufzeitsystem

## Daten

* Argumente: beim Aufruf mitgegebene Werte
* Programm Code: ausführbare Instruktionen
* Laufzeitsystem: z.B. Kopie von Datei-Puffern

### Stack

* Rücksprungadressen, lokale Variablen
* Methodenaufruf: Activation Record / Frame
* effizient, rekursive Aufrufe möglich
* Werte nicht abrufbar nach return

### Heap

* Dynamische Daten als Pointer/Referenzen
* rekursive Aufrufe, dynamische Datenstrukturen möglich
* Zuteilung/Freigabe relativ rechenintensiv

### Statische Daten

* Konstanten, static
* Speicher kann einmal beim Start alloziert werden
* Keine dynamischen Datenstrukturen möglich


## Speicherverwaltung

* Listen für belegten und freien Speicher führen
* Fragmentierung -> periodisch defragmentieren
* Zu wenig Speicher angefordert -> Referenz-Variablen automatisch auf Default initialisieren, Bereichsabfragen auf Gültigkeit prüfen
* Speicher nicht freigegeben -> Memory Leak, abhilfe durch Speicherverwaltung 
* Speicher wird zu früh freigegeben -> Dangling Pointer, abhilfe durch Speicherverwaltung

### Referenzzählung

* Verweise auf ein Objekt zählen
* Löschen wenn keine Referenz mehr existiert
* Vorteile: einfach, geringer Aufwand, Speicher wird schnell freigegeben
* Nachteile: Programmiere muss zählen, zusätlziche Operationen bei Pointer-Zuweisung, zyklische Datenstrukturen können nicht freigegeben werden
* Smart-Pointer: Zählen selber Zuweisungen auf und ab

### Garbage Collector

#### Mark-Sweep GC

* Speicher wird bei Bedarf freigegeben, nicht sofort
* Mark: von Wurzelobject ausgehend alle erreichbaren Blöcke markieren
* Sweep: sequentiell durch den Heap alle nicht markierten Blöcke freigeben, Markierungen löschen
* Vorteile: keine zusätzlichen Operationen bei Pointer-Zuweisung, zyklische Strukturen können aufgelöst werden
* Nachteile: Aufwand, Programm muss gestoppt werden, Fragmentierung

#### Copying GC

* Zwei Semi-Spaces mit aktuellen und obsoleten Daten, Daten werden umkopiert
* Cheney's Copying Algorithm (drei Farben): Breitensuche
* Vorteile: keine Fragmentierung, Suche nach freien Blöcken entfällt
* Nachteile: doppelt so viel Speicher nötig, langer Unterbruch, Lokalität des RAM Zugriffs geht verloren

#### Generational GC

* Hotspot Heap: Young Gen (eden, Survivor Space (s0,s1)), Old Gen (Tenured), Perm Gen (Permanent)
* Young Gen:
  * Neue Objeckte werden hier angelegt
  * Minor Collection - Stop the World
* Old Gen: 
  * GC Survivors werden hier abgelegt
  * Major Collection - Stop the World
* Perm Gen: 
  * Metadaten für die JVM welche Klassen und Methoden beschreiben
  * Wird bei Full Collection einbezogen

### Weak References

* Objekte in Sammlungen können nicht freigegeben werden
* Weak References werden nicht traversiert
* Vorteil: Sammlungen können freigegeben werden
* Nachteil: Dangling Pointer

### Finalizer

* Freigabe von Ressources spätestens bei löschen von Objekt
* Zweistufig wegen z.B. Referenz auf sich selbst
  * 1 GC - finalize aufrufen und in Liste speichern
  * 2 GC - Objekte in Finalized-Liste die nicht markiert wurden freigeben





