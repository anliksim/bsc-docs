---
path: "/ads/sortieren"
title: "ADS Sortieren"
date: "2018-02-14"
draft: false
author: Simon Anliker
tags:
  - ads
  - de
---

* Internes Sortieren: Im Arbeitsspeicher
* Externes Sortieren: Daten werden aufgeteilt und nur Teile intern sortiert, anschliessend gemerged
* Sortierschlüssel: Sortierkriterium, eindeutig wenn er zu einer eindeutigen Sortierung führt
* Ordnung: Bestimmt Veränderung des Aufwands bei weniger/mehr Daten
* Laufzeit: Bestimmt durch Ordnung, Hardware, Sprache, Compiler, Auslastung, etc.
* Teile und Herrsche: Problem zerlegen, Teilprobleme lösen, Lösungen zusammenführen

### Toplist

* Beste Ordnung bei Vergleichen: Worst Case $$O(nlog(n))$$
* Bester für kurze, sortierte Daten: Insertion Sort
* Bester für lange, unsortierte Daten: Quick Sort
* Bester für sehr viele Daten: Externes Verfahren (interns je nach Sortierung)
* Bester für viele Daten, ungeordnet, sehr oft: Distribution Sort angepasst 
* Stabil: Bubble-, Insertion-, Merge Sort
* Instabil: Selection-, Quick Sort


## Bubble Sort

* Wiederholtes Vertauschen von Nachbarfeldern
* BubbleUp des jeweils grössten Elementes im unsortierten Teil des Array
* Best Case: Einmal durch sortierte Daten bubbeln => $$O(n)$$
* Avg/Worst Case: $$ n*(n-1) $$ (Worst) $$ => O(n^2)$$
* Bubble vs. Insertion: Bei grossen Datensätzen bei welchen einzelne Elemente untereinander getauscht wurden wäre Bubble Sort effizienter

## Selection Sort

* Kleinstes Element nehmen und auf neuen Stapel legen
* Anwendung: viel Datensätze, unsortiert (Quick Sort?)
* Vorteil: Weniger Swap Operationen als Bubble/Insertion Sort
* Nachteil: Sortierte Daten bringen nichts
* Instabil: 'Swap' kann die relative Anordnung gleicher Werte verändern

## Insertion Sort

* Nächstes Element auf neuem Stapel richtig einordnen
* Anwendung: wenig Datensätze, gut vorsortiert
* Worst Case: Bei umgekehrt sortierten Datensätzen $$O(n^2)$$

## Quick Sort

* Teile und Herrsche -> [P1][W][P2]
* Pivot: linkes-, mittleres-, rechtes Element als W
  * Nehme eines der drei Elemente
  * Nehme das wertmässig mittlere
  * Nehme das arithmetisch mittlere
* Teile in zwei Partionen ohne W -> Bereich muss $$log(n)$$ mal geteilt werden
* Partitioning: l,r auf W zu und tausche falsche Elemente
  * tausche $$A[l] >= W$$ und $$A[r] <= W$$ bis $$l>=r$$
  * Aufwand proportional zu $$n$$ 
* Anwendung: viel Datensätze, unsortiert
* Best/Avg Case: $$O(nlog(n))$$
* Worst Case: Baum degeneriert zu Liste, nur in konstruierten Fällen $$O(n^2)$$
* Optimierung: Initialer Aufwand ist gross, für kleine Datenmengen ein einfacheres Verfahren wählen
  * Datenbestand auf Sortiertheit untersuchen mit Insertion Sort

## Distribution Sort

* Kommt ohne Vergleiche aus
* Elemente in Fächer verteilen, zusammentragen $$O(n)$$

## Externes Sortieren

* Sortieren: Internes Sortieren
* Verteilen: Schreibe Folgen von sortierten Abschnitten in mindestens zwei Ausgabedateien
* Mischen: Lese von beiden Dateien das erste Element und schreibe das kleiner und lese das nächste von der gleichen Datei
  * Anzahl Mischphasen $$\lfloor log_{m}n \rfloor$$
  * Aufwand von Mischen $$n \lfloor log_{m}n \rfloor$$

## Stabilität

* Behandlung von Elementen mit gleichem Schlüssel
* Stabil, wenn zwei Elemente mit gleichem Schlüssel auch nach dem Sortieren noch die gleiche Reihenfolge untereinander haben.
* Beispiel: Sortieren nach Name -> Sortiere nach Klasse -> Anordnung der Namen innerhalb Klassen immernoch gleich
