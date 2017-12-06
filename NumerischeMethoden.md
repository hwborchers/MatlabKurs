# Numerik mit MATLAB
Hans W Borchers<br />DHBW Mannheim<br />Fakultät für Maschinenbau  
Skript zum Kurs, WS 2016  
<hr>
# Numerische Methoden

## Nullstellenbestimmung

Matlab bietet vorgefertigte Funktionen zum Bestimmen von Nullstellen an, aber es ist nicht schwer, auch eigene Methoden zur Nullstellenbestimmung zu programmieren, zum Beispiel die Bisektionsmethode und das Newtonverfahren.

Die MATLAB Funktion zur Bestimmung der Nullstelle einer Funktion einer Veränderlichen ist `fzero`. Diese Funktion bestimmt genauer gesagt den Ort eines Vorzeichenwechsels einer Funktion. Aus diesem Grund ist es nicht möglich, mit dieser Methode die Nullstelle von Funktionen ohne einen solchen Vorzeichenwechsel, z.B. von $f(x) = x^2$ zu bestimmen.

Zunächst wird $f$ als anonyme Funktion `f` definiert:

    >> f=@(x) sin(x)+x^2-1;

Um einen Überblick zu erhalten, in welchem Bereich die Nullstelle zu suchen ist, ist es immer eine gute Idee, die Funktion zunächst mit `fplot` zu zeichnen. Damit der Nulldurchgang der Funktion besser zu sehen ist, wird noch ein Gitter hinterlegt.

    >> fplot(f)
    >> grid on

Man kann sehen, dass die Funktion die x-Achse zweimal zwischen den Werten -2 und 1 schneidet.

Gesucht ist zunächst die Nullstelle zwischen den x-Werten $a = -2$ und $b = 0$. Dabei kann entweder in der Nähe eines x-Wertes, `x_0 = fzero(f,a)`, oder oder in dem Intervall $[a, b]$ zwischen $a$ und $b$ gesucht werden: `x_0 = fzero(f,[a b])`.

    >> fzero(f, [-2, 0])
    ans = -1.4096
    
    >> fzero(f, [0, 1])
    ans =  0.6367

Zu beachten ist, dass `fzero` immer nur eine Nullstelle findet und dann aufhört. Welche Nullstelle das ist, ist nicht ohne weiteres vorherzusehen. Daher muss eine gesuchte Nullstelle in einem Interval ausreichend eingegrenzt werden.

`fzero` wird keine Funktionen ohne Vorzeichenwechsel lösen. Betrachte dazu das Beispiel $f(x) = x^2$ welche $x = 0$ als NUllstelle hat.

    >> fzero(@(x) x^2, [-1, 1])
    Error using fzero (line 272)
    The function values at the interval endpoints must differ in sign.

In diesem Beispiel ist $f$ ein Polynom, in MATLAB repräsentiert durch den Vektor `[1, 0, 0]`, daher bietet sich die `roots` Funktion an:

    >> roots([1, 0, 0])
    ans = 
          0
          0

$x_0 = 0$ ist eine Doppelnullstelle. Leider funktioniert dieser Ansatz nicht für allgemeine Funktionen. Eine andere Möglichkeit wäre, das Newtonsche Verfahren zu implementieren. Für einen geeigneten Startwert $x_1$ wird die Folge
$$
  x_{n+1} = x_n - \frac{f(x_n)}{f'(x_n)}
$$  
gegen eine Nullstelle $x_0$ konvergieren. Allerdings wissen wir noch nicht, wie wir die Ableitung einer Funktion numerisch berechnen sollen.

Hinweis: Eine weitere Möglichkeit wäre, das *Sekantenverfahren* zu implementieren, eine Variante des Newton-Verfahrens, das ohne die Ableitung auskommt.

**Aufgabe**: Die Kapazität eines eines Diffusionsvorganges werde annähernd mit dem Ausdruck $C(r) = 2r^2 + 3 \log(r) + 1$ berechnet. Bestimme ein $r$ so dass die Kapazität gleich 2 ist.

**Aufgabe**: Die Funktion $f(x) = x\,e^{-x}$ taucht häufig in technischen Anwendungen auf. Bestimme den Punkt $x$ mit $f(x) = 1$, die sogenannte Omega-Konstante. (Definiere die Funktion unter dem Namen 'xexp' in einer Funktionsdatei.)


## Minima und Maxima

Eine ebenso wichtige Aufgabe ist es, Minima und/oder Maxima einer Funktion zu bestimmen, insbesondere in einer Zeit, die so sehr auf Optimierung und Effizienz ausgerichtet ist.

Für Polynome erscheint das einfach. Aus der Analysis (Kurvendiskussion) wissen wir, dass in einem *Optimum* (Minimum bzw. Maximum) $x$ einer differenzierbaren Funktion $f$ gelten muss: $f'(x) = 0$. Das heisst, ein Minimum ist zugleich Nullstelle der Ableitung.

**Beispiel**: $p(x) = x^3 - x^2 - 4x + 4$. Die Ableitung wird mit `polyder` berechnet und deren Nullstelle mit `roots` bestimmt.

    >> p = [1, -1, -4, 4];
    >> p1 = polyder(p)
    p1 =  3    -2    -4         % 3*x^2 - 2*x - 4
    >> r1 = roots(p1)
    r1 =
        1.5352
       -0.8685
    >> polyval(p, r1)
    ans =
       -0.8794
        6.0646

Das Polynom hat ein Maximum bei $x = -0.8794$ und ein Minimum bei $x = 1.5352$. Diese Optima sind *lokal*, d.h. sie sind minimal oder maximal in einer Umgebung des Punkte. Global gesehen läuft das Polynom natürlicher gegen `Inf` bzw. `-Inf`.

(Genau genommen müsste noch die zweite Ableitung benutzt werden, um zu sehen, was die Minima und Maxima sind.)

Für die Bestimmung des Minimums einer allgemeinen Funktion gibt es in MATLAB den Befehl `fminbnd`. Dabei steht 'bnd' für "bounded", also für das Minimum einer Funktion in einem beschränkten Bereich, einem Intervall. Der Aufruf von `fminbnd` ist ähnlich wie der von `fzero`, nur das Intervall wird nicht durch einen Vektor, sondern als zwei getrennte Argumente übergeben.

Im Beispiel scheiben wir das Polynom als anonyme Funktion, dann ist:

    >> p = @(x) x.^3 - x.^2 - 4*x + 4;
    >> fminbnd(p, -3, 3)
    ans = 1.5352

Um gleichzeitig noch den Funktionswert im Minimum zu erhalten, muss `fminbnd` nur mit einem zusätzlichen Ausgabeparameter aufgerufen werden.

    >> [xmin, fmin] = fminbnd(p, -3, 3)
    xmin =  1.5352
    fmin = -0.8794

Gibt es mehrere Minima in dem angegebenen Bereich, wird ein Minimum gefunden, aber nicht notwendigerweise das mit dem kleinsten Funktionswert.

Um Maxima zu finden, "drehen wir die Funktion um", das heisst multiplizieren sie mit -1, denn ein Minimum der Funktion $f$ ist ein Maximum der Funktion $-f$, und umgekehrt.

    >> [xmax, fmax] = fminbnd(@(x) -p(x), -3, 3)
    xmax = -0.8685
    fmax = -6.0646      % besser: fmax = 6.0646

Der Wert von `fmax` muss dann wieder mit -1 multipliziert werden!

**Aufgabe**: Gegeben die gedämpfte Schwingung $s(x) = e^{-0.05x} \sin(x)$. Die Sinus-Funktion hat ihr erstes Maximum in $x = \pi/2$, wo hat die Funktion $s$ ihr erstes Maximum, und um wieviel früher oder später als $\pi/2$?


## Numerische Differentiation

Die Ableitung bzw. der Differenzialquotient eine Funktion $f$ in einem Punkt $x_0$ wird in der Analysis definiert als der Grenzwert
$$
  \lim_{h \to 0} \frac{f(x_0 + h) - f(x_0)}{h}
$$
Einen solchen Grenzwert kann man numerisch nicht nachbilden. Daher müssen wir uns mit einem geeigneten, wahrscheinlich sehr kleinen $h$ begnügen. Wenn $h$ positiv gewählt wird, berechnen wir hier offenbar den "rechsseitigen", für negatives h den linksseitigen Grenzwert.

Für eine differenzierbare Funktion sollte der rechtsseitig und der linksseitige Grenzwert gleich sein. Daher liegt es nahe, den Mittelwert aus diesen beiden Werten als numerischen Differentialquotienten zu nehmen.
$$
  \frac{1}{2}(\frac{f(x_0 + h) - f(x_0)}{h} + \frac{f(x_0) - f(x_0 - h)}{h}) = \frac{f(x_0 + h) - f(x_0 - h)}{2h}
$$
Der Ausdruck rechts heisst auch "zentraler Differenzquotient" (engl. *central differnce formula*). Die folgende Funktion `deriv(f, x0, h)` , in einer Datei `deriv.m`, berechnet diesen Ausdruck für eine Funktion `f`, einen Punkt `x0` und ein `h` grösser als Null.

```matlab
function df = deriv(f, x0, h)
df = (f(x0 + h) - f(x0 - h)) / (2*h);
end
```

Wie klein soll $h$ gewählt werden? Dazu machen wir einen Test. Wir bestimmen diesen numerischen Differenzialquotienten für die Exponentialfunktion $e^x$ im Punkt $x = 1$ für verschiedne $h$. Die Ableitung in diesem Punkt kennen wir, sie ist $e$.

    >> e = exp(1);
       for i = 1:15
	     df = deriv(@exp, 1.0, 10^-i);
	     fprintf('%2d  %3.15f  %2.4e\n', i, df, abs(df-e))
       end
     1  2.722814563947418  4.5327e-03
     2  2.718327133382692  4.5305e-05
     3  2.718282281505724  4.5305e-07
     4  2.718281832989611  4.5306e-09
     5  2.718281828517633  5.8587e-11
     6  2.718281828295588  1.6346e-10
     7  2.718281828517632  5.8587e-11
     8  2.718281821856294  6.6028e-09
     9  2.718281821856294  6.6028e-09
    10  2.718281155722480  6.7274e-07
    11  2.718292257952725  1.0429e-05
    12  2.718492098097158  2.1027e-04
    13  2.717825964282383  4.5586e-04
    14  2.708944180085382  9.3376e-03
    15  2.886579864025407  1.6830e-01

Warum der absolute Fehler für kleinere $h < 10^{-7}$ wieder grösser wird, klären wir besser mündlich in der Vorlesung. Auf jeden Fall scheint ein Wert um $h = 10^{-6}$ am geeignetsten zu sein -- und die Theorie bestätigt das. Daher ändern wir die `deriv` Funktion so dass als dies als 'Fehlwert' gesetzt ist.

```matlab
function df = deriv(f, x0, h)
if nargin < 3, h = 1e-06; end           % h = nthroot(eps, 3)
df = (f(x0 + h) - f(x0 - h)) / (2*h);
end
```

Um zum Beispiel eine Funktion zu schreiben, welche die Ableitung der Funktion $f(x) = \sin(x) \, \cos(x)$ darstellt, genügt daher:

    >> f  = @(x) sin(x) .* cos(x);
    >> df = @(x) deriv(f, x);

**Aufgabe**: Vergleiche die Funktion `df` mit der symbolischen Ableitung von `f`, am besten an Hand eines Plots und des absoluten Fehlers über das Intervall $[0, 2\pi]$.

Ebenso ist es jetzt möglich, das Newton-Verfahren zu implementieren, ohne symbolisch die Ableitung der Funktion bestimmen zu müssen.

```matlab
function x0 = newton(f, x1)
x2 = x1 - f(x1) / deriv(f, x1);
while abs(x2 - x1) > 1e-15
    x1 = x2;
    x2 = x1 - f(x1) / deriv(f, x1);
end
x0 = x2;
end
```

**Aufgabe**: Bestimme die Nullstelle der Funktion $f(x) = sin^2(x)$ im Intervall $[2, 4]$ mit Hilfe des Newton-Verfahrens. (`fzero` würde nicht zum Ergebnis führen.)

## Numerische Integration

Das Integral einer Funktion $f$ auf einem Intervall $[a, b]$ ist definiert als Grenzwert der Summen

als Annäherung an eine Bestimmung der Fläche zwischen dem Funktionsgraphen und der x-Achse. Ähnlich wie der Differentialquotient kann dieser Ausdruck numerisch angenähert werden durch einen endlichen Ausdruck.

Eine Möglichkeit ist die Mittelpunktsformel (engl. *midpoint formula*):
Das Intervall $[a, b]$ wird in eine endliche Zahl von gleich grossen Teilintervallen einer Länge $h$ zerlegt und die Teilfläche durch das Produkt aus $h$ und dem Funktionswert in der Mitte des Teilintervals abgeschätzt. Für genügend kleines $h$ bzw. eine genügend grosse Anzahl von Teilintervallen sollte das Integral recht gut betimmt werden können.

Die folgende Funktion `midpt` implementiert diesen Ansatz. `n` ist die Anzahl der Teilintervalle.

```matlab
function I = midpt(f, a, b, n)
if nargin < 4, n = 100; end
h = (b - a) / n;
x = a:h:b;
s = 0;
for i = 1:(n-1)
    s = s + f(x(i) + h/2);
end
I = h * s;
end
```

Als Beispiel berechne das Integral der Sinusfunktion von 0 bis $\pi$.

    >> midpt(@sin, 0, pi)
    ans = 1.9996            % Fehler: 0.0004

Das richtige Ergebnis wäre $cos(0) - cos(\pi) = 2$. Mit 200 Teilintervallen wird das Ergebnis besser.

    >> midpt(@sin, 0, pi, 200)
    ans = 1.9999            % Fehler: 0.0001

In Mathematik-Vorlesungen lernt man die *Simpson-Formel* als eine sehr gute Näherung eines Integrals kennen. Dabei wird die zu integrierende Funktion durch eine quadratische Funktion approximiert, nicht durch einen linearen Ausdruck, wie in der Mittelpunktsregel (oder der Trapezregel).

Rechnet man die Summen über alle Teilintervalle zusammen, entsteht der folgende Ausdruck
$$
  \int_a^b f(x)\,dx \approx \frac{h}{3} (y_1 + 4(y_2+...+y_{2n}) 
  + 2(y_3+...+y_{2n-1}) + y_{2n+1})
$$
Dabei wird das Intervall $[a, b]$ gleichmässig in $2n$ Teilinervalle aufgeteilt, die Funktionswerte in den Stützstellen sind $y_i = f(x_i)$ für $i = 1, ..., 2n+1$. Die entsprechende MATLAB Funktion kann dann so aussehen (vorausgesetzt, die Funktion $f$ ist vektorisiert):

```matlab
function I = simpson(f, a, b, n)
h = (b - a)/(2*n);
x = a:h:b;
y = f(x);
I = y(1) + y(2*n+1) + 4*(sum(y(2:2:(2*n)))) + 2*(sum(y(3:2:(2*n-1))));
I = I * h / 3;
end
```

Bezogen auf das Beispiel oben verkleinert sich der Fehler bei eine vergleichbaren Anzahl von Teilintervallen deutlich.

    >> simpson(@sin, 0, pi, 100)
    ans = 2.0000            % Fehler: < 10^-9

MATLAB verfügt über einen eigenen Befehl zur Integration, `integral`. Diese Funktion benutzt einen *adaptiven* Ansatz. Das heisst, die Breite der einzelnen Teilintervalle variiert: Je stärker die Funktion sich in einem Bereich verändert, desto schmaler werden auch die Teilintervalle.

    >> integral(@sin, 0, pi)
    ans = 2

**Aufgabe**: Berechne das Integral
$\int_0^1 \frac{x^4 (1-x)^4}{1+x^2}\,\mathrm{d}x$

Die Bogenlänge einer Kurve $y = y(x)$ für $a \le x \le b$ wird berechnet nach der folgenden Formel:
$$
  s = \int_a^b \sqrt{1 +y'^2(x)} \,\mathrm{d}x
$$
**Aufgabe**: Bestimme die Länge der Sinuskurve zwischen 0 und $\pi$, und zwar einmal unter Benutzung der symbolischen Ableitung, einmal unter Benutzung der numerischen Ableitungsfunktion `deriv`.

**Aufgabe**: Für ein elektrisches Bauteil werde der Energieverbrauch in Abhängigkeit eines Parameters $p$ ermittelt zu $\int_0^\pi \sin(x)\, \cos(p\,x) \mathrm{d}x$. Für welches $p$ ist der Verbrauch am geringsten? Kann das in $\pi/2$ sein (Vermutung der Ingenieure)?

Die Koordinaten $(x_S, y_S)$ des Schwerpunktes einer Fläche, die durch einen Funktionsgraphen $x \to f(x)$, die x-Achse und die senkrechten Linien in $x = a$ und $x = b$ begrenzt wird, berechnen sich zu
$$
  x_S = \frac{1}{A} \int_a^b x \, f(x)\,\mathrm{d}x ,\quad 
  y_S = \frac{1}{A} \int_a^b \frac{f(x)^2}{2}\,\mathrm{d}x
$$
Dabei ist $A$ die Gesamtfläche (dieses Flächenstücks), also
$A = \int_a^b f(x) \,\mathrm{d}x$.

Wir schreiben eine Funktion `schwerpt(f, a, b)`, welche die Koordinaten des Schwerpunktes berechnet und als einen Vektor `[xS, yS]` und zurückgibt. Innerhalb dieser Funktion braucht es zwei weitere Funktionen, $x \to x\,f(x)$ und $x \to f^2(x)$, die integriert werden müssen. Mit anonymen Funktionen lässt sich das in MATLAB sehr elegant programmieren.

```matlab
function xy = schwerpt(f, a, b)
A = integral(f, a, b);
xs = integral(@(t) t .* f(t), a, b) / A;
ys = integral(@(t) f(t).^2, a, b) / A / 2;
xy = [xs; ys];
end
```

**Beispiel**: Wo liegt der Schwerpunkt der Fläche, die zwischen $0$ und $\pi$ von der Sinuskurve und der x-Achse begrenzt wird?

    >> schwerpt(@sin, 0, pi)
    ans =
        1.5708
        0.3927

Die erste Zahl ist offenbar $\pi/2$, also die Mitte des Intervalls, was aus Symmetriegründen richtig sein muss. Kennen Sie die zweite Zahl?

Entsprechend gibt es auch einen Befehl `integral2`, der das Integral einer zweidimensionalen Funktion, also eine Funktion auf einem Bereich von $\mathrm{R}^2$, berechnet. Der allgemeine Aufruf ist `integral2(fun,xmin,xmax,ymin,ymax)`, wobei die untere und obere Grenze von `y` auch noch von `x` abhängen darf.

**Beispiel**: Berechne das Integral der Funktion $f(x, y) = \sqrt{1 - x^2 - y^2}$ auf dem Einheitskreis $x^2 + y^2 \le 1$ im ersten Qudranten ($x \ge 0, y \ge 0$). Wir definieren eine Funktion $y_{max} = \sqrt{1 - x^2}$, so dass für festes $x$ von $0$ bis $y_{max}(x)$ integriert wird.

    >> ymax = @(x) sqrt(1 - x.^2);
    >> f = @(x, y) sqrt(1 - x.^2 - y.^2);
    >> v = integral2(f, 0, 1, 0, ymax)
    v = 
        0.5236
    >> (4/3)*pi - 8*v
    ans = 
        -6.8301e-13

Das Ergebnis sollte ein Achtel des Volumens $\frac{4}{3} \pi r^3$ einer Kugel vom Radius $r = 1$ sein.
    

**Aufgabe**: Berechne das Integral
$\int_G log(x^2 + y^2) \,\mathrm{d}x\,\mathrm{d}y$ auf dem Gebiet $G$ der Ebene, das durch die Kreise $x^2 + y^2 = 9$ und $x^2 + y^2 = 25$ begrenzt wird.
