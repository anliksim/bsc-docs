---
path: "/ads/komplex"
title: "ADS Komplexität"
date: "2018-02-14"
draft: false
author: Simon Anliker
tags:
  - ads
  - de
---

 * $$f(n) = O(g(n))$$
 * es existieren konstanten c und $$n_{0}$$, sodass für alle $$n = n_{0}$$ gilt $$f(n) = c*g(n)$$
 * f wächst max. so schnell wie g
 * g ist eine asymptotische obere Schranke für f

## Klassen

| Notation | Aufwand  | Beispiel       |
|----------|----------|----------------|
| $$O(1)$$     | konstant | Lesen in Array |
| $$O(log(n))$$  | logarithmisch | Binary Search in sortiertem Array |
| $$O(n)$$     | linear   | Lineare Suche in unsortiertem Array |
| $$O(nlog(n))$$ | super-linear | Merge-, Heapsort | 
| $$O(n^2)$$   | quadratisch | Selectionsort |
| $$O(n^k)$$ konstantes $$k \neq 1$$ | polynomial | n-fache Schlaufe |
| $$O(2^n)$$ | exponentiell | Rucksackproblem, Brute-Force |
| $$O(!n)$$ | faktoriell | Traveling Salesman (Enumeration) |

| Notation | Erklärung  |
|----------|----------|
| $$O(1)$$     | Überschreitet konstanten Wert nicht |
| $$O(log(n))$$  | Wächst ~ um konstanten Wert wenn sich das Argument verdoppelt |
| $$O(n)$$     | Wächst ~ auf das Doppelte wen sich das Argument verdoppelt  |
| $$O(nlog(n))$$ | ^ | 
| $$O(n^2)$$   | Wächst ~ auf das Vierfache wenn sich das Argument verdoppelt  |
| $$O(n^k)$$ konstantes $$k \neq 1$$ | Wächst ~ auf das $$2^n$$-Fache wenn sich das Argument verdoppelt |
| $$O(2^n)$$ | Wächst ~ auf das Doppelte wenn sich das Argument um 1 erhöht  |
| $$O(!n)$$ | Wächst ~ auf das $$(x+1)$$-Fache wenn sich das Argument um 1 erhöht |




