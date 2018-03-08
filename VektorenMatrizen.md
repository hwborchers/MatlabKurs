# Numerik mit MATLAB
Hans W Borchers<br>DHBW Mannheim<br>Fakultät für Maschinenbau  
Skript zum Kurs, WS 2016  
<hr>

# Numerische Lineare Algebra

Vektoren und Matritzen sind die grundlegenden Bausteine der Linearen Algebra ebenso wie der Numerik. Beim Lösen numerischer Probleme in der Analysis spielen sie eine wichtige Rolle. Das Aufstellen von langen Vektoren und großen Matrizen (auch spezieller Bauart) ist in Matlab leicht möglich.


## Vektoren

### Erzeugung und Zugriff

Vektoren sind in MATLAB -- über die geometrische Vorstellung hinausgehend -- einfach Folgen reeller (oder komplexer) Zahlen. Ein Vektor wie $x = (1,2,3,4,5)$ wird dabei mit eckigen Klammern `[` und `]` erzeugt, die Elemente (oder Komponenten) des Vektors werden durch Komma `,` oder auch nur durch ein Leerzeichen getrennt:

    >> x = [1, 2, 3, 4, 5]
    x =
         1     2     3     4     5

Genauer ist dies ein *Zeilenvektor* (eine 1x5-Matrix). Um einen *Spaltenvektor* zu erzeugen, werden die Elemente durch ein Semikolon `;` getrennt.

    >> y = [1; 2; 3; 4; 5]
    y =
         1
         2
         3
         4
         5

Vektoren einer bestimmten Länge bilden einen *Vektorraum*, d.h. sie können miteinander addiert oder mit einem *Skalar*, einer Zahl, multipliziert werden.

    >> x + x
    ans =
        2     4     6     8    10

    >> 10 * x
    ans =
        10    20    30    40    50

`x` und `y` dagegen gehören trotz gleicher Länge (von 5 Elementen) verschiedenen Vektorräumen an und können nicht addiert werden. Bei dem Versuch gibt MATLAB eine Fehlermeldung aus.

    >> x + y
    Error using  + 
    Matrix dimensions must agree.

Die Meldung "Matrix dimensions must agree." wird immer dann erscheinen, wenn eine Operation auf Vektoren oder Matrizen wegen unterschiedlicher Grössen oder Bauarten nicht möglich ist.

Durch *Transponieren* kann man Zeilen- in Spaltenvektoren umwandeln, und umgekehrt. Transponieren geschieht in MATLAB durch Anhängen des Hochkommas `'` an den Vektor.

    >>> y'
    ans =
         1     2     3     4     5

Für grössere Vektoren ist diese Art der Eingabe unpraktisch oder nicht möglich. Um zum Beispiel die Zahlen von 1 bis 20 in einen Vektor zu schreiben, benutzt man den Doppelpunkt (engl. *colon*) zwischen Anfangs- und Endwert.

    >> x = 1:20
      Columns 1 through 14
     1     2     3     4     5     6     7     8     9    10    11    12        13    14
      Columns 15 through 20
        15    16    17    18    19    20

Die Schrittweite zwischen den Elementen ist automatisch 1, man kann aber eine andere Schrittweite zwischen Anfangs- und Endwert angeben, durch einen weiteren Doppelpunkt getrennt. So wird `x = 1:2:20` alle ungeraden Zahlen ausgeben, nur 21 nicht mehr, da dieser Wert schon grösser als der Endwert ist.

Es kann auch eine Liste mit abnehmendem Zahlenverlauf erstellt werden. Dazu wird vor die Schrittweite ein Minus geschrieben. `10:-2:1` erzeugt den Vektor $(10,8,6,4,2)$.

Eine Folge `0:0.1:pi` bricht schon bei 3.1 ab. Für viele Anwendungen, zum Beispiel beim Plotten, möchte man aber, dass der Endwert wirklich erreicht wird. Das ermöglicht die `linspace` Funktion.

    >> x = linspace(0, pi, 10)
    x =
      Columns 1 through 8
             0    0.3491    0.6981    1.0472    1.3963    1.7453    2.0944    2.4435
      Columns 9 through 10
        2.7925    3.1416

Der Aufruf `linspace(a, b, n)` erzeugt einen Vektor von `a` bis `b` in gleichmässigem Abstand mit genau der Länge `n`.

**Aufgabe**: Wie kann man einen Nullvektor $(0, ..., 0)$ oder einen Vektor $(1, ..., 1)$ der Länge `n` erzeugen?

Die eckigen Klammern `[` und `]` dienen auch dazu, Vektoren aneinander zu hängen, das heisst aus Vektoren $(x_1, ..., x_n)$ und $(y_1, ..., y_m)$ einen neuen Vektor $(x_1, ..., x_n, y_1, ..., y_m)$ zu erzeugen.

    >> x = [1, 2, 3, 4];
    >> [x, x.^2, x.^3]
    ans =
         1     2     3     4     1     4     9    16     1     8    27    64

Entsprechend kann man zwei Spaltenvektoren `x` und `y` durch Benutzung des Semikolons in eckigen Klammern zusammenfügen: `[x; y]`.

Wie kann man den Wert des `i`-ten Elementes eines Vektors `x` benutzen oder sogar verändern. MATLAB benutzt dazu die runden Klammern `(` und `)`, ähnlich wie beim Aufruf einer Funktion. Mit `x(5)` wird das 5-te Element des Vektors gelesen, mit `x(5) = 10` wird diesem Element der Wert 10 zugewiesen.

WICHTIG: Vektoren und Matrizen sind in MATLAB *1-basiert*, das heisst das erste Element hat den Index 1, das zweite 2, usw. In den meisten Programmiersprachen wird dagegen das erste Element eines Vektors mit dem Index 0 erreicht (C, VBA, Python, ...), das $n$-te mit dem Index $n-1$.

Im folgenden Beispiel nutzen wir die Funktion `primes`, welche bei einem Aufruf `primes(n)` alle Primzahlen kleiner oder gleich `n` als Vektor zurückgibt. (Informieren Sie sich auf Wikipedia über Primzahlen.)

    >> P = Primes(100);
    >> P(1)                    % 2 ist die erste Primzahl

Um z.B. die 5-te bis 10-te Primzahl zu sehen, werden die Indizes 5:10 mit *colon*-Notation dem Vektor übergeben.

    >> P(5:10)
    ans =
        11    13    17    19    23    29

Wenn man nicht weiss, wie lang ein Vektor ist, kann man sehr einfach mit dem Schlüsselwort `end` auf das letzte Element zugreifen, `x(end)`, statt das umständlichere `x(length(x))` benutzen zu müssen.

**Aufgabe**: Wie viele Primzahlen gibt es bis 100, wie viele bis 10000? Welches ist die grösste Primzahl unterhalb einer Million. Wie gross ist die 1000-te Primzahl?


### Logische Vergleiche auf Vektoren

Auch Vergleichsoperatoren wie `>`, `>=`,`<`,`<=`,`==` oder `~=` sind vektorisiert. Ist `x` ein Vektor, wird etwa mit `x >= 5` ein Vektor gleicher Länge wie `x` zurückgegeben, der 1 enthält an den Stellen, an denen x grösser als 5 ist, und 0 an den anderen Stellen.

    >> x = [0, 1, 2, -3, 4, -5, -6, 7, -8, 9];
    >> x >= 0
    ans =
         1     1     1     0     1     0     0     1     0     1

Die Funktion `find` findet in einem Vektor die Indizes aller Elemente die nicht 0 sind. Da 0 auch der logische Wert für "falsch" ist, lässt sich das gut verbinden mit solchen Vergleichen auf Vektoren. 

    >> inds = find(x >= 0)
    inds =
         1     2     3     5     8    10
    >> sum(x >= 0)
    ans =
         6

Da `sum` alle Einsen aufsummiert, wird damit die Anzahl der Elemente bestimmt, welche die Bedingung erfüllen, in diesem Fall `x >= 0`.

### Funktionen auf Vektoren

Neben den Vektoroperationen bietet MATLAB verschiedene Funktionen an, die auf dem ganzen Vektor arbeiten. Neben `length(x)`, das die Länge, also Anzahl der Elemente, des Vektors zurückgibt, gibt es `sum`, `prod`, `min`, `max`, und `mean`, in offensichtlichen Bedeutungen. So berechnet `sum(x)` bzw. `prod(x)` die Summe bzw. das Produkt aller Elemente des Vektors `x`, `min(x)` bzw. `max(x)` das Minimum bzw. Maximum aller Elemente von `x`, und `mean(x)` den Mittelwert.

**Aufgabe**: Berechne die Summe aller Zahlen von 1 bis 100. Stimmt das Ergebnis mit der Ihnen bekannten Formel überein? Wie gross ist das Produkt aller Zahlen von 1 bis 100?

`sort(x)` gibt den aufsteigend sortierten Vektor zurück (ohne `x` selbst zu verändern). Um den Vektor absteigend zu sortieren, benutzt man das Schlüsselwort 'descend'.

    >> x = [4, 1, 3, 9, 7, 2, 8, 5, 6];
    >> sort(x)
    ans =
         1     2     3     4     5     6     7     8     9
    >> x
    x =
         4     1     3     9     7     2     8     5     6
    >> sort(x, 'descend')
    ans =
         9     8     7     6     5     4     3     2     1

**Aufgabe**: (Unter Benutzung der MATLAB Dokumentation) Was berechnet die Funktion `median`, was tut die Funktion `unique`? Warum gibt `issorted(x)` den Wert 0 zurück? 

In MATLAB sind alle elementaren mathemathischen Funktionen (trigonometrische, Exponential-, Rundungsfunktionen, etc.) *vektorisiert*, das heisst angewandt auf einen Vektor werden sie auf jedes Element des Vektors angewendet und ein Vektor der berechneten Funktionswerte zurückgegeben.

    >> x = [0, pi/2, pi, 3*pi/2, 2*pi];
    >> sin(x)
    ans =
         0    1.0000    0.0000   -1.0000   -0.0000

Das ist für viele numerische Berechnungen sehr praktisch, muss aber immer beachtet werden, insbesondere beim Schreiben eigener Funktionen.


### Elementweise Operationen

In Matlab bezeichnet der Stern `*` *immer* die *Matrix*multiplikation. Solange in einem Ausdruck `x*y` eine der beiden Variablen (oder beide) ein Skalar ist, also eine einfache Zahl, spielt das keine Rolle und `x*y` verhält sich wie eine Matrix multipliziert mit einer Zahl. Wenn aber beides echte Matrizen sind, Zeilen- oder Spaltenanzahl grösser als 1, dann müssen auch die Voraussetzungen für eine Matrizenmultiplikation erfüllt sein. "Inner matrix dimensions must agree" ist sonst eine übliche Fehlermeldung.

    >> x = [1, 2, 3, 4, 5];
    >> x * x
    Error using  * 
    Inner matrix dimensions must agree. 

Zeilenvektor mal Spaltenvektor erfüllt diese Bedingung und wird daher von MATLAB korrekt ausgeführt.

    >> x = [1, 2, 3, 4, 5];  % Zeilenvektor
    >> y = [1; 2; 3; 4; 5];  % Spaltenvektor
    >> x * y
    ans =
        55

Dasselbe gilt für Division mit `/` und Potenzierung mit `^`, beide werden als Matrixoperationen aufgefasst. (Insbesondere `x^2` sollte ja mit `x * x` übereinstimmen.)

Dennoch ist die *punktweise*, *elementweise* Multiplikation zweier Vektoren (oder Matrizen) in der numerischen Mathematik oder Datenanalyse ein ständig benötigte Operation. MATLAB hat für diese Operationen eine eigene Schreibweise eingeführt, die elementweise Multiplikation `.*`, Division `./` und Potenzierung mit einer Zahl `.^`. Der Punkt soll dabei andeuten, dass diese Operation eben 'punktweise' ausgeführt werden soll.

    >> x = [1, 2, 3, 4, 5];
    >> x .* x
    ans =
         1     4     9    16    25
    >> x ./ x
    ans =
         1     1     1     1     1
    >> x.^0.5
    ans =
        1.0000    1.4142    1.7321    2.0000    2.2361

Anmerkung: Leider ergibt `x .*x` eine (unberechtigte) Fehlermeldung. Es ist in MATLAB durchaus vorteilhaft, Operatoren immer mit Leerzeichen zu umgeben.

**Aufgabe**: Berechne $\sum_{n=1}^N 1/n^2$ für $N=1000$ und für $N=10^6$. Wie gross ist der Fehler in beiden Fällen, wenn $\pi^2/6$ der tatsächliche Grenzwert dieser Reihe ist?


### Geometrische Anwendungen

Vektoren mit zwei oder drei Elementen können auch als Vektoren im 2- oder 3-dimensionalen Raum aufgefasst werden. Addition von Vektoren bedeutet dann das Aneinanderfügen zweier solcher geometrischer Vektoren.

Die Norm eines Vektors ist mathematisch definiert als $|(x_1, ..., x_n)| = \sqrt{x_1^2+...+x_n^2}$ und wird in MATLAB realisiert mit der Funktion `norm` und berechnet die geometrische Länge des Vektors. Der Vektor `x = [1, 1, 1]` beschreibt die Raumdiagonale im Würfel der Seitenlänge 1, und hat selbst die Länge $\sqrt{3}$:

    >> x = [1, 2, 3];
    >> norm(x)
    ans =
        1.7321

Die Funktion `dot` berechnet das Skalarprodukt zweier Vektoren (beliebiger Länge), und `cross` das Kreuzprodukt zweier Vektoren im 3-dimensionalen Raum.

    >> x = [1, 0, 0]; y = [0, 1, 0];
    >> dot(x, y)
    ans =
         0
    >> cross(x, y)
    ans =
         0     0     1

**Aufgabe**: Entspricht das Ihren Erwartungen bzw. der Rechte-Hand-Regel für das Kreuzprodukt?

Der Winkel $\phi$ zwischen zwei Vektoren $v$ und $w$ wird bestimmt durch den Ausdruck $\cos \phi = \frac{v \cdot w}{|v| |w|}$. Eine Funktion, welche diesen Winkel berechnet, kann man folgendermassen definieren:

    >> winkel = @(v, w) acos(dot(v, w) / norm(v) / norm(w));
    >> phi = winkel([1, 1, 1], [1, 1, 0]);
    >> rad2deg(phi)
    ans =
       35.2644

Der Winkel zwischen der Raumdiagonale und der (x, y)-Ebene beträgt etwa 35 Grad.

**Aufgabe**: Die Projektion eines Vektors $v$ auf einen anderen Vektor $w$ wird berechnet zu $vw = \frac{v \cdot w}{|w|^2} w$. Schreibe eine Funktion in MATLAB, die diese Projektion berechnet. Bestimme die Projektion der Flächendiagonale `[1, 1, 0]` auf die Raumdiagonale und umgekehrt die Projektion der Raumdiagonale auf die Flächendiagonale.


## Matrizen

Ein Matrix ist, vereinfacht gesagt, ein rechteckiges Schema von Zahlen. Eine Matrix mit $m$ Zeilen und $n$ Spalten wird auch als $(m \times n)$-Matrix bezeichnet. Ihre grosse Bedeutung haben Matrizen als lineare Abbildungen auf Vektorräumen und als Speicher von Daten in der Datenanalyse.

### Erzeugung von Matrizen

Kleinere Matrizen können ähnlich wie Vektoren unter Benutzung der Klammern `[` und `]` eingegeben werden. Beispielsweise eine Matrix wie
$$
  A =
  \begin{pmatrix}
  1 & 2 & 3\\
  4 & 5 & 6\\
  7 & 8 & 9
  \end{pmatrix}
$$
wird in MATLAB eingetippt mit dem Semikolon `;` als Trennzeichen zwischen den Zeilen und dem Komma `,` (oder nur Leerzeichen) innerhalb der Zeile.

    >> A = [1, 2, 3; 4, 5, 6; 7, 8, 9]
    A =
         1     2     3
         4     5     6
         7     8     9

Sind bei der Eingabe nicht alle Zeilen und Spalten gleich lang, wird eine Fehlermeldung erscheinen: "Dimensions of matrices being concatenated are not consistent."

Um die Matrix `A` auf einen Spalten(!)vektor (warum?) anzuwenden, wird die Matrizenmultiplikation `*` genutzt. Das Ergebnis ist dann wieder ein Spaltenvektor.

    >> x = [1; 1; 1]
    x =
         1
         1
         1
    >> y = A * x
    x =
         6
        15
        24

In MATLAB gibt es Standardmatrizen, die öfters auftauchen und somit die Eingabe insbesondere grosser Matrizen vereinfachen. Dies sind die Nullmatrix (aus lauter Nullen bestehend), eine Matrix aus lauter Einsen, oder die Einheitsmatrix mit Einsen in der Diagonale und sonst nur Nullen.

Die Aufrufe dafür sind `zeros(m,n)`, `ones(m,n)`, bzw. `eye(m, n)`, wobei jeweils `m` die Anzahl der Zeilen und `n` die Anzahl der Spalten ist; `eye(n)` ist die quadratische Einheitsmatrix mit `n` Zeilen und Spalten.

`A=zeros(3,5)` bedeutet, dass A eine (3x5)-Matrix mit Nullen ist. Man kann auch einen Nullvektor auf diese Art erstellen. Tippt man zum Beispiel `ones(3,1)` ein, erhält man einen Spaltenvektor aus Einsen, mit `ones(1,3)` einen Zeilenvektor.

`eye(4)` ergibt eine Einheitsmatrix in der angegebenen Größe. Soll die Matrix auf der Diagonalen andere Werte als die Einheitsmatrix haben, kann man `diag([4,6,2,8])` eintippen. Nun sind die Werte auf der Diagonalen 4, 6, 2 und 8. Umgekehrt liefert der `diag` Befehl auch die Diagonalelemente einer bestehenden Matrix: `diag(A)`.

Die Matrix `A` kann auch schneller generiert werden. Dazu wird der Vektor `1:9` mit dem Befehl `reshape` neu formatiert, indem eine neue Zeilen- und Spaltenanzahl vorgegeben wird.

    >> A = reshape(1:9, 3, 3)
    A =
         1     4     7
         2     5     8
         3     6     9

Es ist typisch für MATLAB, dass Matrizen spaltenweise aufgefüllt und benutzt werden. Um die korrekte Matrix `A` zu erzeugen, könnte man `A` transponieren, `A = A'`, mit dem Hochkomma als Operatorzeichen.

    >> A = reshape(1:9, 3, 3)'
    A =
         1     2     3
         4     5     6
         7     8     9

Eine weitere Möglichkeit, besondere Matrizen zu erzeugen, wird durch den Befehl `repmat` bereitgestellt ("repeat matrix").Dieser Befehl wiederholt eine Matrix so oft in der Zeile und der Spalte, wie angegeben ist.

    >> B = [1 2; 3 4];
    >> repmat(B, 2, 3)
    ans =
         1     2     1     2     1     2
         3     4     3     4     3     4
         1     2     1     2     1     2
         3     4     3     4     3     4

Matrizen können mithilfe der Klammern `[` und `]` erweitert oder aneinander gefügt werden. Durch `[A, B]` werden `A` und `B` nebeneinander gesetzt (Anzahl ihrer Zeilen muss gleich sein), durch `[A; B]` werden sie untereinander gesetzt (Anzahl ihrer Spalten muss gleich sein).


### Zufallszahlen

Manchmal braucht man eine, mehrere oder viele zufällige Zahlen, als Beispieldaten oder zur Simulation von Prozessen. Mit dem Computer können keine wirklichen Zufallszahlen generiert werden. Stattdessen werden sog. *Pseudo-Zufallszahlen* durch teilweise komplizierte math. Prozeduren errechnet.

In MATLAB werden Zufallszahlen mit den Befehlen `rand`, `randn`, oder `randi` erzeugt (engl. *random*). Mit `rand(m, n)` entsteht eine (mxn)-Matrix von Zufallszahlen, also gleichmässig verteilte Gleitkommazahlen zwischen 0 und 1.

    >> rand(3, 2)
    ans =
        0.8147    0.9134
        0.9058    0.6324
        0.1270    0.0975

**Aufgabe**: Generiere 100 gleichverteilte Zufallszahlen im Intervall von -1 bis 1. Ist der Mittelwert nahe bei 0?

Nach jedem MATLAB Neustart werden die gleichen Zufallszahlen erzeugt. Um das zu verhindern, zuvor den Befehl `rng('shuffle')` an den Zufallszahlen-Generator schicken.

Um die Zufallszahlen auch ohne Neustart wiederholbar zu machen, kann man dem Generator eine Art Kennnummer mitgeben, sodass anschliessend immer die gleichen Zufallszahlen benutzt werden. Das ist oft wichtig für die Vergleichbarkeit der Resultate.

Statt Gleitkommazahlen können auch zufällige ganze Zahlen in eine Matrix geschrieben werden. Dies geht über den Befehl `randi`. Zuerst steht, in welchem Bereich $1, ..., n$ die ganzen Zahlen liegen sollen. Danach kommt die Größe der Matrix. `A=randi(10, 3, 2)` gibt somit eine 3x2-Matrix mit Werten von 1 bis 10 aus.

    >> rng(1001)            % Benutze eine persönliche Kennzahl
    >> Z = randi(10, 3, 2)
    Z =
         4     5
         3     1
         2     2

Wir können auch eine 3x2-Matrix mit zufälligen ganzen Zahlen zwischen -5 und 5 anfordern: `C=randi([-5, 5], 4, 5)`.

**Aufgabe**: Erzeuge einen Vektor mit 1000 Würfen eines korrekten Würfels (mit sechs Seiten). Wie gross ist der Mittelwert aller Würfe? Bestimme, wie oft eine 1 bzw. eine 6 gewürfelt wurde. Nach wievielen Würfen wurde 10 Mal die 3 gewürfelt?

Interessant ist noch der Befehl `randperm`, der eine Permutation von Zahlen $1, ..., n$, bzw. eine zufällige Auswahl dieser Zahlen erzeugt.

    >> randperm(10)
    ans =
         6     9     2     1    10     7     4     3     5     8
    >> randperm(10, 5)
    ans =
         6     1     8     7     9

**Aufgabe**: Wähle zufällig 10 Primzahlen aus der Menge der Primzahlen unter 1000 aus. Wie könnte man realisieren, dass auch Mehrfachziehungen einer Zahl möglich sind?

Der Befehl `randn` erzeugt *normalverteilte* Zufallszahlen (s. Gaußsche Normalverteilung) mit Mittelwert 0 und Standardabweichung 1. Nützlich zum Beispiel um die Schwankung von Messwerten um einen Mittelwert zu simulieren.


### Index-Yoga

Ähnlich wie für Vektoren geschieht das Auslesen von Elementen aus einer Matrix durch Klammern, allerdings muss hier sowohl die gewünschte Zeile wie Spalte angegeben werden.

    >> M = magic(4)     % ein magisches Quadrat
    M =
        16     2     3    13
         5    11    10     8
         9     7     6    12
         4    14    15     1
    >> M(2, 3)
    ans =
        10

Entsprechend können auch ganze Untermatrizen oder Zeilen oder Spalten ausgewählt werden. Um etwa die Elemente der 2-ten und 3-ten Zeile und der Spalten 2 bis 4 auszuwählen, schreibt man `M(2:3, 2:4)`.

    >> M(2:3, 2:4)
    ans =
        11    10     8
         7     6    12

Man kann auch ganze Zeilen oder Spalten aus Matrizen auswählen.
Dies kann man durch Eintippen eines Doppelpunktes `:` erreichen, ohne Anfangs- und Endwert. So bedeutet `m = M(:, 1)`, dass die gesamte erste Spalte ausgewählt wird.

    >> M(3, :)                  % Zeile 3, auch M(3, 1:end)
    ans =
         9     7     6    12
    >> M(:, 2)                  % Spalte 2, auch M(1:end, 2)
    ans =
         2
        11
         7
        14

Wie man sieht, ist das Ergebnis ein Zeilen- oder Spaltenvektor, je nachdem ob man eine Zeile oder Spalte ausgewählt hat.

Neben einzelnen Elementen einer Matrix, z.B. durch `M(2, 3) = 20`, können auch ganze Zeilen, spalten oder Untermatrizen geändert werden. Voraussetzung ist aber, dass die Dimensionen (Zeilen-, Spaltenanzahl) des zu ersetzenden Bereiches und des neuen Bereiches übereinstimmen.

    >> M(2, 4) = 2 * M(2, 4);   % verdoppele M(2, 4)
    >> M(2:3, 2:3) = 0
    M =
        16     2     3    13
         5     0     0    16
         9     0     0    12
         4    14    15     1

Ausnahme ist offensichtlich, dass die Zuweisung einer Zahl an einen ganzen Bereich diesen mit nur einen Zahl überschreibt.

**Aufgabe**: Addiere die erste Zeile zur vierten und dividiere anschliessend die vierte Zeile durch ihr erstes Element.

Durch Angabe nur eines Indexes wird eine Matrix wie ein Vektor addressiert, wieder spaltenweise, daher ergibt `M(5)` den Wert 2, das erste Element der zweiten Spalte. Wird nur `:` eingegeben, erhält man die ganze Matrix als  Spaltenvektor.

    >> M(12:15)
    ans =
        15
        13
        16
        12

Man vergleiche die Ergebnisse von `length(M)` und `length(M(:)`.

Um eine Zeile (oder Spalte) einer Matrix zu löschen, weist man ihr einen leeren Vektor zu, in MATLAB symbolisiert durch leere Klammern: `[]`.

    >> M = [1, 2, 3; 4, 5, 6; 7, 8, 9];
    >> M(2, :) = []
    M =
         1     2     3
         7     8     9

Das Einfügen einer Zeile oder Spalte zwischen andere Zeilen oder Spalten ist nicht ganz so einfach und muss in zwei Schritten erfolgen (wie?).


### Rechnen mit Matrizen

Matrizen gleicher Dimension können elementweise addiert und subtrahiert oder mit einem Skalar multipliziert werden. Alle $(m, n)$-Matrizen zusammen bilden also einen Vektorraum der Dimension $m n$ (warum) über den rellen (oder komplexen) Zahlen.

Matrizen der Dimensionen $m \times n$ und $n \times k$ können im Sinne der Matrizenmultiplikation miteinander multipliziert werden und bilden dann eine neue Matrix der Dimension $m \times k$. In MATLAB bezeichnet der Stern `*` immer diese Matrixmultiplikation.

Entsprechend bedeutet `A / B` die Multiplikation der Matrix `A` mit dem Inversen der Matrix `B`, und `A^n` die n-fache Multiplikation der Matrix `A` mit sich selbst. Die Inverse von `A` wird mit `A^-1` oder `inv(A)` berechnet, wobei möglicherweise `inv(A)` vorzuziehen ist.

Und wie für Vektoren gibt es für Matrizen die elementweise Multiplikation `.*`, Division `./` und Potenzierung `.^`. Für `A .* B` und `A ./ B` müssen `A` und `B` gleiche Dimension haben. Im Falle quadratischer Matrizen kann man das direkt miteinander vergleichen.

    >> M = magic(4);            % siehe oben
    >> M * M
    ans =
       345   257   281   273
       257   313   305   281
       281   305   313   257
       273   281   257   345
    >> M .* M
    ans =
       256     4     9   169
        25   121   100    64
        81    49    36   144
        16   196   225     1


### Funktionen auf Matrizen

`size(A)` liefert die Anzahl von Zeilen und Spalten der Matrix `A` als einen Vektor mit zwei Elementen; `size(A, 1)` nur die Zeilen- und 
`size(A, 2)` nur die Spaltenanzahl. Für eine Zahl `a` gibt `size(a)` den Vektor `[1, 1]` zurück, MATLAB betrachtet einzelne Zahlen als ($1 \times 1$)-Matrizen.

    >> A = ones(3, 2);
    >> size(A)
    ans =
         3     2
    >> size(1.0)
    ans =
         1     1

`inv(A)` zum Invertieren einer Matrix `A` haben wir schon kennengelernt. Die Determinante ist eine andere nützliche Funktion und wird aufgerufen mit `det(A)`. `rank(A)` bestimmt den Rang der Matrix, die maximale Zahl linear unabhängiger Zeilen oder Spalten.

    >> M = magic(4);
    >> d = det(M), r = rank(M)
    d =
       5.1337e-13
    r =
         3
    >> inv(M)
    Warning: Matrix is close to singular or badly scaled. Results may be inaccurate.
    RCOND =  4.625929e-18. 
    ans =
       1.0e+15 *
       -0.2649   -0.7948    0.7948    0.2649
       -0.7948   -2.3843    2.3843    0.7948
        0.7948    2.3843   -2.3843   -0.7948
        0.2649    0.7948   -0.7948   -0.2649

Die von Vektoren bekannten Funktionen `sum`, `prod`, `min`, `max` und `mean` funktionieren auch für Matrizen, allerdings in etwas überraschender Weise.

    >> M = magic(4);
    >> sum(M)
    ans =
        34    34    34    34

Diese Funktionen berechnen ihre Werte spaltenweise und nicht über die ganze Matrix. `M` ist ein magisches Quadrat, die Summe aller Spalten ist gleich (und muss deshalb 34 sein). Die Summe der Zeilen könnte man erhalten, indem `sum` auf die transponierte Matrix `M'`angewandt wird, aber `sum(M, 2)` funktioniert auch.

    >> sum(M, 2)
    ans =
        34
        34
        34
        34

**Aufgabe**: Ist auch die Summe der Elemente in der Diagonalen gleich 34? Und wie sieht es mit der Nebendiagonalen aus?

**Aufgabe**: Wie kann man die Summe, das Produkt, etc. aller Elemente der Matrix berechnen?


## Lineare Gleichungssysteme (LGS)

Ein lineares Gleichungssystem in Matrixschreibweise lautet $A\,x = b$. $A$ ist eine ($m \times n$)-Matrix und $b$ ein Vektor der Länge $m$. Gesucht wird der Vektor $x$ der Länge $n$, der diese Gleichung erfüllt. Im Allgemeinen wird angenommen, dass $m=n$ gilt und $A$ eine Matrix von maximalen Rang (d.h., invertierbar) ist. Nur dann dann gibt es genau eine Lösung des Gleichungssystems.

Eine bekannte und effiziente Methode, lineare Gleichungssysteme zu lösen, ist das *Gaußsche* (Eliminations-) *Verfahren*, siehe [Papula, "Mathematik für Ingenieure und Naturwissenschaftler", Band 1, Abschnitt I.5]. Dabei wird die Matrix $A$ durch Zeilenoperationen schrittweise in die Einheitsmatrix überführt. Anwendung derselben Schritte auf den Vektor $b$ ergibt dann die Lösung.

**Beispiel**: Wir wollen das folgende lineare Gleichungssystem lösen:
$$
  \begin{pmatrix}
  1 & 1/2 & 1/3\\
  1/2 & 1/3 & 1/4\\
  1/3 & 1/4 & 1/5
  \end{pmatrix}
  \begin{pmatrix}
  x_1\\
  x_2\\
  x_3
  \end{pmatrix} =
  \begin{pmatrix}
  1\\
  -1\\
  1
  \end{pmatrix}
$$

Wir erstellen die Matrizen `A` und `b` und daraus die 'erweiterte' Matrix `B`:
$$
  B = \begin{pmatrix}
  1 & 1/2 & 1/3 & 1\\
  1/2 & 1/3 & 1/4 & -1\\
  1/3 & 1/4 & 1/5 & 1
  \end{pmatrix}
$$

    >> A = [  1, 1/2, 1/3; 1/2, 1/3, 1/4; 1/3, 1/4, 1/5];
    >> b = [1; -1; 1];
    >> B = [A, b]
    B =
        1.0000    0.5000    0.3333    1.0000
        0.5000    0.3333    0.2500   -1.0000
        0.3333    0.2500    0.2000    1.0000

Im Gaußschen Verfahren sind nur reine Zeilenoperationen erlaubt. Um zum Beispiel das Element `B(3, 1)` zu Null zu machen, benutzen wir die erste Zeile:

    >> B(3, :) = B(3, :) - B(3, 1) * B(1, :)
    B =
        1.0000    0.5000    0.3333    1.0000
        0.5000    0.3333    0.2500   -1.0000
             0    0.0833    0.0889    0.6667

**Aufgabe**: Verwende nur Zeilenoperationen, um die Untermatrix der ersten drei Spalten von `B` in die Einheitsmatrix `eye(3)` umzuwandeln. Was steht dann in der vierten Spalte? (Arbeiten Sie im Editor, wir brauchen diese Folge von Befehlen gleich noch einmal.)

MATLAB stellt das Gaußsche Verfahren in dem Operator `\` ('Gegenschrägstrich', engl. *backslash*) zur Verfügung. Durch den Befehl `x = A \ b` wird dem Vektor `x` die Lösung des Gleichungssystems $A \, x = b$ zugewiesen, unter Einsatz eines optimierten Gaußschen Verfahrens.

Im Prinzip könnte man die Gleichungen auch durch `x = inv(A) * b` auflösen, indem die Gleichung $A \, x = b$ von links mit der Inversen von `A` multipliziert wird, allerdings ist das Verfahren mit `\` i.A. schneller und vor allem robuster (gegen numerische Ungenauigkeiten).

**Aufgabe**: Wende die gleichen Schritte und Zeilenumformungen wie in der letzten Aufgabe an auf die Matrix

$$
  B = \begin{pmatrix}
  1 & 1/2 & 1/3   & 1 & 0 & 0\\
  1/2 & 1/3 & 1/4 & 0 & 1 & 0\\
  1/3 & 1/4 & 1/5 & 0 & 0 & 1
  \end{pmatrix}
$$
Welche Matrix steht dann in den letzten drei Spalten der umgewandelten Matrix `B`?
