<!-- TOC -->

- [Kontrollfluss](#kontrollfluss)
  - [Funktionen](#funktionen)
  - [Fallunterscheidungen und Verzweigungen](#fallunterscheidungen-und-verzweigungen)
  - [Schleifen](#schleifen)
  - [`while`-Schleifen](#while-schleifen)
  - [`for`-Schleifen](#for-schleifen)
  - [Modularisierung](#modularisierung)
- [Datenstrukturen](#datenstrukturen)
  - [Listen](#listen)
  - [Tupel](#tupel)
  - [Mengen](#mengen)
  - [Dictionaries](#dictionaries)
- [Reguläre Ausdrücke](#reguläre-ausdrücke)

<!-- /TOC -->


# Kontrollfluss

Python führt die Anweisungen in einem Skript der Reihe nach aus.

```python
a = 4
b = 9
c = 5
mean = (a + b + c) / 3
print(mean)
```

Es ist offensichtlich, dass dieser grundlegende Kontrollfluss nur einfachste Berechnungen ermöglicht. Daher stellt Python eine Reihe von Möglichkeiten zur Verfügung, um den Kontrollfluss zu modifizieren.


## Funktionen

Eine Funktion wird folgendermaßen *aufgerufen*:

- Funktionsname
- linke runde Klammer
- beliebig viele Argumente (passend zur Funktionsdefinition), die durch Kommas getrennt werden
- rechte runde Klammer

Wir haben bereits `print()` kennen gelernt, ein Beispiel für eine *eingebaute Funktion*. Die Print-Funktion schreibt die Werte ihrer Argumente auf die Standardausgabe (stdout):

```python
print("Hallo, Welt!")
```


Eine Funktion wird folgendermaßen *definiert*:

- Schlüsselwort `def`
- Funktionsname
- linke runde Klammer
- beliebig viele Parameter, die durch Kommas getrennt werden
- rechte runde Klammer
- Doppelpunkt
- Funktionskörper eingerückt

Hier definieren wir eine Funktion, die den Mittelwert dreier Zahlen berechnet:

```python
def mean_of_three(a, b, c):
    result = (a + b + c) / 3
    return result

m = mean_of_three(5, 8, 2)
print(m)
```

Eine Funktion kann einen (oder mehrere) Werte mittels der `return`-Anweisung zurückgeben. Im obigen Beispiel wird der Rückgabewert in der Variablen `m` gespeichert. (Falls eine Funktion keine `return`-Anweisung enthält, gibt sie automatisch den Wert `None` zurück.)

Verdeutlichen wir uns gleich an dieser Stelle den Unterschied zwischen Parametern und Argumenten. Ein *Parameter* (oder formaler Parameter) ist jener Teil der Funktionsdefinition, der angibt, welche Eingabewerte (Anzahl und evtl. auch Typ) eine Funkion erhalten kann. Ein *Argument* ist jener Wert, der beim Funktionsaufruf tatsächlich an die Funktion übergeben wird. (Argumente werden auch als „tatsächliche Parameter“ bezeichnet; wir bevorzugen aber den Begriff „Argument“, da diese Begriffe gerne etwas unscharf verwendet werden.)

Im Rahmen der Definition einer Funktion unterscheiden wir zwischen erforderlichen und optionalen Parametern:
- Ein *erforderlicher Parameter* muss beim Funktionsaufruf zwingend einen Wert erhalten.
- Ein *optionaler Parameter* kann beim Funktionsaufruf weggelassen werden; in diesem Fall erhält er den in der Funktionsdefinition vorgegebenen Standardwert.

Hier implementieren wir eine Exponentialfunktion; der Parameter `exponent` ist erforderlich, `base` ist hingegen optional und hat den Standardwert 2.71828.

```python
def exp(exponent, base=2.71828):
    return base ** exponent

print(exp(4))  # wird zu exp(4, 2.71828)
print(exp(4, 10))
```


Im Rahmen des Aufrufs einer Funktion unterscheiden wir zwischen positionsbezogenen und Schlüsselwortargumenten.
- Ein *positionsbezogenes Argument* wird beim Funktionsaufruf entsprechend seiner Position einem Parameter zugeordnet.
- Ein *Schlüsselwortargument* wird beim Funktionsaufruf entsprechend seines Namens einem Parameter zugeordnet.

```python
print(exp(4, 10))                # 4 -> exponent, 10 -> base
print(exp(base=10, exponent=4))  # 10 -> base, 4 -> exponent
```

Wir weisen insbesondere auf den Unterschied zwischen einem optionalen Parameter und einem Schlüsselwortargument hin: Obwohl sie ähnlich aussehen, bewirken sie doch ganz Unterschiedliches!


## Fallunterscheidungen und Verzweigungen

Eine `if`-Anweisung führt ihren Anweisungskörper aus, falls eine Bedingung eintritt:

```python
n = -1

if n < 0:
    print(n, "ist negativ")
```

Eine Fallunterscheidung wird folgendermaßen erzeugt:
- Schlüsselwort `if`
- Bedingung, ein Ausdruck, der einen Booleschen Wert ergibt
- Doppelpunkt
- Anweisungskörper eingerückt

An einen `if`-Block kann sich ein `else`-Block unmittelbar anschließen, dessen Anweisungen ausgeführt werden, falls die Bedingung nicht zutrifft:

```python
n = 5

if n < 0:
    print(n, "ist negativ")
else:
    print(n, "ist positiv")
```

Mehr als zwei Fallunterscheidungen lassen sich mithilfe von `elif`-Blöcken implementieren:

```python
n = 0

if n < 0:
    print(n, "ist negativ")
elif n > 0:
    print(n, "ist positiv")
else:
    print(n, "ist null")
```

## Schleifen

## `while`-Schleifen

Eine `while`-Schleife führt einen Codeblock solange aus, wie eine Bedingung wahr ist. Diese Schleife wird folgendermaßen erzeigt:
- Schlüsselwort `while`
- Bedingung, ein Ausdruck, der einen Booleschen Wert ergibt
- Doppelpunkt
- Anweisungskörper eingerückt

Die Syntax ist also recht ähnlich zur `if`-Anweisung, mit dem Unterschied, dass letztere nur einmal ausgeführt wird.

```python
i = 2

while i < 1000:
    i = i * 2
    print("Neuer Wert:", i)
```

Die Bedingung ist üblicherweise so gestaltet, dass sie irgendwann falsch wird, z.B. indem sie eine Variable überprüft, die im Schleifenkörper verändert wird. Allerdings können auch unendliche Schleifen programmiert werden:

```python
i = 1
while True:
    print("Ich höre niemals auf!", i)
    i += 1
```

(Um diese Schleife zu unterbrechen, müssen wir mittels `Strg+C` nachhelfen!)


## `for`-Schleifen

Die `for`-Schleife in Python ist eine sog. *iterative Schleife*: Sie führt ihren Anweisungskörper für jedes Element einer Sequenz aus und kann im Schleifenkörper auf das aktuelle Element zugreifen. Diese Schleife wird folgendermaßen definiert:
- Schlüsselwort `for`
- Schleifenvariable
- Schlüsselwort `in`
- iterierbares Objekt (z.B. String oder Liste)
- Anweisungskörper eingerückt

```python
for letter in "Python":
    print(letter)
```

Die obige Schleife wird sechsmal durchlaufen, und die Schleifenvariable `letter` enthält nacheinander die Werte `"P"`, `"y"`, `"t"`, `"h"`, `"o"` und `"n"`.

In anderen Programmiersprachen wird `for` of als *Zählschleife* verwendet, bei der die Anzahl an Wiederholungen bereits zu Beginn feststeht. Dies lässt sich in Python mithilfe von `range()` bewerkstelligen, die Zahlen in einem bestimmten Bereich erzeugt. (Für Experten: Hier handelt es sich um keine Funktion, sondern um einen Datentyp; dies macht für unsere Zwecke aber keinen Unterschied.)

Je nach Anzahl der Argumente liefert `range()`
- eine Zahlenfolge von 0 bis zu einem Endwert (exklusive)
  ```python
  for i in range(10):
      print(i)  #> 0, 1, …, 9
  ```
- eine Zahlenfolge von einem Startwert (inklusive) bis zu einem Endwert (exklusive)
  ```python
  for i in range(2, 7):
      print(i)  #> 2, 3, 4, 5, 6
  ```
- eine Zahlenfolge von Start- zu Endwert mit einer Schrittweite
  ```python
  for i in range(20, 3, -3):
      print(i)  #> 20, 17, 14, 11, 8, 5
  ```


## Modularisierung

Modularisierung erlaubt es, Programmcode in mehrere Dateien aufzuteilen, von denen jede thematisch zusammenhängende Funktionen und Datentypen enthält. Dadurch lässt sich Code wiederverwenden, da eine Funktion nur einmal implementiert werden muss und dann von beliebig vielen anderen Programmen durch *Einbinden* verwendet werden kann.

Ein Beispiel: In Python sind wichtige mathematische Funktionen in das Modul `math` ausgelagert, welches ein Bestandteil der Standardbibliothek ist. (Module der Standardbibliothek sind in jeder Python-Installation verfügbar; andere Module müssen eigens installiert werden, beispielsweise über das Programm `pip`).

Module werden über das Schlüsselwort `import` eingebunden. Im einfachsten Fall wird ein *Namensraum* mit dem gleichen Namen wie das Modul erstellt, innerhalb dessen die Funktionen des Moduls bekannt sind. Auf die Funktion wird dann durch Nennung des Namensraum, gefolgt von einem Punkt und dem eigentlichen Funktionsnamen zugegriffen:

```python
import math

math.factorial(7)
```

Es gibt aber noch weitere Varianten des Einbindens:
- mit Umbenennung des Namensraum
  ```python
  import math as mathematik

  mathematik.log2(65536)
  ```
- mit Einbindung ausgewählter Funktionen in den globalen Namensraum
  ```python
  from math import sqrt

  sqrt(16)
  ```

Aus Gründen der Übersicht ist es üblich, sämtliche `import`-Anweisungen an den Beginn einer Quellcodedatei zu stellen.



# Datenstrukturen

## Listen

Listen gehören zu den sequenziellen Datentypen, welche allgemein mehrere gleichartige oder verschiedene Elemente verwalten. Eine Liste wird erzeugt, indem sämtliche Elemente in eckigen Klammern angeführt und durch Kommas getrennt werden. Hier definieren wir eine Liste mit den ersten acht Primzahlen:

```python
primes = [2, 3, 5, 7, 11, 13, 17, 19]
primes
```

In der ersten Übungseinheit haben wir bereits anhand von Strings gelernt, wie man auf einzelne Elemente mittels Indizierung oder Slicing zugreifen kann:

```python
primes[3]
primes[2:5]
```

Die Liste zählt zu den veränderbaren Datentypen, daher können ihre Elemente überschrieben werden:

```python
primes[3] = 100
primes
```

Die Funktion `len()` list die Länge einer Liste aus.

```python
len(primes)
```

Listen können außerdem beliebig tief verschachtelt werden („Liste von Listen“)

```python
nested_list = [
    [1, 2, 3],
    [4, 5],
    [6, 7, 8, 9]
]
nested_list
```

Auch auf verschachtelte Listen kann man über Indizierung oder Slicing zugreifen:

```python
nested_list[1]
nested_list[1][2]
nested_list[2][2:6]
```

Da es sich bei Listen um Objekte handelt (mehr zu Objekten in der dritten Kurseinheit), besitzen sie besondere Funktionen (sog. *Methoden*), über die sie manipuliert werden können. Eine Methode wird ähnlich wie eine Funktion aufgerufen; da sie sich aber auf ein bestimmtes Objekt bezieht, lautet die Syntax folgendermaßen
- Objektname (Variable)
- Punkt
- Methodenname
- linke Klammer usw. (s. Elemente des Funktionsaufrufs)

Listen besitzen u.a. folgende Methoden (überprüfen Sie nach jedem Methodenaufruf den Inhalt der Variable `bases`, um die Auswirkungen der Methode nachzuvollziehen!):

```python
bases = ["A", "G", "X", "Y"]
bases.append("Z")         # hängt ein Element an die Liste an
del bases[2]              # löscht Element mit Index 2
l = bases.pop()           # entfernt das letzte Element und gibt es zurück
bases.extend(["T", "U"])  # hängt Elemente einer anderen Liste an
bases.insert(1, "C")      # fügt das zweite Argument an der Position ein,
                          # die durch das erste Argument gegeben ist
bases.reverse()           # kehrt Reihenfolge der Elemente um
bases.clear()             # löscht alle Elemente
```


## Tupel

Tupel sind im Gegensatz zu Listen unveränderlich und besitzen daher keine Methoden (wie `append()`), die ihren Inhalt verändern würden. Im Folgenden probieren wir einige Funktionen und Methoden auf Tupel aus:

```python
bases = ("A", "C", "C", "G", "T")
"A" in bases      # überprüfe, ob der Tupel ein Element enthält
bases[1:3]        # Slicing
len(bases)        # Anzahl der Elemente
min(bases)        # kleinstes Element
max(bases)        # größtes Element
bases.count("C")  # wie oft kommt ein Element vor?
bases.index("G")  # welchen Index hat ein Element?
```


## Mengen

Ein weiterer sequentieller Datentyp ist die *Menge* (engl. *set*), mittels dessen Mengen im mathematischen Sinn dargestellt werden können. Mengen sind ungeordnet und existieren in einer veränderlichen (`set`) und nicht veränderlichen (`frozenset`) Variante.

Mengen werde erzeugt, indem ihre Elemente in geschweifte Klammern eingeschlossen werden. Alternativ kann ein anderer Sequenztyp (z.B. eine Liste) in eine Menge umgewandelt werden:

```python
A = {2, 4, 5}        # Variante 1: direkt
A1 = set([2, 4, 5])  # Variante 2: Konvertierung einer Liste

A == A1              # beide Mengen sind identisch
```

Neben dem Hinzufügen und Entfernen von Elementen unterstützen Mengen auch die klassischen Mengenoperatoren:

```python
B = {1, 2, 3, 4, 5}
B.discard(5)  # Element entfernen, falls vorhanden
B.add(7)      # Element hinzufügen

A | B  # Vereinigung (ODER)
A & B  # Schnitt (UND)
A - B  # Differenz
A ^ B  # symmetrische Differenz (XOR)
```


## Dictionaries

Ein *Dictionary* (kurz dict; der deutsche Begriff „Wörterbuch“ ist hingegen nicht gebräuchlich) ist eine ungeordnete Sammlung von Schlüssel-Wert-Paaren. Die Schlüssel müssen einzigartig sein. Ein dict wird erzeugt, indem diese Paare in geschweifte Klammern eingeschlossen werden:

```python
atom_names = {"C": "carbon",
              "H": "hydrogen",
              "N": "nitrogen"}
atom_names
```

Alternativ greifen wir auf die eingebaute Funktion `dict()` zurück, in der die Schlüssel-Wert-Paare als optionale Argumente spezifiziert werden:

```python
atom_names = dict(C="carbon",
                  H="hydrogen",
                  N="nitrogen")
atom_names
```

Typische Operationen auf dicts sind Einfügen, Löschen, sowie – wohl am wichtigsten – Suchen:

```python
atom_names["C"]             # suche den Wert des Schlüssels "S"
atom_names["O"] = "oxygen"  # füge ein neues Paar ein
del atom_names["H"]         # lösche das Paar mit dem Schlüssel "H"
```

Eine wichtige Methode eines dicts ist `items()`, welches eine Folge aller Schlüssel-Wert-Paare liefert und so die Verwendung eines dicts in einer `for`-Schleife erlautbt

```python
for key, value in atom_names.items():
    print(key, value)
```


# Reguläre Ausdrücke

Python bietet im Modul `re` eine umfangreiche Implementierung von Funktionen zur Verwendung regulärer Ausdrücke. Wie uns bereits aus dem Bash-Kurs bekannt ist, sind *reguläre Ausdrücke* (regex) Zeichenketten, die ein Suchmuster definieren und somit Mengen anderer Zeichenketten beschreiben.

Das Modul `re` ist sehr umfangreich und bietet eine Fülle an Varianten an, wie reguläre Ausdrücke vorbereitet und verwendet werden können. Wir beschränken uns auf einige wenige Funktionen und verweisen ansonsten auf die [Dokumentation](https://docs.python.org/3/library/re.html). `findall()` beispielsweise sucht nach allen nicht-überlappenden Vorkommen eines regulären Ausdrucks (erstes Argument) in einem Suchstring (zweites Argument) und gibt alle Treffer als Liste zurück.

```python
import re

re.findall(r".ython", "Python oder Jython")
```

Es empfiehlt sich, den regulären Ausdruck mittels eines Raw-Strings zu definieren (ein String, vor dessen erstem Anführungszeichen ein `r` steht), um den Backslash ohne Escaping verwenden zu können.

Die Syntax für reguläre Ausdrücke in Python ist ähnlich der Syntax in Bash:

- Der Punkt `.` bezeichnet ein *beliebiges Zeichen* (s. Beispiel oben). Ein „echter“ Punkt
- *Zeichenklassen* werden gebildet, indem die gültigen Zeichen in eckige Klammern gesetzt werden.
  ```python
  re.findall(r"H[au]nd", "Hand Hund")
  ```
  Auch die Angabe eines Zeichenbereichs sowie der Ausschluss von Zeichen sind möglich:
  ```python
  re.findall(r"H[a-t]nne", "Hanne Henne Hunne")  # alle Zeichen von a bis t
  re.findall(r"H[^e]nne", "Hanne Henne Hunne")   # alle Zeichen außer e
  ```
  Python kennt außerdem eine Reihe von vordefinierten Zeichenklassen, z.B. `\d` (alle Ziffern des Dezinalsystems) und `\w` (alle alphanumerischen Zeichen inkl. Unterstrich).
- *Quantoren* nach einem Zeichen geben an, wie oft diese Zeichen auftreten darf.
  ```python
  target = "bt bat baat baaat baaaat baaaaaaaaat"
  re.findall(r"ba*t", target)     # beliebig oft
  re.findall(r"ba+t", target)     # mindestens einmal
  re.findall(r"ba?t", target)     # keinmal oder einmal
  re.findall(r"ba{3}t", target)   # genau 3-mal
  re.findall(r"ba{2,}t", target)  # 2-mal oder öfter
  re.findall(r"ba{,3}t", target)  # höchstens 3-mal
  re.findall(r"ba{2,4}t", target) # 2- bis 4-mal
  ```
- *Alternativen* suchen nach zwei verschiedenen Zeichenketten.
  ```python
  re.findall(r"eins|zwei", "eins zwei drei")
  ```
- *Anker* legen fest, dass ein regulärer Ausdruck nur am Anfang oder Ende eines Strings gefunden werden darf:
  ```python
  re.findall(r"^a..", "auf dem Haus")  # Verankerung am Beginn
  re.findall(r"a..$", "auf dem Haus")  # Verankerung am Ende
  ```
- *Gruppen* werden durch runde Klammern um mehrere Zeichen gebildet. Gruppen bilden Einheiten, die z.B. mit einem Quantor versehen werden können. Außerdem können wir über Gruppen bestimmte Teile eines Treffers extrahieren:
  ```python
  re.findall(r"Tier: (.+) (.+)", "Tier: Mus musculus")
  ```






# Aufgaben

Anhand von `print()` können wir viele Varianten von Funktionsaufrufen erproben. So akzeptiert `print()` eine beliebige Anzahl von sog. positionsbezogenen Argumenten.

print() is a nice starting point since it allows us to illustrate all possibilities of specifying arguments for a function. print() accepts an arbitrary number of positional arguments, all of which are printed:

```python
print(1, "plus", 2, "equals", 3)
```

print() also accepts several keyword arguments that modify its behavior. For instance, sep defines the separator between the printed arguments, and end specifies the character that is printed following all positional arguments.

```python
print(1, "plus", 2, "equals", 3, sep="_", end=": ")
```

You may be curious about the default separator and end character. As evident from above, Python usually separates printed arguments by spaces and ends each line with a newline character.




format strings


repeat
break and continue Statements, and else Clauses on Loops
pass Statements
functions with an arbitrary number of arguments (*args and **kwargs)
