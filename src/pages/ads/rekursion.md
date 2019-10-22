---
path: "/ads/rekursion"
title: "ADS Rekursion"
date: "2018-02-14"
draft: false
author: Simon Anliker
tags:
  - ads
  - de
---

* Beispiel Math: Euklid, Fakultät, nat. Zahlen
* Beispiel Natur: Farn, Fraktale, Schneeflocken
* Beispiel IT: rekursive Liste, Baum
* Def: Ein Algorithmus/Datenstruktur heisst rekursiv definiert, wenn er sich selbst als Teil enthält oder mit Hilfe von sich selbst definiert ist.
* Vorteil: Unendlichkeit endlich definieren, leserlich, Problemreduktion
* Nachteil: Overhead bei Methodenaufruf, mehr Speicher
* Direkt: Methode ruft sich selber auf
* Indirekt: 2+ Methoden rufen sich gegenseitig auf
* Endrekursiv: Letze Aktion ist rekursiver Aufruf - lässt sich einfach in iterative Form umwandeln (tail recursion)

## Induktion

* Programmäquivalent der vollständigen Induktion
* Basisfall - Verankerung
* Allgemeinerfall - Induktionsschritt
* Basisfall ist Abbruchbedingung - sonst unendlich -> StackOverflowError

## Komplexität

* Reursionstiefe: max Tiefe - 1 somit 2 => rec(2)-> rec(1)-> rec(0)
