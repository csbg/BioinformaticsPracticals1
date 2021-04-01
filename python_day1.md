<!-- TOC -->

- [Datentypen und Operatoren](#datentypen-und-operatoren)
  - [Numerische Typen](#numerische-typen)
  - [Boolescher Typ](#boolescher-typ)
  - [Texttyp](#texttyp)
- [Interaktive Ein- und Ausgabe](#interaktive-ein--und-ausgabe)
- [Kommandozeilenargumente](#kommandozeilenargumente)
- [Aufgaben](#aufgaben)

<!-- /TOC -->


# Datentypen und Operatoren

Führen Sie die folgenden Anweisungen zunächst im interaktiven Modus aus. Versuchen Sie, das Ergebnis jeder Anweisung zu verstehen.


## Numerische Typen

Numerische Typen werden verwendet, um Zahlen darzustellen.

```python
42      # die ganze Zahl (integer) Zweiundvierzig
3.1415  # eine Gleitkommazahl (float), die pi approximiert
```

Die Raute `#` leitet einen *Kommentar* ein: Python ignoriert alle darauf folgenden Zeichen bis zum Zeilenende.

Zahlen lassen sich durch *arithmetische Operatoren* manipulieren:


```python
1 + 2    # Addition
7 - 4    # Subtraktion
5 * 6    # Multiplikation
54 / 7   # Division
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
2much = 1        # geht nicht - Syntaxfehler
for = 1          # geht nicht - Schlüsselwort
```

Der Radius eines Kreises könnte z.B. durch die folgenden Anweisungen berechnet werden:

```python
pi = 3.1415792
radius = 5
circumference = 2 * radius * pi
area = (radius ** 2) * pi

# Ergebnisse ausgeben
circumference
area
```

Beachten Sie die Klammern in der Formel für den Umfang; wir haben damit explizit angegeben, dass zuerst die Potenz und dann erst das Produkt berechnet werden soll. (In diesem Fall wäre die Potenz aber ohnehin zuerst berechnet werden – die Klammer sind also für Python überflüssig.)

Klammern sind hingegen wichtig, wenn die normale *Operatorrangfolge* geändert werden soll.

```python
3 * 5 + 8  # gleichbedeutend mit (3 * 5) + 8
3 * (5 + 8)
```

Im Zweifelsfall empfiehlt es sich, lieber zu viele als zu wenige Klammern zu verwenden! Axhten Sie außerdem darauf, Leerzeichen (die von Python üblicherweise ignoriert werden) sinnvoll einzusetzen:

```python
3  *  5+8  # hat die Addition hier vielleicht doch Vorrang?
```


## Boolescher Typ

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


## Texttyp

Ein *String* (Zeichenkette) besteht aus einer Folge von Unicode-Zeichen, die in einfache oder doppelte Anführungszeichen eingeschlossen sind.

```python
str_a = "ein String"
str_b = 'auch ein String'
str_c = "ein String mit 'einfachen' Anführungszeichen"

"""ein
mehr-
zeiliger
String"""
```

Zwei Strings können durch den `+`-Operator verknüpft werden:

```python
str_a + str_b
```

Da ein String auch zu den Sequenztypen zählt, können daraus einzelne Zeichen oder Zeichenketten durch *Indizierung* gewonnen werden.

```python
enzyme = "adenylyl cyclase"
enzyme[0]    # erstes Zeichen
enzyme[3]    # viertes Zeichen
enzyme[-2]   # zweites Zeichen, vom Ende aus gezählt
enzyme[2:8]  # Teilstring vom 3. Zeichen bis zum 8. Zeichen
```

Achtung:
* Python beginnt bei der Zählung von Elementen mit null – das erste Zeichen in einem String hat folglich den Index 0.
* In der letzten Zeile wurde das sog. *Slicing* verwendet: `[Startindex : Endindex]`. Der Teilstring beginnt mit dem Zeichen beim Startindex und Endet mit dem Zeichen *vor* (!) dem Endindex.

Machen Sie sich anhand der folgenden Anweisungen deutlich, dass sowohl Startindex als auch Endindex optional sind:

```python
enzyme[:4]   # Teilstring bis Index 4 (exklusive)
enzyme[9:]   # Teilstring ab Index 9 (inklusive)
enzyme[:]    # Kopie des gesamten Strings
enzyme[-9:]  # Teilstring ab der 9. Position vom Ende aus
```

# Interaktive Ein- und Ausgabe

Wir haben die `print()`-Funktion bereits im einleitenden Hallo-Welt-Beispiel kennen gelernt. Eine vergleichbare Funktion existiert zur Eingabe von Daten:

```python
value = input("Please enter any value: ")
value
```

Nach dem Aufruf von `input()` wartet Python, sodass der Benutzer beliebig viele Zeichen eingeben kann. Das Programm wird erst fortgeführt, wenn der Benutzer die Eingabetaste drückt.

Beachten Sie, dass `input()` stets einen String einliest. Das folgende Programm wird also nicht funktionieren:

```python
number = input("Please enter a number: ")
print(number + 10)  # Ein String und ein Integer können nicht addiert werden!
```

Falls Sie den Eingabewert als Zahl verwenden wollen, müssen Sie eine *Typkonvertierung* mittels `int()` oder `float()` durchführen und erhalten so einen numerischen Typ:

```python
x = "10"  # x ist der String "10"
x * 2     # Fehler!

y = int(x)  # wandelt den String "10" in einen Integer 10 um
y * 2       # funktioniert!


a = float("2.3")  # a verweist nun auf die Gleitkommazahl 2.3
a / 5.6           # funktioniert!
```


Das folgende Programm berechnet beispielsweise die Summe zweier Zahlen, die vom Benutzer eingegeben werden. Speichern Sie diese Anweisungen in der Datei `produkt.py` und führen Sie die Datei mit Python aus (`python3 produkt.py`)!

```python
print("Berechnung des Produkts zweier Zahlen")

a = input("Erste Zahl: ")
a = float(a)

b = input("Zweite Zahl: ")
b = float(b)

print("Das Ergebnis lautet", a * b)
```


# Kommandozeilenargumente

Ein Python-Programm auch Werte von der Kommandozeile verarbeiten und funktioniert dann ähnlich wie die Shell-Kommandos, die wir in der ersten Woche des Kurses kennengelernt haben. Diese Funktionalität ist in einem Modul namens `sys` implementiert (mehr zu Modulen morgen). Wir erzeugen ein Python-Skript `argumente.py` mit dem folgenden Inhalt:

```python
from sys import argv

print(argv)
print(argv[0])
print(argv[1:])
```

Anschließend führen wir das Skript folgendermaßen aus:

```bash
python argumente.py a 12 drittes_argument

#> ['argumente.py', 'a', '12', 'drittes_argument']
#> argumente.py
#> ['a', '12', 'drittes_argument']
```

Die Ausgabe verrät uns:
- `argv` ist eine *Liste* mit vier Elementen. (Der Listentyp wird später genauer behandelt. Vorerst reicht es aus, dass wir auf einzelne Elemente zugreifen können – und das funktioniert genau wie bei Strings: Variablenname gefolgt vom Index in eckigen Klammern.)
- Das erste Element `argv[0]` enthält den Namen des aufgerufenen Skripts (`'argumente.py'`).
- Die verbleibenden Elemente enthalten die Kommandozeilenargumente in der angegebenen Reihenfolge.

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

#> Das Ergebnis lautet 350.0
```

# Aufgaben

54 // 7  # ganzzahlige Division
54 % 7   # Modulo
2 ** 8   # Potenz

komplexe Zahlen




Shortcut	Meaning
+=	add and assign
-=	subtract and assign
*=	multiply and assign
/=	divide and assign
%=	assign modulus
**=	exponentiate and assign
g = 5
g += 7   # g is now 12
g *= 10  # g is now 120
g /= 15  # g is now 8
g **= 2  # g should be 64 - really?
g
64.0
There are even more types of operators:
