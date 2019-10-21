---
path: "/ads/hashing"
title: "ADS Hashing"
date: "2018-02-14"
draft: false
author: Simon Anliker
tags:
  - ads
---

# Hashtabellen

* Vorteile: Suchen und Einfügen O(1), degeneriert kaum zur Liste, geringer Aufwand zur Implementierung (vgl. Binäre Bäume)
* Nachteile: Suche nur mit vollständigem Schlüssel, geordnete Ausgabe nicht möglich, kleinstes Element finden nicht einfach
* Anwendung: Optimal wenn Reihenfolge unbedeutend, keine Bereichssuche, Datensatzgrösse bekannt
* Load-Faktor: Hash-Bereich Belegung (zwischen 0 und 1, problematisch ab 0.8)
* Schlüssel: Wird gehasht und für das Abrufen des Datensatzes verwendet, kann Teil des Inhalts sein
* Überlauf: Überlaufsketten, neue Hashtabelle - umkopieren, extendible Hashing (wenn Hauptspeicher zu klein)

## Hashing

* String - ASCII Wert x Position $$256^n$$, in der Praxis Polynome und Modulo Arithmetik, nicht behandelter Überlauf
* HashCode Contract:
  * Immer selber Hash Wert während Lebensdauer
  * Wert muss gleich sein bei equals == true
  * Wert sollte unterschiedlich sein bei equals == false
  * equals == true muss compareTo == 0 geben
  * equals == false muss compareTo != 0 geben

## Kollisionen

* Hängt von Hash-Funktion und Load-Faktor ab

### Seperate Chaining

* Überlauflisten: Hash ist Ankerpunkt für Liste mit Objekten
  * Nachteil: zusätzliche Datenstruktur
  * Vorteil: Funktioniert bei Load-Faktor > 1

### Open Addressing

* Bei Kollision andere Zelle verwenden
* Wrap around: Bei Tabellen ende wieder an den Anfang
* Linear Probing: lineares Sondieren
  * sequentiell freie Zelle suchen
  * Primary Clustering: Wahrscheinlichkeit für suchen steigt mit belegtem Wert neben Hash-Wert
  * ungünstig bei hohem Load Faktor
* Quadratic Probing: quadratisches Sondieren
  * F+1,+4,+9,...,+$$i^2$$
  * bessere Performance da weniger Primary Clustering

## Löschen

* Kein einfaches löschen wegen Ausweichzellen
  * Re-hashing: alle potenziellen Ausweichzellen müssen gelöscht und wieder eingefügt werden
  * Alternativ gelöschte Zellen markieren


## Extendible Hashing

 * Globale Tiefe: 2 bit => Aufteilung auf 4 Buckets => Index %4 ($$2^{Globale Tiefe}$$)
 * Split bei globale == lokale Tiefe: Globale Tiefe 3, Index %8
 * Split bei globale != lokale Tiefe: In diesem Fall zeigen zwei Pointer auf den gleichen Bucket => aufteilen  
