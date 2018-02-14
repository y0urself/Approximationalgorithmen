# Approximationsalgorithmen
(nach Prof. M. Chimani)

* [Classics](#classics)
  * [Gütegarantie](#gütegarantie)
  * [Graphenfärbung](#graphenfärbung)
  * [Rucksackproblem](#rucksackproblem)
  * [Clique](#clique)
  * [Scheduling](#scheduling)


## Classics

* _alle_ Zahlen sind ganzzahlig bzw. rational !
* `Opt(J)` ist polynomiell beschränkt in Instanzgröße `J` und der Größe der in  `J` vorkommenden Zahlen

### Kombinatorische Optimierung (KO)

* ein kombinatorisches Optimierungsproblem wird auf einer Instanz `J` (aus Menge der möglichen Instanzen) gestellt
  * endlichen Menge von zulässigen (feasable) Lösungen `L(J)`
  * dabei gibt es eine Kostenfunktion `c: L(J) &rarr; R`
  * min/max-Problem
  * min: `Opt(J)` = Lösung `L'` mit minimalen Kosten `c(L')`

[up](#approximationsalgorithmen)

---

### Approximationsalgorithmus (APX)

* liefert in polynomieller Zeit eine _zulässige_ Lösung für eine beliebige Instanz `J` eines _NP-vollständigen_ KO-Problems **mit** einem Lösungswert `A(J)`einer bestimmten [Gütegarantie](#gütegarantie).

## Gütegarantie

### absolute Gütegarantie

* eine absolute Gütegarantie `k`mit `k > 0` bedeutet, dass der [APX](#approximationsalgorithmus-apx) für jede Probleminstanz `J` eine Gütegarantie garantiert:
  * **|Opt(J) - A(J)| &le; k**
  
  * nur sehr wenige Probleme haben [APX](#approximationsalgorithmus-apx) mit absoluter Gütegarantie.
    * hierfür lässt sich _meist_(!) der optimale Lösungswerte-Bereich `Opt` _instanzunabhängig_ einschränken und der APX berechnet eine triviale Lösung am pessimistischen Rand dieses Bereiches.
    
  * absolute Gütegarantien sind für **fast alle** halbwegs interessanten Probleme unmöglich!
#### Scaling:
* mögliche Art um zu beweisen, dass keine absolute Gütegarantie existiert.

_Beispiele_: [Rucksack](#rucksackproblem), TSP

[up](#approximationsalgorithmen)

---

### relative Gütegarantie

&alpha;-Approximation

#### Minimierung

**A(J) &le; &alpha;&middot; Opt(J)** für alle Probleminstanzen `J` mit (&alpha; &gt; 1)

* relative Abweichung **&epsilon;**:
  * (A(J) – Opt(J)) / Opt(J) &le; &epsilon; <br>
  A(J) &le; (1 - &epsilon;) &middot; Opt(J)

#### Maximierung

**A(J) &ge; &alpha;&middot; Opt(J)** für alle Probleminstanzen `J` mit (0 &lt; &alpha; &lt; 1)

* relative Abweichung **&epsilon;**:
* (Opt(J) - A(J)) / Opt(J) &le; &epsilon; <br>
A(J) &le; (1 + &epsilon;) &middot; Opt(J)

---

#### scharfe Gütegarantie

Gütegarantie ist _scharf_, wenn:
* eine Instanz J für die A(J) die Gütegarantie mit Gleichheit erfüllt
* eine Folge von Instanzen, dessen Grenzwert die Gütegarantie mit Gleichheit erfüllt

Beweis durch Beispiel genügt!

---

#### asymptotische Gütegarantie

Es existiert eine Konstante `d`, sodass

**A(J) &le; &alpha;&middot; Opt(J) + d** für alle Probleminstanzen `J`

außerdem gibt es noch:

Es existiert eine Konstante `d'`, sodass

**A(J) &le; &alpha;&middot; Opt(J)** für alle Probleminstanzen `J` mit Opt(J) &gt; `d'`

[up](#approximationsalgorithmen)

---

### Approximationsschema

#### polynomielles Approximationsschema (PTAS)

#### effizientes polynomielles Approximationsschema (EPTAS)

#### vollständig polynomielles Approximationsschema (FPTAS)

[up](#approximationsalgorithmen)

---

## Graphenfärbung

### Knotenfärbung

**Gegeben** ist ein Graph G = (V, E) und eine Menge Farben &Phi;.
* &Phi;: V &rarr; {1, 2, ..., c} mit &phi;(v) &ne; &phi;(w) &forall; (v, w) &isin; E

**Gesucht** ist eine möglichst kleine Menge c an verschiedenen Farben, um jeden Knoten in V einzufärben.
Benachbarte Knoten dürfen nicht die gleiche Farbe haben.

Dabei ist c = &chi;(G) = _Chromatische Zahl_ von G

#### bipartiter Graph
  * kann der Graph in zwei Partitionen `V1` und `V2` zerlegt werden, sodass `V1` nur mehr Nachbarn aus `V2` hat, so kann der Graph offensichtlich in 2 Farben eingefärbt werden. `V1` in c1 und `V2` in c2.
  
#### planarer Graph
  * In der Ebene kann der Graph G ohne gekreuzte Kanten gezeichnet werden.
    * **Theorem**: Festzustellen, ob planarer Graph in 3 Farben knoten-färbbar ist, ist _NP-vollständig_.
    * **Theorem**: Jeder Graph G lässt sich mit _maximal_ 5 Farben knoten-färben. (Es reichen sogar 4 ...)

#### [Approximationsalgorithmus](#approximationsalgorithmus-apx)
```
 Teste ob Graph bipartit

   if true:     2 Farben
   else:        berechne 5-Färbung (wir wissen ja, wir benötigen min. 3 Farben)
```

* [absolute Gütegarantie](#absolute-gütegarantie): k = 2
  * immer maximal 2 schlechter als das Optimum

[up](#approximationsalgorithmen)
  
---

### Kantenfärbung

**Gegeben** ist ein Graph G = (V, E) und eine Menge Farben &Phi;'.
  * &Phi;': E &rarr; {1, 2, ..., c'} mit &phi;'(e) &ne; &phi;'(f) &forall; e, f &isin; E adjazent

**Gesucht** ist eine möglichst kleine Menge c' an verschiedenen Farben, um jede Kante in E einzufärben.
Adjazente Kanten dürfen nicht die gleiche Farbe haben.

Dabei ist c' = &chi;'(G) = _Chromatischer Index_ von G

Offensichtlich:

Wenn &Delta;(G) der maximale Grad vom Graph G ist, so benötigen wir mindestens &Delta;(G) viele Farben zum Kantenfärben

#### Satz von Vizing (1964)
Wir benötigen maximal &Delta;(G)+1 Farben um Graph G kanten-zu-färben.

**Theorem**:

Es ist _NP-vollständig_ festzustellen ob ein Graph G mit &Delta;(G) Farben kanten-einfärbar ist.

#### [Approximationsalgorithmus](#approximationsalgorithmus-apx)
```
Färbe den Graph gemäß Vizing ein.
```
* [absolute Gütegarantie](#absolute-gütegarantie): k = 1
  * maximal 1 Farbe zuviel genutzt

[up](#approximationsalgorithmen)

---

## Rucksackproblem

**Gegeben** ist eine Menge I mit n Gegenständen (_items_) I := {1, 2, ..., n}.
Jedes _item_ n hat ein Gewicht w&#x2099; und einen Profit p&#x2099;.
Weiterhin gibt es eine Rucksackgröße B.

**Gesucht** ist eine _gültige_ Teilmenge I'&sube;I der _items_ mit w(I') := &Sigma; w&#x2099; &le; B, so dass gleichzeitig p(I') := &Sigma; p&#x2099; maximiert wird.

---

**Theorem**:

Falls P &ne; NP, so existiert kein [APX](#approximationsalgorithmus-apx) mit [absoluter Gütegarantie](#absolute-gütegarantie) für das Rucksackproblem.

**Beweis durch [_Scaling_](#scaling)**:

Es wird angenommen, dass ein Algoritmus `A` existiert, sowie eine natürliche Zahl k, sodass `A` die [absolute Gütegarantie](#absolute-gütegarantie) k hat.
  * Beliebige Instanz **J = &lt;n, w, p, B&gt;**
  * erstelle aus J die neue Instanz **J' = &lt;n, w, p', B&gt;** wobei p' = (k+1) &middot; p für alle p_i

&rarr; jede Lösung für J' ist gültig für J und umgekehrt. Für jede Lösung: Zielfunktion für J' ist genau (k+1)-mal größer als für J.

Löse J' mit `A` (L = Lösung des Algorithmus `A`, p(L) Wert der Lösung in J, p'(L) Wert der Lösung in J'): <br>
Opt(J') - A(J') &le; k <br>
Opt(J') - p'(L) &le; k <br>
(k+1)&middot;Opt(J) - (k+1)&middot;+(L) &le; k <br>
Opt(J') - p(L) &le; k / (k+1) = 0 <br>

Wir könnten J also in polynomieller Zeit _optimal_ lösen. Dass ist ein **Widerspruch**!

[up](#approximationsalgorithmen)

---

## Clique

**Gegeben** ist ein Graph G=(V, E)

**Gesucht** ist die größte Clique C (= vollständiger Teilgraph) in G.

---

**Theorem**:

Falls P &ne; NP, so existiert kein [APX](#approximationsalgorithmus-apx) mit [absoluter Gütegarantie](#absolute-gütegarantie) für das Clique-Problem.

**Beweis**(idee):

* erstelle m Kopien von G und verbinde alle Knotenpaare die nicht aus der selben Kopie sind. &rarr; `Gm`
* Opt(G) = s &harr; Opt(`Gm`) = m &middot; s

Selbe Idee wie beim Rucksackproblem über Scaling. <br>
Annahme, dass APX A absolute Gütegarantie k hat. Mit `Gk+1` gibt es k+1 Kopien von G.

Opt(`Gk+1`) - A(`Gk+1`) &le; k <br>
(k+1)&middot;Opt(G) - A(`Gk+1`) &le; k <br>
(k+1)&middot;Opt(G) - (k+1) &middot; |C| &le; k <br>
Opt(J') - |C| &le; k / (k+1) = 0 <br>

[up](#approximationsalgorithmen)

---

## Scheduling

### Multiprocessor Scheduling

**Gegeben** ist eine Menge J mit n Jobs J := {1, 2, ..., n}.
Jeder Job n hat eine Laufzeit p&#x2099; .
Weiterhin gibt es m identische Maschinen (CPUs) mit m &gt; 1.

**Gesucht** ist die optimale Aufteilung der Jobs auf die Maschinen, sodass die Jobs schnellstmöglich erledigt sind. <br>
Das bedeutet: Partition der Jobs J in m disjunkte Teilmengen `J1, J2, ..., Jm`, sodass max(Laufzeit Maschine) minimiert wird.

---

**Theorem**: Bereits für m=2 ist Multiprocessor Scheduling _NP-vollständig_.

---

### List Scheduling
(Greedy-Algorithmus)
```
Jobs nacheinander auf die Maschinen legen - dabei den nächsten Job
immer auf die am Maschine legen, die zuerst Fertig wird.
```

_offline_ Problemstellung - d.h. es sind alle Jobs bekannt bevor der Algorithmus startet. Geht auch _online_ ...

**Theorem**: List Scheduling hat eine [scharfe relative Gütegarantie](#scharfe-gütegarantie) von `2-1/m`

**Beweis** der [Gütegarantie](#relative-gütegarantie):

`Mi`ist Maschine mit längster Laufzeit `A(J)`. Auf ihr läuft der letzte Job j. <br>
`P` ist die Summe der Laufzeiten aller Jobs.
Jede Maschine in M läuft mindestens `A(J)` - `pj`

&rarr; `P` &ge; `m` &middot; (`A(J)` - `pj`) + `pj`

Außerdem gilt für `Opt(J)`:

&rarr; `Opt(J)` &ge; `P`/`m` <br>
&rarr; `Opt(J)` &ge; `pj`

Eingesetzt:

`Opt(J)` &ge; (`A(J)` - `pj`) + `pj`/ `m` <br>
`Opt(J)` &ge; `A(J)` - (1 - (1/ `m`)) &middot; `pj` <br>
`Opt(J)` &ge; `A(J)` - (1 - (1/ `m`)) &middot; `Opt(J)` <br>
(2 - 1/m) `Opt(J)` &ge; `A(J)`

**Beweis** der [Schärfe](#scharfe-gütegarantie):

Instanz J hat m(m-1) Jobs der Länge 1 und einen großen Job der Länge m.

**Optimum** `Opt(J)`:

|M | 1| 2| 3| 4| 5| 6| 7| 8| 9| m|
|---|---|---|---|---|---|---|---|---|---|---|
|M1|m|m|m|m|m|m|m|m|m|m|
|M2|1|1|1|1|1|1|1|1|1|1|
|M3|1|1|1|1|1|1|1|1|1|1|
|M4|1|1|1|1|1|1|1|1|1|1|
|M5|1|1|1|1|1|1|1|1|1|1|

`Opt(J)` = m

**List Scheduling** `A(J)`:

|M | 1| 2| 3| 4| 5| 6| 7| 8| 9| m| | | | | | | | | |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|M1|1|1|1|1|1|1|1|1|1|m|m|m|m|m|m|m|m|m|m|
|M2|1|1|1|1|1|1|1|1|1|| | | | | | | | | | |
|M3|1|1|1|1|1|1|1|1|1|| | | | | | | | | | |
|M4|1|1|1|1|1|1|1|1|1|| | | | | | | | | | |
|M5|1|1|1|1|1|1|1|1|1|| | | | | | | | | | |

`A(J)` = (m-1) + m

---

#### Longest-Processing-Time List-Scheduling
(LPT)

* Jobs zunächst absteigend nach Größe sortieren
* dann List Scheduling

**Theorem**: LPT hat eine [relative Gütegarantie](#relative-gütegarantie) von `(4/3) - (1/3)m`.

[up](#approximationsalgorithmen)

---

## Bin Packing

**Gegeben** ist eine Menge I mit n Gegenständen (_items_) I := {1, 2, ..., n}.
Jedes _item_ n hat ein Gewicht w&#x2099; <br>
Bin-Kapazität B.

**Gesucht** ist eine _gültige_ Aufteilung der _items_ in die Bins, sodass keiner übervoll ist. Minimieren über die Anzahl der Bins.

**Grundannahme**: das Gewicht eines jeden Items ist &le; B.

**Normierung**:<br>
B auf 1 skalieren, sowie alle Gewichte mit dem gleichen Faktor auf 0 &lt; w &lt; 1.

---

**Theorem**: Falls P &ne; NP, existiert kein [APX](#approximationsalgorithmus) mit [relative Gütegarantie](#relative-gütegarantie) &alpha; &lt; 1.5 für Bin-Packing.

### Next-Fit

### First-Fit

### und mehr Fits ...

[up](#approximationsalgorithmen)

---

## Partitionsproblem

**Gegeben** ist eine Menge natürlicher Zahlen I := {a1, a2, ..., an}. Aufsummiert ergeben die Zahlen `s`.

**Gesucht** ist eine Aufteilung der Zahlen in zwei Gruppen A1 und A2, sodass ihre Summe je `s`/2 ist.

[up](#approximationsalgorithmen)

---

## Traveling Salesman Problem
(TSP)

[up](#approximationsalgorithmen)

---

## Metrisches TSP
(&Delta;TSP)

### Euler-Tour

### MST-Heuristik

### Christofides-Heuristik

[up](#approximationsalgorithmen)

---

## VertexCover und SetCover

[up](#approximationsalgorithmen)

---

## Steinerbäume

[up](#approximationsalgorithmen)

---

## euklidisches TSP



Folie 16: G^k+1 -> Clique = 1, dann k+1 > k ... Widerspruch

Folie 20:
m1 ... ..... ... .......
m2 ... ....... ... ....
mi ... ..... ......... ...j.....
m  .... ... ...... ....
 
      ------------A(J)-------------
   

      m1 . . . . . . . . . ......m.......
      .
      .  .
      .  .
      .  .
      
      mm .
      
      -------------2m-1---------------

Folie 25:

Bin-Packing über Partitionsproblem
A ={a1, ..., an}
W ={w1, ..., wn}
Aufteilen 

\exists? A_1 \dot\cup A_2 = A

s = \sum_{i=1}^{n} a_i

\sum_{a\inA_1} a ) \sum_{a\inA_2} a = s/2

B aus Binpacking:
B = s/2


Folie 35:

FFD worst case: \sum = 11m Kartons vs. Opt():>: \sum = 9m Kartons
