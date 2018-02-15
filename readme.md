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
* [VertexCover und SetCover](#vertexcover-und-setcover)
  * [VertexCover](#vertexcover)
  * [SetCover](#setcover)


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

### Dynamische Programmierung
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

# VertexCover und SetCover

## VertexCover

[up](#approximationsalgorithmen)

---

## SetCover

[up](#approximationsalgorithmen)

---

## Steinerbäume

[up](#approximationsalgorithmen)

---

## euklidisches TSP



Folie 16: G^k+1 -> Clique = 1, dann k+1 > k ... Widerspruch


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
