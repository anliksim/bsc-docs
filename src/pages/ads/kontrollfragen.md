---
path: "/ads/fragen"
title: "ADS Kontrollfragen"
date: "2018-02-14"
draft: false
author: Simon Anliker
tags:
  - ads
  - de
---

## Einführung in Algorithmen

Welche der Operationen kommen in einem Stack vor?

* push, pop (LIFO), isEmpty 
* peek, removeAll, isFull (optional)

Ein Abstrakter Datentyp (ADT) umfasst?

* die Schnittstelle der Operationen
* die Implementation der Operationen

Was kann man mit einer Queue machen?

* ein neues Element hinzufügen (enqueue)
* das zuerst eingefügte Element ausgeben lassen (dequeue - FIFO)
* testen ob die Queue leer ist (isEmpty)
* peek, removeAll, isFull (optional)

Welche Aufwände liegen zwischen konstantem und quadratischen Aufwand?

* $$ O(1) < O(log_xn) < O(n) < O(n log_xn) < O(n^2) < O(n^3) $$

Für welche der folgende Paare gilt g(n) = O(f(n))?

* $$ 1000n \neq O(n^{1/2}) $$
* $$ log_2n = O(log_{10}n) $$
* $$ n^{1/2} \neq O(n^{1/3}) $$
* $$ nlogn \neq O(n^2) $$
* $$ 7n^2 = O(176n^2-36n) $$
* $$ n(logn)^2 \neq O(nlogn+n^{1/2}) $$


## Listen

In einer einfach verketteten Liste zeigt der -head- auf null, d.h.?

* die Liste ist leer

Welche Operation in einer einfach verketteten List ist teuer?

* Letztes Element löschen

Wie gross ist die Laufzeit für suchen in einer unsortierten, doppelt verketteten Liste?

* linear O(n)

In welcher Listenimplementations geht das einfügen bei 100k Einträgen schneller?

* LinkedList, falls die ArrayList nicht auf mehr als 100k Einträge initialisiert wurde


## Generics

Was machen Generics?

* Generics ermöglichen die Festlegung der Typen zur Übersetzungszeit (compile time).

Was sind die Vorteile der Verwendung generischer Datenstrukturen?

* Es müssen weniger Laufzeuttests durchgeführt werden (wegen type safety/compile time checks)
* Datenstrukturen können für verschiedene Typen programmiert/verwendet werden
 
Wie deklaraiert man Collections von beliebigem Typ?

* Collection<?>

Wie erlaubt man Klassen, welche das Comparable Interface implementieren?

* <? extends Comparable>

Was bedeutet Type Erasure?

* Zur Laufzeit sind keine Typeninformationen verfügbar


## Rekursion

Warum braucht ein rekursives Programm mehr Speicher als ein iteratives?

* Bei jedem Methodenaufruf wird die Rücksprungadresse gespeichert

Kann man jeden rekursiven Algorithmus in einen iterativen umwandeln?

* Ja (z.B. tail recursion - compiler optimiert automatisch)

Was ist die Idee für den rekursiven Algorithmus für die Türme von Hanoi?

* Aufrufen von Algorithmus für die obersten n-1 Scheiden - verschiebt Teilturm, dann die unterste Scheibe auf den anderen Stapel verschieben

Was passiert beim Aufruf dieser Method mit n=5?

```java
  public int fac(int n) {
    return n * fac(n-1);
  }

```

* Fehlermeldung (StackOverflowError) - es fehlt die Abbruchbedingung

Was ist ein Indiz dafür, dass ein Programm rekursiv ist?

* Das Programm ruft sich selber auf


## Bäume

Welche Aussagen über Bäume sind korrekt?

* Ein Baum hat immer genau eine Wurzel
* In einem binären Baum hat jeder Knoten höchstens zwei Kinder

Welche Aussagen zur Traversierung sind korrekt?

* Beim Inorder kommt der aktuelle Knoten vor den Knoten des rechten Teilbaums
* Pre- und Postorder geben immer gleichviele Knoten aus
* Beim Preorder kommt der aktuelle Knoten vor den Knoten des linken Teilbaums

Welche Art von Baum Traversierung ergibt 2,7,1,6,11,3,5,4,9?
```
.........3......
......./..\.....
......7....5....
..../.\.....\...
..2...6......9..
...../.\..../...
....1..11..4....
```

* Inorder

Welche Aussage über binäre Suchbäume sind korrekt?

* Grösstes Element hat keinen rechten Nachfolger

Was muss man tun um den binären Suchbaum in umgekehrter Reihenfolge auszugeben?

* Inorder, aber zuerst den rechten Teilbaum verarbeiten


## Balancierte Bäume

Welche Aussagen über AVL sind korrekt?

* Die Höhe des rechten im Vergleich zum linken Teilbaums ist +/- 1

Welche Datenstruktur sind für die Speicherung der Daten auf einer Festplatte geeignet?

* B-Baum

Mit welcher Operation werden Bäume balanciert?

* Rotationen

Wieviel Schlüsselwerte können in einer Seite eines B-Baums mit Ordnung 7 max gespeichert werden?

* 7

Wie viele Werte können in einem Baum mit Höhe/Tiefe 7 max gepseichert werden?

* $$ 2^7-1 = 127 $$


## Graphen 1

Welche Aussagen über Graphen stimmen?

* Einen ungerichteten kann man auch als gerichteten Graphen darstellen
* Ein ungerichteter Graph kann gewichtet sein
* Die Kanten in einem gerichteten Graph habe eine Richtung

Für welche Art von Graph stimmt "Es gibt keinen geschlossenen Rundweg entlang von gerichteten Kanten"?

* gerichtete, azyklische Graphen

Welches ist die speichereffizientiste Implementation eines Graphen mit wenig Kanten im Vergleich zu Knoten?

* Adjazenzliste

Wie funktioniert Tiefensuche auch bei Graphen mit Zyklen?

* Markieren der besuchten Knoten

Welche Aussagen sind korrekt?

* Bäume sind auch Graphen
* Zyklische, doppelt verkettete Listen sind ungerichtete Graphen
* Ungerichtete Kanten können durch zwei gerichtete ersetzt werden


## Graphen 2

Was findet der Dijkstra Algorithmus?

* immer den kürzesten Weg

Wie bestimmt man in einem Projekt den kritischen Pfad?

* Dijkstra Algorithmus anwenden

Wo wird der maximale Fluss verwendet?

* Verkehrsleittechnik
* Stromleittechnik

Auf welchen Datenstrukturen basiert der Heap in Java?

* eine Liste
* ein Graph
* ein Array von Bytes

Was macht man beim topologischen Sortieren?

* Eingehende Kanten zählen


## Hashtabellen

Für welche Art von Daten kann man einfach Hash-Werte berechnen?

* Strings
* Arrays von Strings
* Ganze Zahlen
* Fliesskommazahlen

Was sind Nachteile von linearem gegenüber quadratischem Sondieren?

* Es bilden sich vermehrt Cluster

Was muss man tun wenn die Hash-Tabelle voll ist?

* Vergrössern und alle Werte re-hashen

Ab welchem Belegungsgrad der Schlüsseltabelle treten beim Hashing vermehrt Kollisionen auf?

* 0.8

Welches sind die Vorteile von Hashtabellen gegenüber Bäumen

* O(1) vs. O(logn)
* Sehr einfache Implementierung


## Sortieren 1

Wie sieht die Worst-Case Eingabe aus, wenn wir mit Bubble Sort absteigend sortieren wollen?

* Die Daten sind aufsteigend sortiert

Welcher Algorithmus beschreibt "Im Stapel Karten nach der kleinsten Zahl suchen und diese auf einen neuen Stapel legen"?

* Selection Sort

Wie verhält sich Insertion Sort bei sortierter Eingabe?

* Best Case, Ordnung O(n)

Von was hängt die Laufzeit der Sortieralgorithmen ab?

* Ordnung des Algorithmus
* Anzahl Werte
* Datentyp des Sortierschlüssels
* sonstige Auslastung des Rechners

Bei Messungen ist Bubble Sort der schnellste, was könnte der Grund sein?

* Testdaten leigen in sortierter Reihenfolge vor


## Sortieren 2

Welche ussagen treffen auf Quicksort's Divide-and-Conquer zu?

* Teilt das Problem auf und löst Teilprobleme

Welches sind praktikable (evtl nicht optimale) Strategien bei der Pivot Wahl bei Quicksort?

* Wert in der Mitte des Arrays nehmen
* Wert beliebig auswählen

Was bedeutet Stabilität im Zusammenhang mit Sortieralgorithmen?

* Einmal sortierte Folgen behalten ihre Reihenfolge

Wie fügt man am schnellsten 1mil sortierten Werten 100 hinzu, sortiert?

* Insertion Sort
* 100 Werte sortieren und Mergen

Warum wird Quicksort trotz Worst Case $$O(n^2)$$ verwendet?

* Tritt sehr selten auf







