# Approximationsalgorithmen
(nach Prof. M. Chimani)<br>
[alles ohne Gewähr.]

* [Classics](#classics)
  * [Gütegarantie](#gütegarantie)
  * [Approximationsschema](#approximationsschema)
  * [Graphenfärbung](#graphenfärbung)
  * [Rucksackproblem](#rucksackproblem)
  * [Clique](#clique)
  * [Scheduling](#scheduling)
  * [Bin Packing](#bin-packing)
    * [Next-Fit](#next-fit)
    * [First-Fit](#first-fit)
  * [Partition](#partition)
  * [Traveling Salesman Problem](#traveling-salesman-problem)
  * [metrisches TSP](#metrisches-tsp)
  * [Euler-Tour](#euler-tour)
  * [Hamilton-Kreis](#hamilton-kreis)
  * [EdgeCover](#edgecover)
  * [IndependentSet](#independentset)
  * [VertexCover](#vertexcover)
  * [WeightedVertexCover](#weightedvertexcover)
  * [SetCover](#setcover)
  * [WeightedSetCover](#weightedsetcover)


# Classics

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

### randomisierter Approximationsalgorithmus

* treffen gewissen Entscheidungen randomisiert
* aus der Randomisierung resultieren Erwartungswerte für algorithmische Kernzahlen (Laufzeit, Güte, ...)

* Laufzeit sollte weiterhin polynomiell sein (zumindest hier!) - nicht nur erwartet polynomiell!
* aus Güte wird **erwartete Güte**

[up](#approximationsalgorithmen)

---

### Lineare Programme
(LP)

Ein LP hat Variablen, eine *lineare* Zielfunktion und mehrere *lineare* Nebenbedigungen.

Zielfunktion wird maximiert oder minimiert.

Normierte Matrixdarstellung:
```
min    cTx         x = n-dimensionaler Varablenvekor
s.t.    Ax <= b    c = n-dimensionaler Kostenvektor
         x >= 0    A = m×n Constraint-Matrix
                   b = m-dimensionale Right-Hand-Side
                   T meint hier transponiert
```

[up](#approximationsalgorithmen)

---

### Integer Lineare Programme
(ILP)

Ein LP hat Variablen die ganzzahlig sein müssen, eine *lineare* Zielfunktion und mehrere *lineare* Nebenbedigungen.

Normierte Matrixdarstellung:
```
min    cTx         x = n-dimensionaler Varablenvekor
s.t.   Ax  <= b    c = n-dimensionaler Kostenvektor
        x  >= 0    A = m×n Constraint-Matrix
        x aus Z    b = m-dimensionale Right-Hand-Side
                   T meint hier transponiert
                   Z meint hier ganzzahlig
```

### Komplexität
* **ILP**s sind _NP-schwer_ zu lösen
* **LP**s lassen sich in **polynomieller Zeit** (in n, m) lösen
* mit Ellipsoid-Methoden, Innere-Punkte-Methode
* Simplex-Algorithmus im worst exponentiell, aber in Praxis top


Idee: <br> Wieviel verliert man, wenn man statt ILP nur LP rechnet?

[up](#approximationsalgorithmen)

---

### LP-Relaxierung

Streiche Ganzzahligkeitsbedingung <br>
ILP &rarr; LP

Lösung _L_ der LP-Relaxierung = **Duale Schranke** für das ILP
* bei Minimierung: untere Schranke
* bei Maximierung: obere Schranke

* Weniger Einschränkungen &rarr; "bessere" Lösungen (bzgl. Zielfunktion)
* größere Lösungsmenge!
* _L_ im Allgemeinen **infeasible** für das ILP weil höchstwahrscheinlich fraktional
* Falls _L_ "zufälligerweise" ganzzahlig ist, handelt es sich direkt um die optimale Lösung.

**Primale Schranke**: **feasable** Lösung, die nicht optimal ist (ermittelbar durch Heuristiken, [APX](#approximationsalgorithmus), ...)

```
              <-LP-Gap->┌─────────────────────────────────────────────────
                        |   ILP - Lösungen (nur die ganzzahligen :D)
              ┌─────────┴─────────────────────────────────────────────────
              |  LP-Relaxierung
┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─
              ^         ^         ^
              |         |         |
              OptLP     OptILP    zul. primale Lösung

```

[up](#approximationsalgorithmen)

---

### Duales Programm
(DP)

Sei [primales Programm](#lineare-programme) ein [Minimierungsproblem](#minimierung).
Das dazugehörige duale Programm ist ein [Maximierungsproblem](#maximierung), welches die selbe duale Schranke als Optimum hat, wie das primale Programm.

Aus jedem LP lässt sich ein DP erstellen.

**Beispiel:**
```
Primales Programm
min   7 x1 +   x2 + 5 x3           (ZF)
s.t.    x1 –   x2 + 3 x3 >=  10     (1)
      5 x1 + 2 x2 –   x3 >=   6     (2)
        x1,    x2,    x3 >=   0
```
Untere Schranke für `(ZF)`?  Koeffizientenvergleich!<br>
Beobachte: alle **Variablen &ge; 0**

Koeffizientenvergleich von `(ZF)` mit `(1)`: <br>
7 &ge; 1 +1&ge;–1, 5&ge;3 &rarr; 7x1 + x2 + 5x3 &ge; 10 &rarr; 10 ist untere Schranke

Koeffizientenvergleich von `(ZF)` mit `(1)`+`(2)`: <br>
```
6 x1 +   x2 + 2 x3 >= 16     (1)+(2)
```
7 &ge; 6, 1 &ge;1, 5 &ge;2  &rarr; 16 ist untere Schranke

Koeffizientenvergleich von (ZF) mit 2·(1)+(2):
```
7 x1 + 0 x2 + 2 x3 >= 26     2*(1)+(2)
```
7 &ge;7, 1&ge;0, 5&ge;5 &rarr; 26 ist untere Schranke

**Optimaler Lösungsvektor**: x=(7/4),0, (11/4))<br>
Zielfunktionswert: 26

```
Duales Programm
max   10 y1 + 6 y2           (ZF)
s.t.     y1 + 5 y2 <=  7     (1)
       - y1 + 2 y2 <=  1     (2)
       3 y1 - 1 y2 <=  5     (3)
         y1,    y2,>=  0
```

**Optimaler Lösungsvektor**: y=(2, 1)<br>
Zielfunktionswert: 26

[up](#approximationsalgorithmen)

---

Normiertes Duales Programm:
```
max    bTy          y = m-dimensionaler Varablenvekor
s.t.   ATy <= c     b = m-dimensionaler Kostenvektor
         y >= 0     A = n×m Constraint-Matrix
         c = n-dimensionale Right-Hand-Side
         T meint hier transponiert
```

**Für jedes primale Constraint eine duale Variable**<br>
**Für jede primale Variable ein duales Constraint**

---
>**Theorem**: Die optimalen Zielfunktionswerte des primalen und des dualen Programms sind gleich (sofern sie existieren). Man spricht von "starker Dualität"
---

```
                          <-LP-Gap->┌─────────────────────────────────────────────────
                                    |   ILP - Lösungen (nur die ganzzahligen :D)
──────────────────────────┬─────────┴─────────────────────────────────────────────────
     Duales Programm      |  LP-Relaxierung (Lineares Programm)
┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─
                ^         ^         ^         ^
                |         |         |         |
     zul. duale Lösung    |       OptILP    zul. primale Lösung
                       Duale Schranke
```
Jede feasable DP Lösung ist untere Schranke für ILP.<br>
DP-Approximationen haben eine Güte die _niemals_ besser als LP-Gap werden kann.

[up](#approximationsalgorithmen)

---

### Primal-Dualer Algorithmus

statt LP-Relaxierung:

mit kombinatorischen Algorithmen primale und duale Lösung finden und zeigen, dass diese nicht "zu weit" von einander entfernt sind.

**Vorteile** (gegenüber LP-basierten Verfahren): <br>
kein LP-Solver &rarr; bessere Laufzeiten

Sind ursprünglich für exakte Algorithmen für Probleme aus _P_ entwickelt worden.

Effizienteste Algorithmen für [Matching](#matching), Network-Flow, (Shortest-Path), ...

[up](#approximationsalgorithmen)

---

### Primal-Dualer Algorithmus

Sei &chi; (&gamma;) eine primale (duale) zulässige (nicht zwingend optimale) Lösung.

#### Primal Complementary Slackness Condition (PCSC)

Sei &alpha; &ge; 1. &forall; 1 &le; j &le; n: Entweder &chi;(j) = 0, oder &sum;(1 &le; i &le; m) a(ij) &chi;(i) &ge; c(j)/&alpha;

#### Dual Complementary Slackness Condition (DCSC)

Sei &beta; &ge; 1. &forall; 1 &le; j &le; m: Entweder &gamma;(j) = 0, oder &sum;(1 &le; i &le; n) a(ij) &gamma;(j) &ge; b(i) &middot; &beta;

Optimale (fraktionale) Lösungen erfüllen PCSC und DCSC mit &alpha; = &beta; = 1!

---
>**Theorem**: Seien z(&chi;) und z(&gamma;) die Zielfunktionswerte eines Lösungspaares das PCSC und DCSC erfüllt. Es gilt: z(&chi;) &le; &alpha; &middot; &beta; &middot; z(&gamma);
---

**Beweis** des [Theorems](#primal-dualer-algorithmus):

Verteile &alpha; &middot; &beta; &middot; z(&gamma;) viel Geld auf die dualen Variablen **y(i)**: <br>
Jedes **y(i**) erhält &alpha; &middot; &beta; &middot; b(i) &middot; &gamma;(i) viel Geld.

Duale Variablen "bezahlen" die primale Lösung.
* Jedes **y(i)** bezahlt an primale Variable x(j): &alpha; &middot; &beta; &middot; a(ij) &middot; &chi;(j) viel Geld.<br>
  **y(i)** gibt wegen [DCSC](#dual-complementary-slackness-condition-dcsc) nicht mehr Geld aus, als es hat:<br>
  &alpha; &middot; &gamma;(i) &middot; &sum;(1 &le; j &le; n) (a(ij) &middot; &chi;(j)) &le; &alpha; &middot;  &gamma;(i) &middot; b(i) &middot; &beta;
* Jede primale Variable x(j) erhält &alpha; &middot; &chi;(j) &middot; &sum;(1 &le; j &le; n) (a(ij) &middot; &gamma;(i)) viel Geld. <br>
  Wegen des [PCSC](#primal-complementary-slackness-condition-pcsc): &alpha; &middot; &chi;(j) &middot; &sum;(1 &le; j &le; n) (a(ij) &middot; &gamma;(i)) &le; &alpha; &middot; &chi;(j) &middot; c(j)/&alpha; = &chi;(j) &middot; c(j).<br>
  Summe des Geldes das auf x(j) Variablen verteilt wird ist &ge; &sum;(1 &le; j &le; n)(&chi;(j) &middot; c(j)) = z(&chi;)

#### (Allgemeines) Vorgehen:

* Starte mit **unzulässiger** (zielfunktionsmäßig "superoptimaler") _primaler_ Lösung und **zulässiger** (zielfunktionsmäßig suboptimaler) _dualer_ Lösung. <br>
  (meist trivial &chi; = 0, &gamma; = 0)
* **iterativ**: Verbessere Gültigkeit von &chi; und Optimalität von &gamma;. <br>
  Veränderungen von &chi; immer ganzzahlig, &gamma; bleibt immer zulässig.<br>
  Dabei: Benutze &chi; um zu wissen, wie man &gamma; ändern muss und umgekehrt.
* Terminiere mit zulässigem &chi;, so dass (für bestmögliches &alpha;, &beta;) PCSC und DCSC erfüllt sind.
* &gamma; gibt untere Schranke &rarr; Gütegarantie &alpha; &middot; &beta;

[up](#approximationsalgorithmen)

---

## Gütegarantie

### absolute Gütegarantie

* eine absolute Gütegarantie `k`mit `k > 0` bedeutet, dass der [APX](#approximationsalgorithmus-apx) für jede Probleminstanz `J` eine Gütegarantie garantiert:
  * **|Opt(J) - A(J)| &le; k**
  
  * nur sehr wenige Probleme haben [APX](#approximationsalgorithmus-apx) mit absoluter Gütegarantie.
    * hierfür lässt sich _meist_(!) der optimale Lösungswerte-Bereich `Opt` _instanzunabhängig_ einschränken und der APX berechnet eine triviale Lösung am pessimistischen Rand dieses Bereiches.
    
  * absolute Gütegarantien sind für **fast alle** halbwegs interessanten Probleme unmöglich!
#### Scaling:
* mögliche Art um zu beweisen, dass keine absolute Gütegarantie existiert.

_Beispiele_: [Rucksack](#rucksackproblem), [TSP](#traveling-salesman-problem)

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

## Approximationsschema

Familie von parametrisierten Approximationsalgorithmen

### polynomielles Approximationsschema (PTAS)

Für jede Konstante &epslion; &gt; 0 ist Algorithmus A&epsilon; eine (1&plusmn; &epsilon;)-Approximation. <br>
Laufzeit is polynomiell in Eingabegröße n (für konst. &epsilon;), Polynom hängt von &epsilon; ab.

_Beispiel_: O(n^(1/&epsilon;)

### effizientes polynomielles Approximationsschema (EPTAS)

PTAS mit einer Laufzeit von O(f(&epsilon; &middot; n^c), wobei c eine von &epsilon; unabhängige Konstante ist.

_Beispiel_: O(2^(1/&epsilon;) &middot; n^5)

### vollständig polynomielles Approximationsschema (FPTAS)
PTAS mit einer Laufzeit die polynomiell in n und 1/&epsilon; ist.

_Beispiel_: O(n^5/&epsilon^4)

[up](#approximationsalgorithmen)

---

## Dynamische Programmierung

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

---
> **Theorem**: Festzustellen, ob planarer Graph in 3 Farben knoten-färbbar ist, ist _NP-vollständig_.
---
> **Theorem**: Jeder Graph G lässt sich mit _maximal_ 5 Farben knoten-färben. (Es reichen sogar 4 ...)
---

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

---
> **Theorem**: Es ist _NP-vollständig_ festzustellen ob ein Graph G mit &Delta;(G) Farben kanten-einfärbar ist.
---

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
> **Theorem**: Falls P &ne; NP, so existiert kein [APX](#approximationsalgorithmus-apx) mit [absoluter Gütegarantie](#absolute-gütegarantie) für das Rucksackproblem.
---

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

### APX-Rucksack

#### Greedy Algorithmus (GA)
```
1. Sortiere Items absteigend nach Nutzen*.
2. Packe die Items in dieser Reihenfolge ein, falls es passt.

*Nutzen: Profit/Gewicht
```

_Beispiel_:
```
 _                      ^  OPT          B              ^  GA           B
|1| w=E/2             p |               |           p  |    ___________|__
|_| p=E                 |               |              |   |2          |  |
                        | ______________               |   |           |  |
 ______________         ||2             |              |   |           |  |
|2             |        ||              |              |   |           |  |
|   w=B        |        ||              |              |   |           |  |
|   p=B        |        ||              |              | _ |___________|__|
|              |        ||              |              ||1|            |
|              |        ||______________|              ||_|            |
|______________|        +---------------+---->         +---------------+---->
                                        B    w                         B    w
```
`OPT`/`GA` = `B`/`E`

Dieser Algorithmus liefert keine Gütegarantie.

[up](#approximationsalgorithmen)

---

#### Modified Greedy Algorithmus (MGA)
```
1. Sortiere Items absteigend nach Nutzen*.
2. Packe die Items in dieser Reihenfolge ein, falls es passt.
3. Vergleiche diese Lösung mit der Lösung, die nur das größte Element einpackt.

*Nutzen: Profit/Gewicht
```

---
> **Theorem**: `MGA`hat eine scharfe relative Gütegarantie von &alpha; = (1/2)
---

**Beweis** der Güte:

Sei `j` das erste Item, dass beim [GA](#greedy-algorithmus-ga) nicht eingepackt wird. <br>
Dementsprechend beim [FA](#fraktrionaler-algorithmus-fa) nur fraktional. <br>
`Z` ist eingepacktes Gesamtgewicht zum Zeitpunkt, an dem `j` nicht mehr passt.

Es gilt, dass `FA(J)`&le; `Z` + `pj` &le; `GA(J)` + `pj` ist. <br>
Der Profit von der fraktionalen Lösung ist somit höchstens so groß, wie die von `Z` auf das das nicht mehr in `B` passende Item `j`aufaddiert wird. Und Die Lösung des Greedy Algorithmus kann auch nicht kleiner sein, als letzteres (sie ist ja _mindestens_ `Z`groß).

Da `Opt(J)` &le; `FA(J)`, gilt auch `Opt(J)` &le; `GA(J)` + `pj`. <br>
Das lässt sich weiter durch `Opt(J)` &le; 2 &middot; max{`GA(J)`, `pj`} abschätzen. <br>
Schätzen wir weiterhin `pj` durch &pi; ab. &pi; sei das größte in `J` vorkommende Element.

`Opt(J)` &le; 2 &middot; max{`GA(J)`, &pi;}

`MGA` liefert **max{`GA(J)`, &pi;}**, dementsprechend ist `Opt(J)` &le; 2 `MGA`

**Beweis** der Schärfe:

3 Items. (w,p): <br>
i1 = (&epsilon;/2, &epsilon;), <br>
i2 = (B/2, B/2), <br>
i3 = (B/2, B/2)

Opt: i2, i3 mit Profit **B**

MGA: i1, i2 mit Profit **B/2 + &epsilon;**

[up](#approximationsalgorithmen)

---

### PTAS für den Rucksack

#### Modiified Greedy Algorithmus 1 (MGA1)
```
1. Sortiere Items absteigend nach Nutzen*.
2. for i = 1 ... n:
  - beginne neuen RUcksack mit Item i
  - vervollständige diese Rucksackinstanz mittels MGA it allen noch folgenden Items
3. return beste Lösung, der n Lösungen
```

`MGA1` rät das Element mit dem größten Profit in der Lösung.

---
> **Theorem**: `MGA1` hat eine scharfe Gütegarantie von &alpha; = 2/3.
---

`MGA2` rät die zwei Elemente mit dem größten Profiten in der Lösung.

---
> **Theorem**: `MGA2` hat eine scharfe Gütegarantie von &alpha; = 3/4.
---

`MGAk` rät die k Elemente mit dem größten Profiten in der Lösung.

---
> **Theorem**: `MGAk` hat eine scharfe Gütegarantie von &alpha; = (k+1)/(k+2).
---

**Laufzeit**: <br>
k Elemente aus n Items auswählen. O(n^k) mal `MGA` ausführen.

**Beweis** der Güte lass ich glaube ich (erstmal) hier raus. [Wird zu crazy in Markdown.]

**Beweis** der Schärfe:

n = k+3 Items. (w,p): <br>
i1 = (&epsilon;/2, &epsilon;) (kleines Item) <br>
i2...n = (M, M), i3 = (M, M) (große Items)

Opt: k+2 große Items mit Profit **(k+2) &middot; M**

MGA: i1 und k+1große Items mit Profit **(k+1) &middot; M + &epsilon;**

[up](#approximationsalgorithmen)

---

#### parametrisierter Algorithmus A&epsilon;:
* k := min{&lceil;1/&epsilon&rceil; - 2, n}
* Berechne `MGAk`

---
> **Theorem**: der parametrisierte Algorithmus A&epsilon; ist ein PTAS für das Rucksackproblem.
---
A&epsilon; hat eine scharfe Gütegarantie von (1/(&epsilon; - 1))/(1/&epsilon;) = 1 - &epsilon;.

A&epsilon; hat eine Laufzeit von (n^(1/(&epsilon; - 1))

[up](#approximationsalgorithmen)

---

### Rucksack: Dynamische Programmierung
(exaktes Lösen des Rucksackproblems)

Binärer Baum, also 2^n Möglichkeiten den (infeasable) Rucksack zu packen.

[up](#approximationsalgorithmen)

---

### fraktionales Rucksackproblem

**Gegeben** ist eine Menge I mit n Gegenständen (_items_) I := {1, 2, ..., n}.
Jedes _item_ n hat ein Gewicht w&#x2099; und einen Profit p&#x2099;.
Weiterhin gibt es eine Rucksackgröße B.

**Gesucht** ist eine Teilmenge I'&sube;I der _items_ mit w(I') := &Sigma; x&#x2099;w&#x2099; &le; B, so dass gleichzeitig p(I') := &Sigma; p&#x2099; maximiert wird. <br>
Dabei dürfen Gegenstände "zum Teil" eingepackt werden, x&#x2099; &isin;[0,1] ist der fraktionale Teil des Elements .

#### Fraktionaler Algoritmus (FA)
```
1. Sortiere Items absteigend nach Nutzen*.
2. Packe die Items in dieser Reihenfolge ein, falls es passt.
3. Sobald ein Item i nicht mehr passt, packe den fraktionalen Teil von i ein, der den Rucksack füllt.

*Nutzen: Profit/Gewicht
```
**Laufzeit**: O(n log n)

`FA(J)` liefert obere Schranke für das `Opt(J)` des klassisches Rucksackproblem.

[up](#approximationsalgorithmen)

---

## Clique

**Gegeben** ist ein Graph G=(V, E)

**Gesucht** ist die größte Clique C (= vollständiger Teilgraph) in G.

---
> **Theorem**: Falls P &ne; NP, so existiert kein [APX](#approximationsalgorithmus-apx) mit [absoluter Gütegarantie](#absolute-gütegarantie) für das Clique-Problem.
---

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
> **Theorem**: Bereits für m=2 ist Multiprocessor Scheduling _NP-vollständig_.
---

### List Scheduling
(Greedy-Algorithmus)
```
Jobs nacheinander auf die Maschinen legen - dabei den nächsten Job
immer auf die am Maschine legen, die zuerst Fertig wird.
```

_offline_ Problemstellung - d.h. es sind alle Jobs bekannt bevor der Algorithmus startet. Geht auch _online_ ...

---
> **Theorem**: List Scheduling hat eine [scharfe relative Gütegarantie](#scharfe-gütegarantie) von `2-1/m`
---

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

---
> **Theorem**: LPT hat eine [relative Gütegarantie](#relative-gütegarantie) von `(4/3) - (1/3)m`.
---

Optimale Lösung könnte gefunden werden.

**Laufzeit** O(m^(n-1))

#### parametrisiertes Scheduling

Algorithmus `Ak` ("`A` in Abhängikeit von Parameter `k`")
```
0. Sortiere Jobs
1. finde optimale Lösung des Problems für die k größten Jobs
2. schedule die weiteren Jobs mittels LPT
```

**Laufzeit** O(m^(k-1) + poly(n,m))

_Ab jetzt wird es endlich spaßig._

---
> **Theorem**: Algorithmus `Ak` garantiert für LPT eine garantierte Güte &alpha; &le; 1 + ((1-(1/m))/(1+&lfloor;k/m&rfloor;))
---

**Beweis**:

P ist (immernoch) die Summe der Laufzeiten aller Jobs. <br>
`K` ist die Schedule-Dauer nach Schritt 1. <br>
Wenn `Ak`(J) = `K`, dann ist Lösung optimal, sonst ist  `Ak`(J) &gt; `K` weil k &lt; n.<br>
`j` ist der Job, der bis `Ak`(J) läuft. Dann sind alle Maschinen im Zeitintervall [0, `Ak`(J) -`pj`] belegt.<br>
Es gilt also [immernoch](#list-scheduling):

`P` &ge; `m` &middot; (`Ak`(J) - `pj`) + `pj`

`P` &ge; `m` &middot; `Ak`(J) - (`m` - 1) &middot; `pj` <br>
`P` &ge; `m` &middot; `Ak`(J) - (`m` - 1) &middot; `pk+1` _(da `pk+1`&ge; `pj`)_

Da [immernoch](#list-scheduling) &rarr; `Opt(J)` &ge; `P`/`m` gilt:

`Opt(J)` &ge; `Ak`(J) - (1 - (1/`m`)) &middot; `pk+1` (4)

Die `k` größten Jobs sind optimal auf alle Maschinen verteilt.<br>
Durchschnittlich hat also jede Maschine &lfloor;k/m&rfloor; Jobs.
Genauer hat sogar mindestens eine Maschine 1 + &lfloor;k/m&rfloor; viele der `k`+ 1 größten Jobs. Wir erinnern uns, dass k &lt; n.<br>
Diese `k`+ 1 größten Jobs laufen jeweils mindestens `pk+1`. Das Optimum muss also mindestens so lange laufen:

`Opt(J)` &ge; (1 + &lfloor;`k`/`m`&rfloor;)) &middot; `pk+1`

Das könnte man nun nach  `pk+1` umstellen und in (4) einsetzen.

---

Nun wird A&epsilon; aus `Ak`gebildet. Dafür wird das `k`groß genug gewählt. <br>
&epsilon; &ge; ((1-(1/m))/(1+&lfloor;k/m&rfloor;)) <br>
&epsilon; &ge; ((1-(1/m))/**(1+(k/m)))** <br>
**&epsilon; + ((&epsilon;k) / m)** &ge; 1 - (1/m) <br>
((&epsilon;k) / **m**) &ge; 1 - (1/m) - &epsilon; <br>
&epsilon;k &ge; **m** - 1 - **m**&epsilon; = (1-&epsilon;)m - 1 <br>
k &ge; ((1-&epsilon;)m - 1)/&epsilon; <br>

Man könnte `k` jetzt auch einfach &ge; &lceil;m/&epsilon;&rceil; wählen.

**Laufzeit?** O(m^(k-1) + poly(n,m)) &rarr; O(m^(m/&epsilon) + poly(n,m))

Ist polynomiell für konstantes &epsilon; und m!

---
> **Theorem** A&epsilon; ist [EPTAS](#effizientes-polynomielles-approximationsschema-eptas) für das Scheduling-Problem mit fester Anzahl an Maschinen (m)!
---

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
> **Theorem**: Falls P &ne; NP, existiert kein [APX](#approximationsalgorithmus) mit [relative Gütegarantie](#relative-gütegarantie) &alpha; &lt; 1.5 für Bin-Packing.
---

**Beweis** mit [Partitionsproblem](#partitionsproblem):

*Gegeben* ist beliebige Instanz J des Partitionsproblems. Seien eine Zahl das Itemgewicht beim Bin Packing.
Sowie `s`/2 die Bin-Kapazität.
* Partitionsproblem erfüllbar &rarr; 2 Bins reichen
* Partitionsproblem unerfüllbar &rarr; (mind.) 3 Bins nötig

Für das erfüllbare Problem müsste der Algorithmus A mit &alpha; &lt; 1.5 weniger als 3 Bins brauchen <br>
&rarr; A würde nur 2 Bins benötigen und damit _NP-vollständiges_ Problem ([Partition](#partition)) lösen.

---

### Next-Fit
(NF) (Greedy Algorithmus) Laufzeit: `O(n)`
```
Lege items der Reihe nach in den aktuellen Bin.
Falls item nicht passt, neuen Bin aufmachen.
```
---
> **Theorem**: Next-Fit hat eine scharfe (auch [asymptotische](#asymptotische-gütegarantie) [relative Gütegarantie](#relative-gütegarantie) &alpha; = 2.
---

**Beweis** der Güte:

Betrachte zwei beliebige benachbarte Bins.
Summe der beiden Bins ist größer als B, sonst hätte NF die Items des 2. Bins noch in den 1. gepackt. <br>
&rarr; NF benötigt maximal doppelt so viele Bins  wie Opt.

[up](#approximationsalgorithmen)

---

### First-Fit
(FF) (Greedy Algorithmus) Laufzeit: `O(n log(n)`
```
Lege items der Reihe nach in den ersten Bin, in dem es Platz findet.
```
---
> **Theorem**: First-Fit hat eine scharfe (auch [asymptotische](#asymptotische-gütegarantie) [relative Gütegarantie](#relative-gütegarantie) &alpha; = 2.
---

**Beweis** der Güte:

Betrachte zwei beliebige Bins.
Summe der beiden Bins ist größer als B, sonst hätte NF die Items des 2. Bins noch in den 1. gepackt. <br>
&rarr; FF benötigt maximal doppelt so viele Bins  wie Opt.

[up](#approximationsalgorithmen)

---

### und mehr Fits ...
* Best-Fit (BF)
* Worst-Fit (WF)
* Almost-Worst-Fit (AWF)
* Any-Fit (AF)

"Es gibt keine bessere Strategie als FF/BF und keine Strategie kann schlechter als NF sein."

---

#### Worst-Case Beweis

First-Fit mit &exist;J: FF(J) &ge; (5/3) &middot; Opt(J))

Instanz:
* 18m Items (Reihenfolge)
  * 6m mal (1/7)+&epsilon;
  * 6m mal (1/3)+&epsilon;
  * 6m mal (1/2)+&epsilon;
  
**Opt** = 6m

|Anzahl| Inhalt|
|---|---|
|6m Bins | je ein Item pro Typ|

**FF** = 10m

|Anzahl| Inhalt|
|---|---|
|m Bins | 6x (1/7)+&epsilon; |
|3m Bins | 2x (1/3)+&epsilon; |
|6m Bins | (1/2)+&epsilon; |

10m/6m = 5/3

Problem ist nur die "_dumme_" Reihenfolge, sonst würde FF das Optimum liefern.

---

### erweiterte Bin Packing Heuristiken

* Fit erst nach Größe sortieren. (Decreasing)

---
> **Theorem**: First-Fit-Decreasing und Best-Fit-Decreasing haben scharfe (nicht-asymptotische) [relative Gütegarantie](#relative-gütegarantie) &alpha; = (3/2). <br>
  *besser geht nicht!*
---
> **Theorem**: First-Fit-Decreasing und Best-Fit-Decreasing haben scharfe asymtotische [relative Gütegarantie](#relative-gütegarantie) &alpha; = (11/9). <br>
  *besser als nicht-asymptotisch*
---

(ohne Güte-Beweis!)

**Schärfe**:

Instanz:
  * `a` 6m mal (1/2)+&epsilon;
  * `b` 6m mal (1/4)+2&epsilon;
  * `c` 6m mal (1/4)+&epsilon;
  * `d` 12m mal (1/4)-2&epsilon;
  
  **Opt** = 9m
  
  |Anzahl| Inhalt|
  |---|---|
  |6m Bins | `a` + `c` + `d` |
  |3m Bins | `b` + `b` + `d` + `d`|
  
  **FF** = 11m
  
  |Anzahl| Inhalt|
  |---|---|
  |6m Bins | `a` + `b` |
  |2m Bins | `c` + `c` + `c` |
  |3m Bins | `d` +  `d` +  `d` +  `d`|

11m/9m = 11/9

---
> **Theorem**:  Modified First-Fit Decreasing (MFFD) (mit speziellen Regen für Items &isin;(1/6, 1/3] hat scharfe asymptotische relative Gütegarantie von &alpha; = 71/60 = 1.18333....
---

[up](#approximationsalgorithmen)

---

## Partition

**Gegeben** ist eine Menge natürlicher Zahlen I := {a1, a2, ..., an}. Aufsummiert ergeben die Zahlen `s`.

**Gesucht** ist eine Aufteilung der Zahlen in zwei Gruppen A1 und A2, sodass ihre Summe je `s`/2 ist.

[up](#approximationsalgorithmen)

---

## Traveling Salesman Problem
(TSP)

**Gegeben** sind `n` Orte und Distanzen `d` zwischen ihnen. Repräsentiert als vollständiger Graph G=(V, E) mit V als Orte und Kantenkosten als Distanzen.

**Gesucht** ist die kürzeste Rundreise, die alle Orte _genau_ 1x besucht <br>
(_NP-schwer_!) <br>
Liegt nicht in APX-schwer!

---
> **Theorem**: Falls P &ne; NP, existiert kein [APX](#approximationsalgorithmus) mit konstanter [absoluter Gütegarantie](#absolute-gütegarantie) k.
---

**Beweis** durch [Scaling](#scaling). (hier nicht.)

---
> **Theorem**: Falls P &ne; NP, existiert kein [APX](#approximationsalgorithmus) mit konstanter [relativer Gütegarantie](#relativer-gütegarantie) k.
---

**Beweis**:

Angenommen es gäbe Algorithmus A mit [relativer Gütegarantie](#relativer-gütegarantie)  &alpha;.

Gegeben ist eine beliebige Instanz G des [Hamilton-Kreis Problems](#hamilton-kreis).
Jede Kante in G hat Länge 1.
Erweitere G zu einem vollständigen Graphen, dabei haben die neu entstehenden Kanten die Länge n&alpha;+1.

G hat Hamilton-Kreis der Länge n, der gleichzeitig optimale TSP-Lösung ist.

Die zweitbeste TSP-Lösung hätte die Länge &ge; n-1 + n&alpha;+1 = n(&alpha; + 1).

Da diese Lösung aber größer ist als Opt &middot; &alpha; müsste der Algorithmus A den Hamiltonkreis finden.

[up](#approximationsalgorithmen)

---

## Metrisches TSP
(&Delta;TSP)

Metrik? Abbildungsfunktion.
1. Identität
2. Symmetrie
3. Dreiecksungleichung (d(u,w) &le; d(u,v) + d(v,w))

* Oft sind TSPs metrisch (Landkarte, ...)
* falls nich (Fahrzeiten in einem Straßennetz), reicht oft "besuche jeden Ort _mindestens_ 1x"
  * Instanzen metrisieren, falls d(u,w) &gt; d(u,v) + d(v,w): mache d(u,w) = d(u,v) + d(v,w)

**Trotzdem**: <br>
_NP-schwer_, aber viel einfacher zu approximieren
-APX-schwer_, lässt sich nicht besser als 220/219 approximieren

---

### Approximationsheuristiken für &Delta;TSP

#### Nearest-Neighbor

```
Starte irgendwo, nimm nächsten Nachbarn.
Gehe zuletzt zurück zum Start.
```

---

#### Insertion-Algorithmen

```
Starte mit trivialer Tour. z.B. < 3 Knoten
Wähle Knoten der nicht in der Tour liegt nach (*) Kriterium.
Ersetze eine Kante mit den Kanten die den neuen Knoten umschließen,
sodass der Umweg am kleinsten ist.

(*) nearest, cheapest, farthest, random, ...
```


Nearest/Cheapest Insertion: *relative Güte*: &alpha; = 2 (scharf) <br>
Farthest Insertion: *relative Güte*: &alpha; &ge; 2.43 <br>
Random Insertion: *relative Güte*: &alpha; &ge; log(log(n)) / log(log(log(n)))

---
> **Theorem**: Jede mögliche Insertion-Strategie hat eine relative Güte &alpha; &le; log(n) + 1.
---

#### Verbesserungsheuristiken: 2-Opt (_k-Opt_)
```
Mit beliebiger (feasable) Tour starten.
Alle Kantenpaare (a,b) und (x,y) überprüfen ob (a,x) und (b,y) Tour verbessert.
```

---

> **Theorem**: 2-Opt hätte &alpha; &ge; n^(1/2)

---

**Laufzeit**: O(n&sup2;) pro Iteration. <br>
Es gibt aber Instanzen die exponentiell viele Iterationen benötigen.

&rarr; **kein** [APX](#approximationsalgorithmus)!

#### k-Opt
k Kanten tauschen ...

Lin-Kernighan Heuristik: k-Opt mit wachsendem k und Tricks und so. <br>
&rarr; beste Heuristik in der Praxis.

[up](#approximationsalgorithmen)

---

### MST-Heuristik
```
1. Berechne minimalen Spannbaum B von Graph G.
2. Verdopple Kanten von B -> es entsteht Eulertour ET
3. Laufe Eulertour lang und "überspringe" schon besuchte Knoten
4. Freue dich über die entstandende TSP-Tour
```

---
> **Theorem**: MST-Heuristig hat für &Delta;TSP scharfe Gütegarantie von &alpha; = 2.
---

**Beweis** der Güte:

`O` sei optimale TSP Tour. Entfernen wir beliebige Kante aus `O`, entsteht Spannbaum. <br>
Dieser Spannbaum ist &ge; d(B) <br>
Weiterhin ist d(ET) = 2 &middot; d(B) <br>
Wegen der &Delta;-Ungleichung ist d(T) &le; d(ET)

Somit ergibt sich, dass d(T) &le; 2 &middot; Opt(J)

[up](#approximationsalgorithmen)

---

### Christofides-Heuristik
```
1. Berechne minimalen Spannbaum B von Graph G.
2. Berechne minimales perfektes Matching M zwischen den Knoten mit ungeradem Grad in B
   [O(n^3)]
3. Vereinige die Knotenmengen aus B und M, daraus entsteht Eulertour ET.
4. Laufe Eulertour lang und "überspringe" schon besuchte Knoten
5. Freue dich über die entstandende TSP-Tour
```

---
> **Theorem**: Christofides-Heuristig hat für &Delta;TSP scharfe Gütegarantie von &alpha; = 1.5.
---

&rarr; beste bekannte Approximationsgüte.

[up](#approximationsalgorithmen)

---

## Euler-Tour

**Gegeben**: Multigraph G (mit Mehrfachkanten)

**Gesucht**: Euler-Tour in diesem Multigraphen G, der jede Kante genau einmal besucht.

---
> **Theorem**:
> * G erlaubt eine Euler-Tour, genau dann wenn alle Knoten geraden Grad haben.
> * G erlaubt einen Euler-Pfad, genau dann wenn alle Knoten bis auf 2 geraden Grad haben.
---

[up](#approximationsalgorithmen)

---

## Hamilton-Kreis

**Gegeben** ist ein Graph G.

**Gesucht** ist ein Kreis in G, der jeden Knoten genau einmal besucht.

Problem ist _NP-vollständig_.

[up](#approximationsalgorithmen)

---

## EdgeCover

[up](#approximationsalgorithmen)

---

## IndependentSet

**Gegeben** ist ein Graph G = (V, E).

**Gesucht** ist eine (möglichst große) Menge von Knoten, die nicht durch Kanten verbunden ist.
* Formal: **&forall; (v, w) &isin; E: {v, w}&notin;S**

Beobachtung:

Wenn C ein [VC](#minimum-vertexcover) ist, ist V\C ein IS. &harr;
Wenn C ein [minimum VC](#minimum-vertexcover) ist, ist V\C ein maximum IS. &harr;

[up](#approximationsalgorithmen)

---

## Matching

### Maximal Matching

**Gegeben** ist ein Graph G = (V, E).

**Gesucht** ist eine Menge von Kanten M, sodass keine weitere Kante e existiert, die ein weiteres Matching zur Menge hinzufügen kann.

### Maximum Matching

**Gesucht** ist eine Menge von Kanten M, sodass es kein anderes Matching gibt, das für diese Instanz besser ist.
Das Matching M hat die maximale Kardinalität in G.

### Perfektes Matching

**Gesucht** ist eine Menge von Kanten M, sodass |V|=2&middot;|M| gilt. Jeder Knoten ist gematcht.

[up](#approximationsalgorithmen)

---

## Minimum VertexCover

**Gegeben** ist ein Graph G = (V, E).

**Gesucht** ist eine (möglichst kleine) Knotenmenge C &sube; V die mit jeder Kante im Graphen inzident ist.
* Formal: min(**&forall; (v, w) &isin; E: {v, w}&cap;C&ne; &empty;**)

---

Beobachtung:

Wenn S ein [IS](#independentset) ist, ist V\S ein VC. &harr;
Wenn S ein [maximum IS](#independentset) ist, ist V\S ein minimum VC. &harr;

Beobachtung:

Wenn M ein beliebiges Matching in G ist, gilt, dass |M| &le; |C| ist.

---

### `Greedy-1`

```
C := null
H := G
while E(H) != null:
  choose random edge e (from E(H))
  choose incident vertex v from e
  C += v
  H -= v, E(H) -= e(v)      // delete v with incident edges from H
```

keine Gütegarantie.

**Beweis:**

Sterninstanz. Algorithmus wählt immer einen der äußeren Knoten.

`Opt(G)` wäre 1, `Greedy-1` hat n-1`.

[up](#approximationsalgorithmen)

---

### `Greedy-2`

```
C := null
H := G
while E(H) != null:
  choose random edge e (from E(H))
  choose the incident vertex v from e with the higher cardinality deg(v)
  C += v
  H -= v, E(H) -= e(v)      // delete v with incident edges from H
```

---
>**Theorem**: `Greedy-2` garantiert nur einen Gütefaktor von &Omega;(log n).
---

Beobachtung:

`Greedy-2` garantiert einen Gütefaktor von &theta;(log n).

**Beweis** Konstruktion:

n = O(r log r)<br>
Knotenmengen `L`, `R1`, ..., `Rr`<br>
`|L|` = `r` <br>
`|Ri|` = &lfloor;`r`/`i`&rfloor;

* Jeder Knoten in `Ri` hat `i` Kanten nach `L`
* Jeder Knoten in `L` hat nur max. eine Kante nach `Ri`.

`Opt(G)` wäre r (nämlich die Knotenmenge `L`, jede Kanten hat einen Knoten in `L`)

`Greedy-2` könnte alle Knoten aus allen `Ri` wählen. Hat also O(r log n) Knoten.

[up](#approximationsalgorithmen)

---

### `Greedy-Matching`

```
C := null
H := G
while E(H) != null:
  choose random edge e (from E(H))
  choose both incident vertices v, w from e
  C += v, w
  H -= v, w, E(H) -= e(v), e(w)      // delete v and w with incident edges from H
```

---
>**Theorem**: `Greedy-Matching` garantiert einen scharfen Gütefaktor von **2**.
---

**Beweis**:

#### Korrektheit:
Annahme `C`ist kein VC, dann existiert eine Kante e aus E, für die beide Knoten noch nicht in `C` sind. Dann wäre e noch in H und der Algorithmus ist noch nicht fertig.

#### Güte:
Die von `Greedy-Matching` gewählten Kanten bilden ein maximales [Matching](#matching).
Wir wissen |M|&le; `Opt(G)`.
Wir wählen aber genau 2|M| Knoten.

#### Schärfe:
G sei Pfad mit gerader Anzahl Knoten.

`Opt(G)` = n/2 wählt jeden 2. Knoten.

`Greedy-Matching` = n wählt jeden Knoten.

[up](#approximationsalgorithmen)

---

### `Greedy-Random`

```
C := null
H := G
while E(H) != null:
choose random edge e (from E(H))
choose one of the incident vertices v with the probability 50:50 // with coin toss
C += v
H -= v, E(H) -= e(v)                  // delete v and w with incident edges from H
```


---
>**Theorem**: `Greedy-Random` garantiert eine (scharfe) **erwartete Güte** von **2**.
---

**Beweis**:

Korrektheit: Jede Kante wird gecovert, sonst wäre sie noch in H und der Algorithmus würde noch laufen.

Güte: `Greedy-Random`  betrachtet die Kanten F. Jedes e&isin;F wird in `Opt(G)` gecovert. Der Münzwurf hat eine Wahrscheinlichkeit von &ge; 50% "gut" zu sein, d.h. einen Knoten aus `Opt(G)` zu wählen. Nach maximal `Opt(G)` vielen "guten" Münzwürfen terminiert der Algorithmus (weil dann ist ja alles gecovert).<br>
&rarr; es gibt maximal 2&middot;`Opt(G)` Münzwürfe. (So würde jeder Münzwurf die Lösung um 1 Knoten erhöhen)


[up](#approximationsalgorithmen)

---

## WeightedVertexCover

**Gegeben** ist ein Graph G = (V, E). Jeder Knoten `v` hat ein Gewicht w(`v`)

**Gesucht** ist die günstigsteKnotenmenge C &sube; V die mit jeder Kante im Graphen inzident ist.
* Formal: min(**&forall; (v, w) &isin; E: {v, w}&cap;C&ne; &empty;**)

### `W-Greedy-1`

```
C := null
H := G
while E(H) != null:
  choose vertex v with deg(v)>0 and minimum weight // so it has any edges
  C += v
  H -= v, E(H) -= e(v)                             // delete v with incident edges from H
```

keine Gütegarantie.

**Beweis:**

Sterninstanz innerer Knoten hat Gewicht 1+&epsilon;, alle äußeren Gewicht 1.
Algorithmus wählt immer einen der äußeren Knoten.

`Opt(G)` wäre 1+&epsilon, `W-Greedy-1` hat n-(1+&epsilon)`.

[up](#approximationsalgorithmen)

---

### `W-Greedy-2`

```
C := null
H := G
while E(H) != null:
  choose vertex v with minimum w(v)/(deg(v)>0)     // minimum weight per edge ...
  C += v
  H -= v, E(H) -= e(v)                             // delete v with incident edges from H
```
`w(v)/deg(v)` wählt billigen Knoten mit hohem Grad.

---
>**Theorem**: `W-Greedy-2` garantiert einen Gütefaktor von **&theta;(log n)**.
---

[up](#approximationsalgorithmen)

---

### `W-Greedy-Matching`
```
C := null
H := G
while E(H) != null:
  choose edge e (from E(H)), with min(w(u) + w(v))
  C += u, v
  H -= u, v, E(H) -= e(u), e(v)      // delete u and v with incident edges from H
```

Beobachtung:
`W-Greedy-Matching` hat keine konstante Gütegarantie. :(

**Beweis**:
Zum Beispiel V={u,v}, E={(u,v)} mit w(u) = 1,  w(v) = beliebig hoch.

`Opt(G)` wäre 1, `W-Greedy-Matching` hat 1+ beliebig hoch.

[up](#approximationsalgorithmen)

---

### `W-Greedy-2-Modified`
```
C := null
H := G
w'(v) = w(v) for all v
while E(H) != null:
  choose vertex v with minimum alpha = w'(v)/(deg(v)>0)    // minimum weight per edge ...
  for all adjacent vertices u:
    w'(u) -= alpha                                         // all neigbours will get more expensive
  C += v, w'(v) = 0
  H -= v, E(H) -= e(v)                                     // delete v with incident edges
```

---
>**Theorem**: `W-Greedy-2-Modified` hat einen Gütefaktor von **2**.
---
**Beweis**:

**Lemma A.** Zu jedem Zeitpunkt gilt: &forall; x &isin; V: w'(x) &ge; 0.

Immer wenn sich ein w'(x)  (mit x als u) ändert, gilt:
* Knoten x und v haben beide min. Grad 1 in H.
* Knoten v hat kleinstes &alpha; (`alpha`)
&rarr; w'(v) ist auf jeden Fall &ge; w'(v)/deg(v), da es nicht negativ werden kann.

---

**Lemma B.** Zu jedem Zeitpunkt gilt: &forall; x &isin; V: w(x) = w'(x) + &sum;(y&isin;NG(x)) &alpha;(y,x). <br>
*("+ die Summe der bereits wegen der Nachbarschaft abgezogenen &alpha;'s)*

Anfangs ist w(x) = w'(x)

Immer wenn sich ein w'(x)  (mit x als u) ändert, dann wird es um ein eindeutiges &alpha;(x,v) reduziert.
Gleichzeitig wird e(x,v) gelöscht (aber auf der rechten Seite des Terms im Lemma B hinzuaddiert)

Immer wenn sich ein w'(x)  (mit x als v) ändert, dann wird es auf 0 gesetzt.
Dadurch wird es um deg(v) &middot; &alpha; reduziert.
Gleichzeitig werden deg(v) viele Kanten (x, u) gelöscht (aber auf der rechten Seite des Terms in Lemma B hinzuaddiert)

---

**Lemma C.** &forall; x &isin; C: w(x) = &sum;(y&isin;NG(x)) &alpha;(y,x) und &forall; x &notin; C: w(x) &ge; &sum;(y&isin;NG(x)) &alpha;(y,x). <br>
*(ist x in C, so ist sein Gewicht die Summe der &alpha;'s, ist es nicht in C, so ist es &ge; der Summe der &alpha;'s)*

Folgt aus **Lemma B**.

---

**Lemma D.**  w(C) &le; 2 &middot; &sum;(e&isin; E(G)) &alpha;(e).

Sei &alpha;(e) der &alpha; Quotient als e entfernt wurde, bzw. 0, wenn e noch in H ist.

w(C) = &sum;(x&isin; C) w(x) = &sum;(x&isin; C) [&sum;(y&isin;NG(x)) &alpha;(y,x)] (aus **Lemma C**)

Da in der Summe jede Kante *maximal* 2 mal vorkommt, stimmt **Lemma D**.

---

**Lemma E.** &sum;(e&isin; E(G)) &alpha;(e). &le; `Opt(G)`.

Sei `C*` optimales VC.

`Opt(G)` = w(`C*`) = &sum;(x &isin; `C*`) w(x) &le; &sum;(x &isin; `C*`) &sum;(y&isin;NG(x)) &alpha;(y,x)

Da in der Summe jede Kante (also jedes &alpha;(e) *genau* einmal vorkommt, stimmt **Lemma E**.
Im Optimum muss (natürlich) jede Kante sein.

Aus **Lemma D** und **Lemma E** lässt sich nun ableiten, dass das Theorem gilt.

[up](#approximationsalgorithmen)

---

### Traditionelle Algorithmen für [VC](#minimum-vertexcover) und [WVC](#weightedvertexcover)

Diese "traditionellen Algorithmen" erlauben also in [APX](#apprapproximationsalgorithmus-apx) eine Gütegarantie von 2.
Geht es (asymptotisch) besser als 2?

---
>**Theorem** VC erlaubt **kein** [Approximationsschema](#approximationsschema).
---

---

### randomisiertes WVC

### `W-Greedy-Random`

```
C := null
H := G
while E(H) != null:
choose random edge e (from E(H))
choose x = (u or v) with the probability Prob[x=v]=(w(u))/(w(u)+w(v)) //= probability for v
C += x
H -= x, E(H) -= e(x)                  // delete v and w with incident edges from H
```
---
>**Theorem** `W-Greedy-Random` garantiert eine (scharfe) **erwartete Güte** von **2**.
---

**Beweis** "durch Spiel":

* Reihenfolge der Kanten ist zwar beliebig, steht aber fest.
* Resultierendes VC `C`ist zufällige Teilmenge von V(G).<br>
&rarr; Wahrscheinlichkeits**verteilung** von `C`steht _a priori_ fest (wie auch immer)

* Zufallsvariable X(v) = w(v) falls v&isin;C - sonst X(v)=0.
* Erwartungswert e(v) = **E**[X(v)] ist der Erwartungswert von X(v)
>"X(v)/e(v) ist der tatsächliche/wahrscheinliche Anteil des Knotens v am WVC"<br>
&rarr; Wahrscheinlichkeits**verteilung** von X(v) und exakter Wert von e(v) stehen_a priori_ fest"

&rarr; w(C) = &sum;(v &isin; V(G)) X(v)<br>
&rarr; **E**[w(C)] = &sum;(v &isin; V(G)) e(v)

Sei `C*` das optimale VC, welches zu Beginn fest steht.

`C'` = `C` &cap; `C*`  "= die (vom Algo) richtig erkannten Knoten"

&rarr; w(`C'`) = &sum;(v&isin;`C*`) X(v)<br>
&rarr; **E**[w(`C'`)] = &sum;(v &isin; `C*`) e(v)

Da `C'`&sube; `C*` und e(v) &le; w(v): **E**[w(C')] =&le; w( `C*`) = `Opt(G)`

---

**Idee** für das Spiel:
* jeder Knoten v &isin; V erhält e(v) Geldeinheiten, sodass die Gesamtgeldmenge, die verteilt wird E[w(C)]
* Geldeinheiten werden geschickt zwicshen den Knoten verschoben, so dass:
  * am Ende jeder Knoten v&isin; `C*` sein Geld maximal verdoppelt
  * alle anderen Knoten sind pleite.

Wenn das klappt, stimmt das **Theorem**, denn:

**E**[w(`C`)] = 2 &middot; &sum;(v &isin; `C*`) e(v) &le; 2 &middot; `Opt(G)`

#### Verschieberegeln

Schritt 1: <br>
Jeder Knoten verteilt sein Geld vollständig auf seine inzidenten Kanten. (Jede Kante bekommt demnach von zwei Knoten Geld) - aber: Jede Kante e(u,v) enthält von u und v gleichviel Geld.

Schritt 2: <br>
Für jede Kante e(u,v) mit $:<br>
if {u,v}&isin;`C*`: u und v erhalten jeweils $/2.

**Lemma.** Jeder Knoten kann sein Geld e(v) so auf inzidente Kanten verteilen dass gilt: <br>
** Jede Kante (u,v) erhält von u gleich viel wie von v.**

**Beweis:**

Seien F die im Algorithmus gewählten Kanten. N(v) die zu v adjazenten Knoten.<br>
Zufallsvariable X(v, u) = w(v) falls v&isin;C ab Iteration der Kante (i,v)&isin; F, sonst 0.
* für jede Kante (u, v)&isin; F: entweder X(v,u) oder X(u,v) ist &gt; 0.
* X(v) = &sum;(u&isin; N(v)) X(v, u)
&rarr; e(v) = **E**[X(v)] = &sum;(u&isin; N(v)) **E**[X(v, u)] - _(**E**[X(v, u)] a priori bekannt!)_<br>
&rarr; Verteilungsstrategie für Geld e(v): Gib **E**[X(v, u)] jeweils an die Kante (v,u)

Zu zeigen: **E**[X(v, u)] = **E**[X(u, v)] :

**E**[Xv,u] = w(v) &middot; Prob[ (v,u) &isin; F und wählt dabei v ]<br>
= w(v) &middot; Prob[ (v,u) &isin; F ] · w(u)/w(u)+w(v)<br>
= w(u) &middot; Prob[ (v,u) &isin; F ] · w(v)/w(u)+w(v)<br>
= w(u) &middot; Prob[ (v,u) &isin; F und wählt dabei u ] = **E**[Xu,v]

&rarr; &exist; Verteilungsstrategie, so dass am Ende nur die Knoten aus `C*` Geld haben, und zwar maximal 2x so viel wie am Begin des Spiels.

Somit stimmt das **Theorem**.

[up](#approximationsalgorithmen)

---

## Minimum SetCover

**Gegeben** ist eine n-elementige Grundmenge M und eine Familie &sum; = {S1, S2, ...} von Teilmengen über M.

**Gesucht**: eine (minimale) Teilfamilie C&sube;&sum; ist ein SetCover, wenn jedes Element aus M in mindestens einer Menge aus C enthalten ist.

**Beispiel:**
M = { a, b, c, d, e, f, g }<br>
&sum; = { {a,b,d}, {a,c,e,f}, {f,g}, {d,g}, {b,e,g}, {c,d,f} } <br>
Lösung C = { {a,b,d}, {b,e,g}, {c,d,f} }

---

VertexCover als SetCover.

Grundmenge M ist die Menge der Kanten. <br>
Familien &epsilon; sind die Knoten.

## WeightedSetCover

### `WSC-Greedy`
(basiert auf [`W-Greedy-2`](#w-greedy-2)

```
C := null
N := null
while N != M:
choose S with minimal relative cost alpha := w(S) / |S\N|
choose incident vertex v from e
C = C &cup; {S}    // put S into the cover C
N = N &cup; S      // put elements in S to the covered elements in N
```
w(S) / |S\N| ... wählt die Menge, die pro ungecovertem Element am günstigsten ist.

---
>**Theorem** `WSC-Greedy` hat eine scharfe Gütegarantie Hn = 1 + (1/2) + (1/3) + ... + (1/n)
---

#### Harmonische Reihe
_(Hn? &rarr; Harmonische Reihe! Konvergiert gegen unendlich!)_

**Beweis**:

`m1`, `m2`, ... = Elemente M in der Reihenfolge in der `WSC-Greedy` sie covert (Reihenfolge beliebig, wenn gleichzeitig)
&alpha;(k) = relativer Preis zum Zeitpunkt wenn Element `mk` gecovert wird.`

**Lemma.** Für alle 1 &le; k &le; n: &alpha;(k) &le; `Opt(G)`/(n-k+1).

Sei `C*` das optimale SetCover.

Zu jedem Zeitpunkt k: Man kann mit Kosten &le; `Opt(G)` ein SetCover erzeugen, indem man die Mengen `C*`\ `C` hinzunimmt.<br>
&rarr; &exist; S'&isin; `C*`\ `C` mit relativem Preis &alpha;(S') &le; `Opt(G)`/|M-N| = `Opt(G)`/(n-k+1)<br>
(kleiner, wenn mehrere `mi` gleichzeitig gecovert werden) <br>
&rarr; es wird Menge S mit minimalen relativen Preis &alpha; gewählt, also &alpha; &le; &alpha(S')

**Güte**:

Die Kosten einer gewählten Menge S werden immer als relative Preise komplett auf die neu gecoverten Knoten (in S) augeteilt.

&rarr; w(`C`) = &sum;(1&le;k&le;n) &alpha;(k) &le; &sum;(1&le;k&le;n) `Opt(G)`/|M-N| = `Opt(G)` &middot; ((1/n) + (1/n-1) + ... + (1/3) + (1/2) + 1) = `Opt(G)` &middot; Hn

**Schärfe**:
```
Beispielinstanz:

       S1      S2      S3            Sn
     ┌───┐   ┌───┐   ┌───┐         ┌───┐
  ┌──┼───┼───┼───┼───┼───┼─────────┼───┼──┐
  │  │   │   │   │   │   │         │   │  │ Sn+1
  │  │ • │   │ • │   │ • │   ···   │ • │  │
  │  │   │   │   │   │   │         │   │  │ 1/1+ε
  └──┼───┼───┼───┼───┼───┼─────────┼───┼──┘
     └───┘   └───┘   └───┘         └───┘
      1/n    1/n-1   1/n-2          1/1
     
```
`Opt(G)` wählt `C*` ={Sn+1} &rarr; **Kosten:** 1+&epsilon;,
`WSC-Greedy` wählt `C` = {S1, S2, S3, ..., Sn} &rarr; **Kosten:** 1+(1/2)+(1/3)+ ... + (1/n) = Hn

---
>**Theorem**: `WSC-Greedy` garantiert einen Gütefaktor Hn.
---

Hn liegt irgendwo zwischen:

ln(n) + 1/n &le; Hn &le; ln(n) + 1

---
>**Theorem**: Entweder `[sehr-unwahrscheinliches-Komplexitätsresultat]` oder [WSC](#weightedsetcover) lässt sich nicht besser als **(1/2) ln(n)** approximieren
---

`WSC-Greedy` ist also im wesentlichen der "bestmögliche" Algorithmus.

[up](#approximationsalgorithmen)

---
## Linear Programming

### [WSC](#weightedsetcover) als ILP

Variablen:

* binäre Variablen x(s) &isin {0, 1} &forall; S &isin; &sum;:
* x(s) = 1 &harr; S &isin; C

ILP:

**min**  ` `  ` `  ` `  &sum;(S &isin; &Sigma;) w(S) &middot; x(S) <br>
**s.t.** ` ` ` ` ` ` &sum;(S:m&isin;S) x(S) &ge; 1   ` `  ` `  ` `  ` `&forall; m &isin; M <br>
` `  ` `  ` `  ` `  ` `  ` `x(S) &isin; {0, 1}   ` `  ` `  ` `  ` `  ` `  ` `  ` `  ` `&forall; S &isin; &Sigma;


LP-Relaxierung:

**min**  ` `  ` `  ` `  &sum;(S &isin; &Sigma;) w(S) &middot; x(S) <br>
**s.t.** ` ` ` ` ` ` &sum;(S:m&isin;S) x(S) &ge; 1   ` `  ` `  ` `  ` `&forall; m &isin; M <br>
` `  ` `  ` `  ` `  ` `  ` `x(S) &ge; 1   ` `  ` `  ` `  ` `  ` `  ` `  ` `  ` `  ` `  ` `&forall; S &isin; &Sigma;

Duales Programm:

**max**  ` `  ` `  ` `  &sum;(m &isin; M) y(m) <br>
**s.t.** ` ` ` ` ` ` &sum;(m&isin;S) y(m) &ge; w(S)   ` `  ` `  ` `  ` `&forall; S &isin; &Sigma; <br>
` `  ` `  ` `  ` `  ` `  ` `y(m) &ge; 1   ` `  ` `  ` `  ` `  ` `  ` `  ` `  ` `  ` `  ` `&forall; m &isin; M

[up](#approximationsalgorithmen)

---

### `WSC-LP-Rounding`

#### Frequenz
**Definition**:
Sei f die maximale Frequenz eines Elements, d.h. f := max(m &isin; M) |{S : m &isin; S}|.
_(In wievielen Mengen kommt ein Element maximal vor?)_

```
löse LP-Relaxierung von WSC-LP -> Lösungsvektor χ
wähle alle Mengen S mit χ(S) >= 1/f als Cover
```

---
>**Theorem**: `WSC-LP-Rounding` garantiert eine scharfe Gütegarantie von **f**.
---

**Beweis:**

Constraint &sum;(S:m&isin;S) x(S) &ge; 1: maximal f viele Summanden auf der linken Seite
* in feasable fraktionaler Lösung: **mind. eine** der Variablen muss Wert &le; 1/f haben.
* Aufrunden von dieser covert dieses Element m
* Algorithmus erzeugt gültiges [SetCover](#minimum-setcover)

**Güte**:

Gerundete ILP Lösungsvariablen: Entweder 0 wenn < 1/f, oder aufgerundet auf 1 wenn &ge; 1/f.
&rarr; ILP Lösung maximal um Faktor f größer als die LP Lösung (die ja untere Schranke ist)

**Beobachtung.** ([Weighted](#weightedvertexcover)) [VertexCover](#minimum-vertexcover) als SetCover hat f = 2.

**Folgerung.** `WSC-LP-Rounding` liefert für [WVC](#weightedvertexcover) eine 2-Approximation.<br>
(ist halbintegral :D)

[up](#approximationsalgorithmen)

---

### `WSC-LP-Rounding`
```
1) solve LP-relaxation from WSC-LP -> solution vector χ, objective function value z(LP).
2) for j = 1, ..., d * ln(n):
      create SetCover Cj: choose every S with probability χ(S)
3) C := Union(j)[C(j)]
4) If C is not a SetCover with a value &le; 4d*ln(n)*z(LP):
      forget C
      goto 2)
```

---
>**Theorem**: `WSC-Random-LP-Rounding` findet mit hoher Wahrscheinlichkeit ein SetCover mit Güte O(log n).
> Im Erwartungswert benötigt er dafür zwei Versuche.
---

**Beweis** in fünf Schritten:

**I:** Die erwarteten Kosten eines `C(j)` bzw. `C` sind `z(LP)` bzw. `d*ln(n)*z(LP)`.

**I.a:** Die erwarteten Kosten eines `C(j)` sind `z(LP)`:<br>
**E**[w(`C(j)`)] = &sum;(S &isin; &Sigma;) Pr[S wird gewählt] &middot; w(S) = &sum;(S &isin; &Sigma;) χ(S) &middot; w(S) = z(LP)

**I.b:** Die erwarteten Kosten von `C` sind &le; `d*ln(n)*z(LP)`:<br>
Gilt.

**II:** Die Kosten von `C` sind "mit hoher Wahrscheinlichkeit" &le; `4d*ln(n)*z(LP)`.<br>
[Markov'sche Ungleichung](https://de.wikipedia.org/wiki/Markow-Ungleichung_(Stochastik)): Pr[X &ge; t] &le; **E**[X] / t <br>
> Die Ungleichung gibt eine obere Schranke für die Wahrscheinlichkeit an, dass eine Zufallsvariable eine vorgegebene reelle Zahl überschreitet.

Wende Markov auf I.b an (t = `4d*ln(n)*z(LP)`).<br>
&rarr; Pr[w(C) &ge; `4d*ln(n)*z(LP)`] &le; `d*ln(n)*z(LP)` / (`4d*ln(n)*z(LP)`) = 1/4

**III:** Für ein beliebiges `C(j)` gilt: Jedes Element ist mit konstanter Wahrscheinlichkeit gecovert.

Betrachte Element `m` &isin; M.
Es ist in k vielen Sets enthalten.
Seien χ(1), ..., χ(k) die Werte der zugehörigen LP-Variablen.
Wir wissen &sum;(1 &le; i &le; k) χ(i) &ge; 1.<br>
&rarr; Wahrscheinlichkeit, dass m gecovert ist, ist minimal wenn alle χ(i) = 1/k.
&rarr; Pr[m ist nicht von `C(j)` gecovert] &le; (1 - 1/k)^k &le; 1/e (konstante Wahrscheinlichkeit &lt; 0.37)

_Ein m wird demnach mit Wahrscheinlichkeit > 1-(1/e) gecovert._

**IV:** Vereinigung `C` covert alle Elemente "mit hoher Wahrscheinlichkeit".

Pr[m ist nicht von `C(j)` gecovert] &le; (1/e)^(d &middot; ln(n)) &le; 1 / (4n)

> d wird so gewählt, dass 4n &le; n^d (z.B. d=2 für n &ge; 4) &rarr; 4n &le; (e^(ln(n)))^d = e^(d &middot; ln(n))

&rarr; Summe über alle m: Pr[C ist kein Cover] &le; n &middot; 1/(4n) &le; 1/4.

**V:** **Theorem** durch Kombination von II+IV.

Pr[C ist kein Cover oder teurer als `4d*ln(n)*z(LP)`] &le; <br>
Pr[C ist kein Cover] + Pr[w(C) &gt; `4d*ln(n)*z(LP)`] &le; 1/4 + 1/4 &le; 1/2

&rarr; Pr[C ist ein Cover mit Kosten maximal `4d*ln(n)*z(LP)`] &ge; 1/2.

Im Erwartungswert findet der Algorithmus eine Lösung mit Güte O(log n) nach 2 Iterationen.

[up](#approximationsalgorithmen)

---

### `WSC-DP`
`WSC-Greedy` mittels Dual-Fitting

Alternativer Beweis (mittels Dual-Fitting) für das Theorem von [`WSC-Greedy`](#wsc-greedy).

---
Duales Programm (`WSC-DP`):

**max**  ` `  ` `  ` `  &sum;(m &isin; M) y(m) <br>
**s.t.** ` ` ` ` ` ` &sum;(m&isin;S) y(m) &ge; w(S)   ` `  ` `  ` `  ` `&forall; S &isin; &Sigma; <br>
` `  ` `  ` `  ` `  ` `  ` `y(m) &ge; 1   ` `  ` `  ` `  ` `  ` `  ` `  ` `  ` `  ` `  ` `&forall; m &isin; M

---

&alpha;(m) ... (ist immernoch) relativer Preis zu dem Element m gecovert wird.
`C` ... primale Lösung mit Wert w(`C`) = &sum;(m&isin;M) &alpha;(m)

Duale Variablen **y(m)** ... selbe Indexmenge wie relative Preise **&alpha;(m)** ... <br>
&rarr; aus **&alpha;(m)** feasable duale Lösung bauen.

**Lemma**: Die Lösung mit **y(m)** := **&alpha;(m)**/Hn für alle m&isin; M ist zulässig für `WSC-DP`.

[Hn?](#harmonische-reihe)

**Beweis**

Klar ist, dass **y(m)** &ge; 0, &forall; m &isin; M.

Zu zeigen, dass Ungleichungen &sum;(m&isin;S) y(m) &ge; w(S) erfüllt sind.

Betrachte beliebige Menge **S** mit k Elementen `m1`, `m2`, ... `mk` <br>
(Wenn gleichzeitig gecovert, dann Reihenfolge egal (wie zuvor)

Direkt bevor ein Element `mi` gecovert wird, sind alle nachfolgenden Elemente (k-i+1) ungecovert.
&rarr; **S** könnte sich selber zum relativen Preis &le; w(**S**)/(k-i+1) covern.

Es wird immer mit minimalem Preis gecovert. Dementsprechend ist **y(`mi`)** &le; w(**S**)/(k-i+1) &middot; 1/Hn

Bilden wir nun die Summe über alle `mi` &isin; **S**:

w(**S**) / Hn &middot; (1 + (1/2) + (1/3) + ... + (1/k)) = w(**S**) &middot; (Hk / Hn) &le; w(**S**)

### [Primal-Dualer Algorithmus](#primal-dualer-algorithmus) für `WSC`

#### [PCSC](#primal-complementary-slackness-condition-pcsc):

&forall; S &isin; &Sigma;: &chi;(S) &ne; 0 &rarr; &sum;(m &isin; S) &gamma;(m) = w(S).

#### [DCSC](#dual-complementary-slackness-condition-dcsc):

&forall; m &isin; M: &gamma;(m) &ne; 0 &rarr; &sum;(S:m &isin; S) &chi;(S) &le; [f](#frequenz).

**Beobachtung**: Da wir (primal) nur 0/1 Lösungen erzeugen werden und f maximale Frequenz ist: DCSC ist immer erfüllt.

**Definition**: &sum;(m &isin; S) y(m) = w(S) &isin; Set S ist tight.

Algorithmische Idee:
* Erhöhe duale Variablen.
* Wähle nur tight Sets in das Cover (= primale Variable auf 1 setzen).

## Steinerbäume

[up](#approximationsalgorithmen)

---

## euklidisches TSP

