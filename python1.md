# Kurseinheit 1

## Inhalt

- [Datentypen und Operatoren](#datentypen-und-operatoren)
  - [Numerische Typen](#numerische-typen)
  - [Boolescher Typ](#boolescher-typ)
  - [Texttyp](#texttyp)
  - [`None`-Typ](#none-typ)
- [Interaktive Ein- und Ausgabe](#interaktive-ein--und-ausgabe)
- [Kommandozeilenargumente](#kommandozeilenargumente)
- [Aufgaben](#aufgaben)



## Datentypen und Operatoren

Führen Sie die folgenden Anweisungen zunächst im interaktiven Modus aus. Versuchen Sie, das Ergebnis jeder Anweisung zu verstehen. Bitte zögern Sie nicht, sich bei Schwierigkeiten an Ihren Kursleiter zu wenden.


### Numerische Typen

Numerische Typen werden verwendet, um Zahlen darzustellen.

```python
42      # die ganze Zahl (integer) Zweiundvierzig
3.1415  # eine Gleitkommazahl (float), die pi approximiert
```

Die Raute `#` leitet einen *Kommentar* ein: Python ignoriert alle darauf folgenden Zeichen bis zum Zeilenende. (Daher können Sie sämtliche Kommentare in den folgenden Beispielen ruhig weglassen, wenn Sie die Anweisungen in Python eingeben!)

Zahlen lassen sich durch *arithmetische Operatoren* manipulieren:


```python
1 + 2    # Addition
7 - 4    # Subtraktion
5 * 6    # Multiplikation
54 / 7   # Division
10 % 4   # Modulo
```

Der *Zuweisungsoperator* weist einem Wert einen Namen zu:

```python
a = 343
b = 14
```

Nachdem ein Wert benannt wurde, kann der Name anstelle des Werts in einer Anweisung aufscheinen:

```python
a
a + b
```

Variablennamen dürfen aus beliebigen Kombinationen von Buchstaben, Ziffern, und dem Unterstrich bestehen. Sie dürfen jedoch mit keiner Ziffer beginnen. Bestimmte Schlüsselwörter (z.B. `if`, `for`) dürfen nicht als Variablennamen verwendet werden.

```python
x = 1            # ok
first_value = 1  # ok
number5 = 1      # ok
2much = 1        # Syntaxfehler – Name beginnt mit einer Ziffer
for = 1          # Syntaxfehler – for ist ein Schlüsselwort
```

Die folgenden Anweisungen berechnen beispielsweise Umfang und Fläche eines Kreises mit dem Radius 5:

```python
pi = 3.1415792
radius = 5
circumference = 2 * radius * pi
area = (radius ** 2) * pi

# Ergebnisse ausgeben
circumference
area
```

Wir beachten die Klammern in der Formel für den Umfang: Sie legen explizit fest, dass zuerst die Potenz und dann erst das Produkt berechnet werden soll. (In diesem Fall wäre die Potenz aber ohnehin zuerst berechnet worden – die Klammern sind also für Python überflüssig.)

Klammern sind hingegen wichtig, wenn die normale *Operatorrangfolge* geändert werden soll.

```python
3 * 5 + 8  # gleichbedeutend mit (3 * 5) + 8
3 * (5 + 8)
```

Im Zweifelsfall empfiehlt es sich, lieber zu viele als zu wenige Klammern zu verwenden! Wir achten außerdem darauf, Leerzeichen sinnvoll einzusetzen (auch wenn sie von Python meist ignoriert werden):

```python
3  *  5+8  # hat die Addition hier vielleicht doch Vorrang?
```


### Boolescher Typ

Der Boolesche Typ kennt nur zwei Werte: `True` (wahr) und `False` (falsch). Boolesche Typen sind häufig das Ergebnis von *Vergleichsoperatoren*:

```python
3 > 2    # größer als
7 < 4    # kleiner als
10 >= 8  # größer/gleich
2 <= 2   # kleiner/gleich
5 == 5   # gleich
10 != 9  # ungleich
```

Boolesche Werte können durch *logische Operatoren* verknüpft werden:

```python
(2 < 3) and (15 / 4 > 1)  # Konjunktion; wahr, falls beide Operanden wahr sind
(5 != 5) or (12 >= 13)    # Disjunktion; wahr, falls zumindest ein Operand wahr ist
not (5 < 7)               # Negation, ändert True zu False und umgekehrt
```


### Texttyp

Ein *String* (Zeichenkette) besteht aus einer Folge von Unicode-Zeichen, die in einfache oder doppelte Anführungszeichen eingeschlossen sind.

```python
str_a = "ein String"
str_b = 'auch ein String'
str_c = "ein String mit 'einfachen' Anführungszeichen"
```

Python kennt auch mehrzeilige Strings, die in drei Anführungszeichen eingeschlossen sind.

```python
"""ein
mehr-
zeiliger
String"""
```

Zwei Strings können durch den `+`-Operator verknüpft werden (sog. *Konkatenation*):

```python
str_a + str_b
```

Da ein String auch zu den Sequenztypen zählt (s. Kurseinheit 2), können wir daraus einzelne Zeichen oder Zeichenketten durch *Indizierung* gewinnen.

```python
enzyme = "adenylyl cyclase"
enzyme[0]    # erstes Zeichen
enzyme[3]    # viertes Zeichen
enzyme[-2]   # zweites Zeichen, vom Ende aus gezählt
enzyme[2:8]  # Teilstring vom 3. Zeichen bis zum 8. Zeichen
```

Achtung:
* Python beginnt bei der Zählung von Elementen mit null – das erste Zeichen in einem String hat folglich den Index 0 (sog. *zero-based indexing*).
* In der letzten Zeile haben wir das sog. *Slicing* verwendet: `[Startindex : Endindex]`. Der Teilstring beginnt mit dem Zeichen beim Startindex und endet mit dem Zeichen *vor* (!) dem Endindex.

Sowohl Startindex als auch Endindex sind optional:

```python
enzyme[:4]   # Teilstring bis Index 4 (exklusive)
enzyme[9:]   # Teilstring ab Index 9 (inklusive)
enzyme[:]    # Kopie des gesamten Strings
enzyme[-3:]  # Teilstring ab der 3. Position vom Ende aus
```


### `None`-Typ

Python kennt auch den Datentyp `None`, der das „Nichts“ explizit darstellt:

```python
nichts = None
print(nichts)
```

Mit dem `is`-Operator können wir überprüfen, ob eine Variable `None` enthält (wir sollten dies *nicht* mithilfe eines Booleschen Vergleichs tun):

```python
nichts is None  # bevorzugte Variante
nichts == None  # vermeiden
```


## Interaktive Ein- und Ausgabe

Wir haben die `print()`-Funktion zur Ausgabe von Daten bereits im einleitenden Hallo-Welt-Beispiel kennen gelernt. Eine vergleichbare Funktion existiert zur Eingabe von Daten:

```python
value = input("Please enter any value: ")
value
```

Nach dem Aufruf von `input()` wartet Python auf die Eingabe beliebig vieler Zeichen. Das Programm wird erst fortgeführt, nachdem die Eingabetaste gedrückt wurde.

Achtung: `input()` list stets einen String ein. Das folgende Programm wird also nicht funktionieren:

```python
number = input("Please enter a number: ")
print(number + 10)  # Ein String und ein Integer können nicht addiert werden!
```

Falls wir den Eingabewert als Zahl verwenden wollen, müssen wir eine *Typkonvertierung* mittels `int()` oder `float()` durchführen und erhalten so einen numerischen Typ:

```python
x = "10"  # x ist der String "10"
x * 2     # Fehler!

y = int(x)  # wandelt den String "10" in einen Integer 10 um
y * 2       # funktioniert!

a = float("2.3")  # a verweist nun auf die Gleitkommazahl 2.3
a / 5.6           # funktioniert!
```


Das folgende Programm berechnet beispielsweise die Summe zweier Zahlen, die vom Benutzer eingegeben werden. Speichern Sie diese Anweisungen in der Datei `produkt.py` und führen Sie die Datei mit Python aus (`python produkt.py`)!

```python
print("Berechnung des Produkts zweier Zahlen")

a = input("Erste Zahl: ")
a = float(a)

b = input("Zweite Zahl: ")
b = float(b)

print("Das Ergebnis lautet", a * b)
```


## Kommandozeilenargumente

Ein Python-Programm kann auch Werte von der Kommandozeile verarbeiten und funktioniert dann ähnlich wie die Shell-Befehle, die wir in der ersten Woche des Kurses kennengelernt haben. Um diese Funktionalität zu nutzen, müssen wir das Modul `sys` importieren (mehr zu Modulen in Kurseinheit 2). Wir erzeugen ein Python-Skript `argumente.py` mit dem folgenden Inhalt:

```python
from sys import argv

print(argv)
print(argv[0])
print(argv[1:])
```

Anschließend führen wir das Skript folgendermaßen aus:

```bash
python argumente.py a 12 drittes_argument
```

Anhand der Ausgabe erkennen wir:
- Erste Zeile: `argv` ist eine *Liste* mit vier Elementen. (Der Listentyp wird in Kurseinheit 2 genauer behandelt. Vorerst genügt es uns zu wissen, dass wir auf einzelne Elemente zugreifen können – und das funktioniert genau wie bei Strings: Variablenname gefolgt vom Index in eckigen Klammern.)
- Zweite Zeile: Das erste Element `argv[0]` enthält den Namen des aufgerufenen Skripts (`'argumente.py'`).
- Dritte Zeile: Die verbleibenden Elemente `argv[1:]` enthalten die Kommandozeilenargumente in der angegebenen Reihenfolge.

Unter Verwendung von Kommandozeilenargumenten können wir das Produkt zweier Zahlen also so berechnen:

```python
from sys import argv
a = float(argv[1])
b = float(argv[2])
print("Das Ergebnis lautet", a * b)
```

Wir speichern diese Befehle in  `produkt2.py` und führen das Programm aus:
```bash
python produkt2.py 35 10
```



## Aufgaben

Jede Kurseinheit schließt mit einer Reihe von Aufgaben, die Sie selbstständig bearbeiten sollen. Ihre Lösungen bilden die Grundlage für die Beurteilung des Kurses. Bei jeder Aufgabe ist die maximale Anzahl von Punkten vermerkt, die für eine richtige Lösung vergeben werden.

Speichern Sie alle Dateien, die Sie in Kurseinheit 1 erstellen müssen, im Ordner `python1` in Ihrem Home-Verzeichnis.



#### Aufgabe 1.1 (3 P)

Diese Aufgabe haben Sie erfolgreich gelöst, wenn Sie die Code-Beispiele dieser Kurseinheit durchgearbeitet haben. Achten Sie darauf, dass sich im Ordner `python1` die folgenden Dateien befinden, die Sie im Rahmen der Übungen erstellt haben:
- `produkt.py`
- `argumente.py`
- `produkt2.py`



#### Aufgabe 1.2 (1 P)

Schreiben Sie ein Python-Programm, dass die folgenden Anweisungen nacheinander ausführt. Speichern Sie das Programm unter `aufgabe_1_2.py`.
1. Speichere die ganze Zahl 32 in der Variable `z` und die Gleitkommazahl 2.5 in der Variable `a`.
2. Berechne das Produkt von `z` und `a` und speichere das Ergebnis in der Variable `b`.
3. Dividiere `a` durch 8 und speichere das Ergebnis wiederum in `a`.
4. Gib aus, ob `a` größer als `b` ist (hier soll das Program einen einzigen Booleschen Wert ausgeben.)
5. Gib aus, ob die Summe von `z` und 11 ungleich 44 ist (auch hier soll das Programm einen einzigen Booleschen Wert ausgeben).



#### Aufgabe 1.3 (2 P)

Schreiben Sie ein Programm `aufgabe_1_3.py`, das zur Eingabe dreier Zahlen auffordert und anschließend ausgibt, ob der Rest bei Division der ersten Zahl durch die zweite gleich der dritten Zahl ist. Ein Aufruf dieses Programms könnte z.B. folgendermaßen aussehen:

```
$ python aufgabe_1_3.py
Dividend: 10
Divisor: 4
Rest: 2
True
```

Die drei Werte 10, 4, und 2 wurden eingeben, das Programm gibt anschließend `True` zurück.

Ein anderer Aufruf des Programms könnte so aussehen:

```
$ python aufgabe_1_3.py
Dividend: 835
Divisor: 111
Rest: 20
False
```

Es ist egal, ob Ihr Programm etwas in den Eingabezeilen schreibt (z.B. „Dividend“ in der ersten Zeile). Wichtig ist lediglich, dass in der letzten Zeile der Ausgabe ein einziger Boolescher Wert steht, der richtig berechnet wurde.


#### Aufgabe 1.4 (2 P)

Schreiben Sie ein Programm `aufgabe_1_4.py`, welches vier Kommandozeilenargumente einliest. (Diese Argumente werden im Folgenden als `dna`, `x`, `a` und `e` bezeichnet.) Das erste Argument ist ein beliebig langer String, der eine DNA-Sequenz darstellt. Die Argumente zwei bis vier sind jeweils ein Integer. Das Programm soll anschließend zwei Zeilen ausgeben:
- In der ersten Zeile steht die `x`-te Base von links in `dna`.
- In der zweiten Zeile steht jene Teilsequenz von `dna`, die zwei Basen vor der durch `a` angegebenen Stelle beginnt und an der durch `e` angegebenen Stelle endet.

Die Zählung der Basen folgt dabei von Eins ausgehend (d.h. die erste Base in der DNA-Sequenz trägt auch tatsächlich die Nummer „1“).

Sie können die korrekte Funktionalität Ihres Programm anhand der folgenden Beispiele testen:

```
                         x  a   e
                         ↓  ↓   ↓
$ python aufgabe_1_4.py AGCTATAGTAATCCAAT 2 5 9
G
CTATAGT

                              a     x      e
                              ↓     ↓      ↓
$ python aufgabe_1_4.py ATCTACGCGATATCGCGATAGCCGATGCTGACGACTGACTTGACG 13 7 20
T
ACGCGATATCGCGATA
```
