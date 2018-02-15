# Beschreibung der Probleme
(nach Prof. M. Chimani)

* [Knotenfärbung](#knotenfärbung)
* [Kantenfärbung](#kantenfärbung)
* [Rucksackproblem](#rucksackproblem)
* [Clique](#clique)
* [Multiprocessor Scheduling](#multiprocessor-scheduling)
* [Bin Packing](#bin-packing)
* [Partition](#partition)
* [Traveling Salesman Problem](#traveling-salesman-problem)
* [Euler-Tour](#euler-tour)
* [Hamilton-Kreis](#hamilton-kreis)
* [VertexCover](#vertexcover)
* [SetCover](#setcover)

---

## Knotenfärbung

**Gegeben** ist ein Graph G = (V, E) und eine Menge Farben &Phi;.
* &Phi;: V &rarr; {1, 2, ..., c} mit &phi;(v) &ne; &phi;(w) &forall; (v, w) &isin; E

**Gesucht** ist eine möglichst kleine Menge c an verschiedenen Farben, um jeden Knoten in V einzufärben.
Benachbarte Knoten dürfen nicht die gleiche Farbe haben.

Dabei ist c = &chi;(G) = _Chromatische Zahl_ von G

[up](#beschreibung-der-probleme)

---

## Kantenfärbung

**Gegeben** ist ein Graph G = (V, E) und eine Menge Farben &Phi;'.
* &Phi;': E &rarr; {1, 2, ..., c'} mit &phi;'(e) &ne; &phi;'(f) &forall; e, f &isin; E adjazent

**Gesucht** ist eine möglichst kleine Menge c' an verschiedenen Farben, um jede Kante in E einzufärben.
Adjazente Kanten dürfen nicht die gleiche Farbe haben.

Dabei ist c' = &chi;'(G) = _Chromatischer Index_ von G

[up](#beschreibung-der-probleme)

---

## Rucksackproblem

**Gegeben** ist eine Menge I mit n Gegenständen (_items_) I := {1, 2, ..., n}.
Jedes _item_ n hat ein Gewicht w&#x2099; und einen Profit p&#x2099;.
Weiterhin gibt es eine Rucksackgröße B.

**Gesucht** ist eine _gültige_ Teilmenge I'&sube;I der _items_ mit w(I') := &Sigma; w&#x2099; &le; B, so dass gleichzeitig p(I') := &Sigma; p&#x2099; maximiert wird.

[up](#beschreibung-der-probleme)

---

## Clique

**Gegeben** ist ein Graph G=(V, E)

**Gesucht** ist die größte Clique C (= vollständiger Teilgraph) in G.

[up](#beschreibung-der-probleme)

---

## Multiprocessor Scheduling

**Gegeben** ist eine Menge J mit n Jobs J := {1, 2, ..., n}.
Jeder Job n hat eine Laufzeit p&#x2099; .
Weiterhin gibt es m identische Maschinen (CPUs) mit m &gt; 1.

**Gesucht** ist die optimale Aufteilung der Jobs auf die Maschinen, sodass die Jobs schnellstmöglich erledigt sind. <br>
Das bedeutet: Partition der Jobs J in m disjunkte Teilmengen `J1, J2, ..., Jm`, sodass max(Laufzeit Maschine) minimiert wird.

[up](#beschreibung-der-probleme)

---

## Bin Packing

**Gegeben** ist eine Menge I mit n Gegenständen (_items_) I := {1, 2, ..., n}.
Jedes _item_ n hat ein Gewicht w&#x2099; <br>
Bin-Kapazität B.

**Gesucht** ist eine _gültige_ Aufteilung der _items_ in die Bins, sodass keiner übervoll ist. Minimieren über die Anzahl der Bins.

[up](#beschreibung-der-probleme)

---

## Partition

**Gegeben** ist eine Menge natürlicher Zahlen I := {a1, a2, ..., an}. Aufsummiert ergeben die Zahlen `s`.

**Gesucht** ist eine Aufteilung der Zahlen in zwei Gruppen A1 und A2, sodass ihre Summe je `s`/2 ist.

[up](#beschreibung-der-probleme)

---

## Traveling Salesman Problem
(TSP)

**Gegeben** sind `n` Orte und Distanzen `d` zwischen ihnen. Repräsentiert als vollständiger Graph G=(V, E) mit V als Orte und Kantenkosten als Distanzen.

**Gesucht** ist die kürzeste Rundreise, die alle Orte _genau_ 1x besucht.

[up](#beschreibung-der-probleme)

---

## Euler-Tour

**Gegeben**: Multigraph G (mit Mehrfachkanten)

**Gesucht**: Euler-Tour in diesem Multigraphen G, der jede Kante genau einmal besucht.

[up](#approximationsalgorithmen)

---

## Hamilton-Kreis

**Gegeben** ist ein Graph G.

**Gesucht** ist ein Kreis in G, der jeden Knoten genau einmal besucht.
