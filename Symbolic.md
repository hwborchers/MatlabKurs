---
title: "Symbolic Toolbox"
author: "Hans W Borchers"
date: "10/29/2016"
output:
  html_document: 
    css: NumerikSkript.css
    keep_md: yes
    theme: cerulean
    toc: yes
  pdf_document: 
    keep_tex: yes
---

## Symbolisches Rechnen mit MATLAB

MATLAB hauptsächlich für die Verarbeitung von numerischen Daten gedacht, es kann in MATLAB auch symbolisch gerechnet werden, zum Beispiel exaktes Differenzieren oder Integrieren. Voraussetzung ist, dass die *Symbolic Toolbox* im Suchpfad von MATLAB vorhanden ist.

Normalerweise sucht MATLAB beim Verarbeiten einer Variablen, etwa `x`, nach einem gespeicherten numerischen Wert für diese Variable. Wir müssen MATLAB explizit sagen, dass `x` eine symbolische Variable ist. Das geschieht mit dem Befehl `syms x` bzw. `syms x y ...`.

    >> syms x
    >> y = cos(x)

Im Workspace Window erscheinen `x` und `y` jetzt als '1x1 sym', also als symbolische Variablen.

Dabei ist es ein Unterschied, ob wir `y = cos(x)` oder `z(x) = cos(x)` schreiben. Das erste erzeugt y als einen symbolischen Ausdruck, das zweite erzeugt z als symbolische Funktion. Beide erscheinen nun auch im Workspace, `z` als '1x1 symfun'.

Man kann für x wieder einen numerischen Wert einsetzen und sich den Kosinus ausgeben lassen, das geschieht mit `subs(y, x, pi)` bzw. `z(pi)`, durch "Substitution" erhält man `y` und `z` an der Stelle `x = pi`.

    >> z(pi/2)
    ans =
        0

Wenn ein Ausdruck symbolisch nicht bekannt ist, z. B. `z(1.0)`, dann wird er numerisch nur ausgewertet, wenn darauf der Befehl `double` angewendet wird.

    >> z1 = z(1.0)
    z1 =
        cos(1)
    >> double(z1)
    ans = 
        0.5403

Wie bereits erwähnt, kann man das symbolische Rechnen für Differenzieren und Integrieren nutzen. Zum Differenzieren gibt es den Befehl `diff`,

    >> diff(cos(x), x)      % oder diff(y, x)
    ans =
    -sin(x)

wobei das zweite Argument die Variable nennt, nach der differenziert werden soll. Man kann auch noch die Höhe der Ableitung als weiteres Argument angeben, die dritte Ableitung nach `x` also bestimmt mit

    >> diff(cos(x), x, 3)
    ans =
    sin(x)

Analog dazu wird zum Integrieren von `log(x)` der Befehl `int` benutzt, unter Angabe der Integrationsvariablen.

    >> int(log(x), x)
    ans =
    x*(log(x) - 1)

Die Symbolic Toolbox liefert also nur die Stammfunktion, die freie Integrationskonstante müssen wir uns dazudenken. Ein bestimmtes Integral erhält man durch die zusätzliche Angabe der Integrationsgrenzen.

    >> int(sin(x)^2, x, 0, pi)
    ans =
    pi/2

(Hinweis: Siehe die obige Berechnung des Schwerpunktes: Die unbekannte Zahl 0.3927 ist offenbar $\pi/8$.)

Auch Stammfunktionen komplexerer Integrale lassen sich mit `int` herleiten, zum Beispiel $\int \frac{log(x)}{(2x+5)^3}\,\mathrm{d}x$. Und mit `pretty` kann eine etwas besser lesbare Anzeige des Ergebnisses erzielt werden.

    >> int(log(x) / (2*x + 5)^3)
    ans =
    log(x)/100 - log(x + 5/2)/100 + (x/10 - log(x)/4 + 1/4)/(2*x + 5)^2
    >> pretty(ans)
                /     5 \    x   log(x)   1
             log| x + - |   -- - ------ + -
    log(x)      \     2 /   10      4     4
    ------ - ------------ + ---------------
      100         100                   2
                               (2 x + 5)

Auch unendliche Integrationsbereiche oder Funktionen mit Polstellen am Rand sind möglich. Soll `1/x^2` von 1 bis Unendlich integriert werden, tippt man: `int(1/x^2, 1, Inf)` mit der Antwort `ans = 1`.

**Aufgabe**: Wie gross ist $\int_{-\infty}^{\infty} e^{-x^2}\,\mathrm{d}x$?

Ebenso einfach ist es, sich die *Taylorentwicklung* (das Taylorpolynom) einer (hinreichend oft differenzierbaren) Funktion $f$ in einem Punkt $a$ anzeigen zu lassen. Als Beispiel das Taylorpolynom der Funktion $\log(1+x)$ im Punkt $x = 0$.

    >> taylor(log(1+x), x, 0)
    ans = 
    x^5/5 - x^4/4 + x^3/3 - x^2/2 + x

Bis zur fünften Ordnung ist der 'Default'. Um auch höhere Glieder des Taylorpolynoms zu erhalten, müsste man eingeben:
`taylor(log(1+x), x, 0, 'Order', 8)`

    >> T = taylor(log(1+x), x, 0, 'Order', 8);
    >> pretty(T)
     7    6    5    4    3    2
    x    x    x    x    x    x
    -- - -- + -- - -- + -- - -- + x
     7    6    5    4    3    2

**Aufgabe**: Plotten Sie die Funktion $\log(1+x)$ in einer Umgebung von $x = 0$ und zusätzlich in rot das Taylorpolynom.

Tatsächlich können mit `ezplot` auch symbolische Funktionen und Ausdrücke geplottet werden.

    >> g = 1/(5 + 4*cos(x));
    >> ezplot(T, [0, 6*pi])

`coeffs(T)` liefert die Koeffizienten des Polynoms als Vektor, allerdings in umgekehrter Reihenfolge, als es von MATLAB erwartet wird. (Man kann die Reihenfolge der Elemente eines Vektors 'umdrehen' mit `x(end:-1:1).)

Symbolische Ausdrücke können vereinfacht werden mit dem `simplify` Befehl.

    >> simplify((x^2-5*x+6)/(x-2))
    ans =
    x - 3

Der `factor` Befehl bestimmt die Faktoren eines Polynoms.

    >> factor(x^2-5*x+6)
    ans =
    [ x - 2, x - 3]

Umgekehrt kann ein Produkt auch ausmultipliziert werden, mit `expand`.

    >> expand((x+2)*(x-7))
    ans =
    x^2 - 5*x - 14

Symbolische Bestimmung von Nullestellen ist möglich mit dem `solve` Befehl.

    >> solve(x^2 - 5*x - 14, x)
    ans =
        -2
        7

Die symbolische Toolbox kennt viel mehr Funktion als ein Anwender, daher kann es auch Überraschungen beim Lösen von Gleichungen geben. Ein Beispiel von früher: $x*e^x = 1$.

    >> omega = solve(x * exp(x) - 1, x)
    omega =
    lambertw(0, 1)

`lambertw` ist die Umkehrfunktion von $x \to x\,e^x$, daher ist das Ergebnis richtig, obwohl vielleicht nicht gleich verständlich. Den genauen Wert erhält man wieder mit `double`.

    >> format long
    >> double(omega)
    ans =
       0.567143290409784

Intern rechnet die symbolische Toolbox mit sehr viel höherer Genauigkeit. Um sich mehr Dezimalstellen eines Wertes anzeigen zu lassen, benutze den `vpa` Befehl (*variable-precision arithmetic*). Das zweite Argument gibt die Anzahl der gewünschten Ziffern an:

    >> vpa(omega, 32)
    ans =
    0.56714329040978387299996866221036

    >> vpa(pi, 64)
    ans =
    3.141592653589793238462643383279502884197169399375105820974944592

Auch Grenzwerte von Funktionen oder Folgen können exakt bestimmt werden, etwa $\lim_{x\to0} \frac{sin(x)}{x}$

    >> limit(sin(x)/x, x, 0)
    ans =
    1

Für Folgen wie die der $n$-ten Wurzel von $n$: $\lim_{n\to \infty} n^{1/n}$ muss wieder zuerst $n$ als symbolische Variable deklariert werden.

    >> syms n
    >>limit(n^(1/n), n, Inf)
    ans =
    1

Der exakte, symbolische Wert einer unendlichen Reihe wie 
$\sum_{n=1}^{\infty} \frac{1}{n^2}$ wird mit dem Befehl `symsum` berechnet. Sowohl Anfangs- wie Endwert für $n$ müssen angegeben werden.

    >> a = symsum(1/n^2, n, 1, Inf)
    a =
    pi^2/6

Einen numerischen Wert für `a` erhält man mit `vpa`; ohne Angabe der Anzahl von Ziffern werden 50 Ziffern angezeigt.

    >> vpa(a)
    ans =
    1.6449340668482264364724151666460251892189499012068

Beachten Sie, dass `vpa(a)` weiterhin vom Typ 'sym' ist. Um diesen Wert in weiteren rein numerischen Berechnungen verwenden zu können, muss er vorher mit `double` in eine Gleitkommazahl umgewandelt werden. Diese Zahl hat dann aber nur noch unsere übliche Genauigkeit von 15--16 Stellen.

Hinweis: Die wichtige Anwendung der symbolischen Toolbox auf Probleme mit Differenzialgleichungen werden wir später noch kennenlernen.

Die symbolische Toolbox basiert auf **MuPAD**, einem Computer-Algebra System (engl. *Computer-Algebra-System*, CAS), das ursprünglich an der Universität Paderborn entwickelt und dann von MathWorks aufgekauft wurde.

Die ursprüngliche graphische Oberfläche von MuPAD kann man noch sehen, wenn man in MATLAB den Befehl `mupad` eintippt. Im Menu *Command Bar* auf der rechten Seite können vorgefertigte Befehle aufgerufen und dann ausgefüllt werden. Auf der Eingabefläche werden die Ergebnisse in mathematischer Notation angezeigt.

Zu MuPAD gibt es ganze Bücher, zum Beispiel das "MuPAD Tutorial", Springer-Verlag, 2004.

**Aufgabe**: Generiere in MuPAD die bekannte symbolische Lösung der quadratischen Gleichung $x^2 + p \, x + q = 0$.
