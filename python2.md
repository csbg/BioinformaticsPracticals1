# Kurseinheit 2

## Inhalt

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
- [Aufgaben](#aufgaben)



## Kontrollfluss

Python führt die Anweisungen in einem Skript der Reihe nach aus.

```python
a = 4
b = 9
c = 5
mean = (a + b + c) / 3
print(mean)
```

Es ist offensichtlich, dass dieser grundlegende Kontrollfluss nur einfachste Berechnungen ermöglicht. Daher stellt Python eine Reihe von Möglichkeiten zur Verfügung, um den Kontrollfluss zu modifizieren.

*Hinweis:* Alle folgenden Beispiele können zwar im Python-Interpreter eingegeben werden. Gerade bei zusammengesetzten Anweisungen kann es aber umständlich sein, mehrere eingerückte Anweisungen im Anweisungskörper einzugeben. Daher ist es manchmal einfacher, ein kleines Python-Skript mit den Anweisungen eines Beispiels zu erstellen (z.B. `func_test.py`) und dieses Skript dann aufzurufen (z.B. `python func_test.py`).


### Funktionen

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

Eine Funktion kann einen oder mehrere Werte mittels der `return`-Anweisung zurückgeben. Im obigen Beispiel wird der Rückgabewert in der Variable `m` gespeichert.
- Falls eine Funktion *mehrere* Werte zurückgeben soll, lautet die Syntax `return wert, wert2, wert3` usw. – die einzelnen Rückgabewerte werden durch Kommas getrennt.
- Falls eine Funktion *keine* `return`-Anweisung enthält, gibt sie automatisch den Wert `None` zurück.

Verdeutlichen wir uns gleich an dieser Stelle den Unterschied zwischen Parametern und Argumenten.
- Ein *Parameter* (oder formaler Parameter) ist jener Teil der Funktionsdefinition, der angibt, welche Eingabewerte (Anzahl und evtl. auch Typ) eine Funkion erhalten kann.
- Ein *Argument* ist jener Wert, der beim Funktionsaufruf tatsächlich an die Funktion übergeben wird. (Argumente werden auch als „tatsächliche Parameter“ bezeichnet; wir bevorzugen aber den Begriff „Argument“, um eine eindeutige Unterscheidung zu den Parametern zu treffen.)

Im Rahmen der Definition einer Funktion unterscheiden wir zwischen erforderlichen und optionalen Parametern:
- Ein *erforderlicher Parameter* muss beim Funktionsaufruf zwingend einen Wert erhalten.
- Ein *optionaler Parameter* kann beim Funktionsaufruf weggelassen werden. In diesem Fall erhält er den in der Funktionsdefinition vorgegebenen Standardwert.

Unten implementieren wir eine Exponentialfunktion; der Parameter `exponent` ist erforderlich, `base` ist hingegen optional und hat den Standardwert 2.71828.

```python
def exp(exponent, base=2.71828):
    return base ** exponent

print(exp(4))  # wird zu exp(4, 2.71828)
print(exp(4, 10))
```


Im Rahmen des Aufrufs einer Funktion unterscheiden wir zwischen positionsbezogenen und Schlüsselwortargumenten.
- Ein *positionsbezogenes Argument* wird beim Funktionsaufruf entsprechend seiner Stelle innerhalb der Klammern einem Parameter zugeordnet.
- Ein *Schlüsselwortargument* wird beim Funktionsaufruf entsprechend seines Namens einem Parameter zugeordnet.

```python
print(exp(4, 10))                # 4 -> exponent, 10 -> base
print(exp(base=10, exponent=4))  # 10 -> base, 4 -> exponent
```

Wir weisen insbesondere auf den Unterschied zwischen einem optionalen Parameter und einem Schlüsselwortargument hin: Obwohl sie ähnlich aussehen, bewirken sie doch ganz Unterschiedliches!


### Fallunterscheidungen und Verzweigungen

Eine `if`-Anweisung führt ihren Anweisungskörper aus, falls die im Anwendungskopf abgefragte Bedingung eintritt:

```python
n = -1

if n < 0:
    print(n, "is negative")
```

Eine Fallunterscheidung wird folgendermaßen erzeugt:
- Schlüsselwort `if`
- Bedingung (ein Ausdruck, der einen Booleschen Wert ergibt)
- Doppelpunkt
- Anweisungskörper eingerückt

An einen `if`-Block kann sich ein `else`-Block unmittelbar anschließen. Anweisungen innerhalb dieses Blocks werden ausgeführt, falls die Bedingung *nicht wahr* ist:

```python
n = 5

if n < 0:
    print(n, "is negative")
else:
    print(n, "is positive")
```

Mehr als zwei Fallunterscheidungen lassen sich mithilfe von `elif`-Blöcken implementieren. Wir speichern den folgenden Funktion in der Datei `test_sign.py` und führen die Datei mit Python aus.

```python
def test_sign(n):
    if n < 0:
        return f"{n} is negative"
    elif n > 0:
        return f"{n} is positive"
    else:
        return f"{n} is zero"

print(test_sign(102))
print(test_sign(-12))
print(test_sign(0))
```

`f" ... "` ist ein sog. *f-String* (genauer, ein *formatiertes Stringliteral*). Ein f-String verhält sich grundsätzlich wie ein normaler String. Allerdings wird jeder Teil in geschwungenen Klammern als Anweisung ausgeführt, und das Ergebnis der jeweiligen Anweisung wird in den String eingefügt:

```python
f"An addition: {2+3}"
```



### Schleifen

#### `while`-Schleifen

Eine `while`-Schleife führt einen Codeblock solange aus, wie eine Bedingung wahr ist. Diese Schleife wird folgendermaßen erzeugt:
- Schlüsselwort `while`
- Bedingung (ein Ausdruck, der einen Booleschen Wert ergibt)
- Doppelpunkt
- Anweisungskörper eingerückt

Die Syntax ist also recht ähnlich zur `if`-Anweisung, mit dem Unterschied, dass letztere nur einmal ausgeführt wird.

```python
i = 2

while i < 1000:
    i = i * 2
    print("Neuer Wert:", i)
```

Die Bedingung ist üblicherweise so gestaltet, dass sie nach ein paar Aufrufen des Schleifenkörpers falsch wird. In der obigen Schleife wird z.B. die Variable `i` überprüft, die bei jedem Durchlauf des Schleifenkörpers einen neuen Wert erhält. Allerdings können auch unendliche Schleifen programmiert werden:

```python
while True:
    print("Ich höre niemals auf!", i)
```

(Um diese Schleife zu unterbrechen, müssen wir mittels `Strg+C` nachhelfen!)

Unendliche Schleifen können dennoch nützlich sein. Im folgenden Beispiel implementieren wir ein Ratespiel und speichern den Code in der Datei `guessing_game.py`.

```python
correct_answer = 5

while True:
    answer = input("Guess a number between 0 and 10: ")
    if int(answer) == correct_answer:
        break
    else:
        print("Wrong, try again.")

print("Correct!")
```

Die `while`-Schleife wird so lange ausgeführt, bis die gesuchte Zahl erraten wurde. Die `break`-Anweisung beendet in diesem Fall die Schleife und lässt den Kontrollfluss zur ersten Anweisung nach dem Schleifenkörper springen.


#### `for`-Schleifen

Die `for`-Schleife in Python ist eine sog. *iterative Schleife*: Sie führt ihren Anweisungskörper für jedes Element einer Sequenz aus und kann im Schleifenkörper auf das aktuelle Element zugreifen. Die `for`-Schleife wird folgendermaßen gebildet:
- Schlüsselwort `for`
- Schleifenvariable
- Schlüsselwort `in`
- iterierbares Objekt (z.B. String oder Liste)
- Doppelpunkt
- Anweisungskörper eingerückt

```python
for letter in "Python":
    print(letter)
```

Die obige Schleife wird sechsmal durchlaufen, und die Schleifenvariable `letter` enthält nacheinander die einzelnen Zeichen des Strings (also die Werte `"P"`, `"y"`, `"t"`, `"h"`, `"o"` und `"n"`).

In anderen Programmiersprachen wird `for` oft als *Zählschleife* verwendet, bei der die Anzahl an Wiederholungen bereits zu Beginn feststeht. Dies lässt sich in Python mithilfe von `range()` bewerkstelligen, wodurch eine Folge von Zahlen erzeugt wird. (Für Experten: Hier handelt es sich um keine Funktion, sondern um einen Datentyp; dies macht für unsere Zwecke aber keinen Unterschied.)

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
- eine Zahlenfolge von Start- zu Endwert mit einer gegebenen Differenz zwischen den Folgegliedern:
  ```python
  for i in range(20, 3, -3):
      print(i)  #> 20, 17, 14, 11, 8, 5
  ```


### Modularisierung

Modularisierung erlaubt es, Programmcode in mehrere Dateien aufzuteilen, von denen jede thematisch zusammenhängende Funktionen und Datentypen enthält. Dadurch lässt sich Code wiederverwenden, da eine Funktion
- nur einmal in einem *Modul* (auch *Paket* genannt) implementiert werden muss
- und dann von beliebig vielen anderen Programmen durch *Einbinden* dieses Moduls verwendet werden kann.

Ein Beispiel: In Python sind wichtige mathematische Funktionen in das Modul `math` ausgelagert, welches ein Bestandteil der Standardbibliothek ist. (Module der *Standardbibliothek* sind in jeder Python-Installation verfügbar; andere Module müssen eigens installiert werden.)

Wir binden ein Modul mit dem Schlüsselwort `import` ein. Dies erzeugt einen *Namensraum* mit dem gleichen Namen wie das Modul und erlaubt uns, folgendermaßen auf eine Funktion zuzugreifen:
- Namensraum
- Punkt
- Funktionsname

```python
import math

math.factorial(7)
```

Wir können auch eine beliebige Bezeichnung für den Namensraum wählen:

```python
import math as mathematik

mathematik.factorial(7)
```

Darüber hinaus lassen sich einzelne (oder sogar alle Funktionen) in den sog. *globalen Namensraum* importieren:

```python
from math import factorial

factorial(7)
```

Aus Gründen der Übersicht ist es üblich, sämtliche `import`-Anweisungen an den Beginn einer Quellcodedatei zu stellen.



## Datenstrukturen

### Listen

Listen gehören zu den *sequenziellen Datentypen*, welche mehrere gleichartige oder verschiedene Elemente in einer Struktur zusammenfassen. Eine Liste wird erzeugt, indem sämtliche Elemente in eckigen Klammern angeführt und durch Kommas getrennt werden. Hier definieren wir eine Liste mit den ersten acht Primzahlen:

```python
primes = [2, 3, 5, 7, 11, 13, 17, 19]
primes
```

In Kurseinheit 1 haben wir bereits anhand von Strings gelernt, wie man auf einzelne Elemente mittels Indizierung oder Slicing zugreifen kann:

```python
primes[3]
primes[2:5]
```

Die Liste zählt zu den *veränderbaren Datentypen*, daher können einzelne Elemente überschrieben werden:

```python
primes[3] = 100
primes
```

Die eingebaute Funktion `len()` berechnet die Anzahl der Elemente einer Liste:

```python
len(primes)
```

Listen können außerdem beliebig tief verschachtelt werden („Liste von Listen“):

```python
nested_list = [
    [1, 2, 3],
    [4, 5],
    [6, 7, 8, 9]
]
nested_list
```

Auch auf Elemente von verschachtelten Listen kann man über Indizierung oder Slicing zugreifen:

```python
nested_list[1]
nested_list[1][2]
nested_list[2][2:6]
```

Da es sich bei Listen um Objekte handelt (mehr zu Objekten in Kurseinheit 3), besitzen sie besondere Funktionen (sog. *Methoden*), über die sie manipuliert werden können. Eine Methode wird ähnlich wie eine Funktion aufgerufen; da sie sich aber auf ein bestimmtes Objekt bezieht, lautet die Syntax folgendermaßen:
- Objektname (Variable)
- Punkt
- Methodenname
- linke Klammer usw. (s. Elemente des Funktionsaufrufs)

Listen besitzen u.a. folgende Methoden (nach jedem Methodenaufruf überprüfen wir den Inhalt der Variable `bases`, um die Auswirkungen der Methode nachzuvollziehen!):

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


### Tupel

Tupel sind im Gegensatz zu Listen *unveränderlich* und besitzen daher keine Methoden, die ihren Inhalt verändern würden (wie `append()`). Im Folgenden probieren wir einige Funktionen und Methoden auf Tupel aus:

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


### Mengen

Die *Menge* ist ein sequentieller Datentyp, der eine Menge im mathematischen Sinn darstellt. Mengen sind *ungeordnet* und existieren in einer veränderlichen (`set`) und nicht veränderlichen (`frozenset`) Variante. Wir erzeugen eine Menge, indem wir ihre Elemente innerhalb geschweifter Klammern nennen. Alternativ können wir einen anderen Sequenztyp (z.B. eine Liste) in eine Menge umwandeln:

```python
A = {2, 4, 5}        # Variante 1: direkt
A1 = set([2, 4, 5])  # Variante 2: Konvertierung einer Liste

A == A1              # sind beide Mengen identisch?
```

Neben dem Hinzufügen und Entfernen von Elementen unterstützen Mengen auch die klassischen Mengenoperatoren:

```python
B = {1, 2, 3, 4, 5}
B.discard(5)  # Element entfernen, falls vorhanden
B.add(7)      # Element hinzufügen
B.add(7)      # keine Änderung – jedes Element kann nur einmal vorkommen!

A | B  # Vereinigung (ODER)
A & B  # Schnitt (UND)
A - B  # Differenz
A ^ B  # symmetrische Differenz (XOR)
```


### Dictionaries

Ein Dictionary (kurz Dict; der deutsche Begriff „Wörterbuch“ ist hingegen nicht gebräuchlich) ist eine ungeordnete Sammlung von *Schlüssel-Wert-Paaren*. Die Schlüssel müssen einzigartig sein. Wir erzeugen ein Dict,
- entweder, indem wir Schlüssel und Wert durch einen Doppelpunkt trennen und alle Schlüssel-Wert-Paare durch Kommas getrennt in geschweifte Klammern einschließen:
  ```python
  atom_names = {"C": "carbon",
                "H": "hydrogen",
                "N": "nitrogen"}
  atom_names
  ```

- oder, indem wir die eingebaute Funktion `dict()` verwenden und die Schlüssel-Wert-Paare als optionale Argumente angeben:
  ```python
  atom_names = dict(C="carbon",
                    H="hydrogen",
                    N="nitrogen")
  atom_names
  ```

Typische Operationen auf Dicts sind Einfügen, Löschen, sowie – wohl am wichtigsten – Suchen:

```python
atom_names["C"]             # suche den Wert des Schlüssels "S"
atom_names["O"] = "oxygen"  # füge ein neues Paar ein
del atom_names["H"]         # lösche das Paar mit dem Schlüssel "H"
```

Die Dict-Methode `items()` liefert eine Folge aller Schlüssel-Wert-Paare, wodurch wir ein Dict in einer `for`-Schleife verwenden können:

```python
for key, value in atom_names.items():
    print(key, value)
```



## Aufgaben

Speichern Sie alle Dateien, die Sie in Kurseinheit 2 erstellen müssen, im Ordner `python2` in Ihrem Home-Verzeichnis.



#### Aufgabe 2.1 (3 P)

Diese Aufgabe haben Sie erfolgreich gelöst, wenn Sie die Code-Beispiele dieser Kurseinheit durchgearbeitet haben. Achten Sie darauf, dass sich im Ordner `python2` die folgenden Dateien befinden, die Sie im Rahmen der Übungen erstellt haben:
- `test_sign.py`
- `guessing_game.py`



#### Aufgabe 2.2 (3 P)

Implementieren Sie eine Funktion `get_charge`, die überprüft, ob eine Aminosäure positiv geladen (z.B. Arginin), negativ geladen (z.B. Aspartat), oder neutral ist (z.B. Valin).
- Die Funktion soll mit einem Argument aufgerufen werden, das die Aminosäure als Ein-Buchstaben-Code angibt. Nur die 21 eukaryotischen proteinogenen Aminosäuren sollen berücksichtigt werden.
- Je nach Ladung soll die Funktion einen der folgenden Strings zurückgeben: `"positive"`, `"negative"`, oder `"neutral"`.
- Falls keine gültige Aminosäure angegeben wurde, soll die Funktion `"invalid input"` zurückgeben.

Speichern Sie diese Funktion in der Datei `aufgabe_2_2.py`.

Sie können die korrekte Funktionalität Ihres Programm anhand der folgenden Beispiele testen:
```python
print(get_charge("D"))    #> "negative"
print(get_charge("F"))    #> "neutral"
print(get_charge("H"))    #> "positive"
print(get_charge("foo"))  #> "invalid input"
```


#### Aufgabe 2.3 (3 P)

Schreiben Sie eine Funktion `count_bases`, die die Anzahl an Purin- und Pyrimidinbasen in einer DNA- oder RNA-Sequenz zählt.
- Der erste Parameter `sequences` ist erforderlich. Er wird eine Liste mit beliebig vielen Strings erhalten, von denen jeder eine Sequenz darstellt.
- Der zweite Parameter `purine` ist optional und soll den Standardwert `True` haben. Wenn `purine` wahr ist, soll die Funktion die Anzahl an Purinen berechnen, ansonsten die Anzahl an Pyrimidinen.
- Die Funktion soll eine einzige Zahl zurückgeben, d.h. die Summe aller Purine oder Pyrimidine in *allen* Sequenzen.

Speichern Sie diese Funktion in der Datei `aufgabe_2_3.py`.

Sie können die korrekte Funktionalität Ihres Programm anhand der folgenden Beispiele testen:
```python
print(count_bases(["ACCGGGTTTT", "TTAAAGGGGCCCCC"], True))         #> 11
print(count_bases(["AA", "UUUU", "G", "CCCCCCCC"], purines=False)) #> 12
```


#### Aufgabe 2.4 (3 P)

Wenn Sie regelmäßig programmieren, werden Sie unweigerlich an den Punkt kommen, an dem Sie fremde Pakete verwenden und in der jeweiligen Dokumentation nachschlagen müssen. Dies werden wir in dieser Aufgabe üben.

Schreiben Sie eine Funktion `make_sequence`, die eine zufällige DNA- oder RNA-Sequenz gegebener Länge erzeugt und berechnet, wie oft eine bestimmte Base in dieser Sequenz vorkommt.
- Der erste (erforderliche) Parameter bestimmt die Länge der erzeugten Sequenz.
- Der zweite (optionale) Parameter `rna` legt fest, ob eine DNA- oder RNA-Sequenz erzeugt werden soll. Dieser Parameter hat einen Booleschen Typ und soll standardmäßig falsch sein.
- Der dritte (optionale) Parameter `count_base` gibt an, welche Base gezählt werden soll (Vorgabe: `"A"`).
- Die Funktion soll zwei Werte zurückgeben: (1) Die erzeugte Sequenz als String, und (2) die Anzahl der gezählten Basen.

Da die Sequenz zufällig erzeugt werden soll, werden Sie auf die Funktion `choices()` des Moduls `random` zurückgreifen müssen. Dieses Modul ist Teil der Standardbibliothek – bitte suchen Sie in der entsprechenden [Dokumentation](https://docs.python.org/3/library/index.html), wie die Funktion zu verwenden ist. Außerdem werden Sie die String-Methode `join()` brauchen, um den Rückgabewert von `choices()` in einen String umzuwandeln. Da Strings ein eingebauter Datentyp sind, sind sie ebenfalls in der Standardbibliothek dokumentiert.

Speichern Sie diese Funktion in der Datei `aufgabe_2_4.py`.

Sie können die korrekte Funktionalität Ihres Programm anhand der folgenden Beispiele testen:
```python
random.seed(42)
print(make_sequence(20, False, "A"))
#> ('GACAGGTACAAGAAGGAGTA', 9)

print(make_sequence(15, rna=True, count_base="C"))
#> ('UGCAUCAAUGUGGUC', 3)
```



#### Aufgabe 2.5 (3 P)

In dieser Aufgabe entwerfen Sie ein Programm `aufgabe_2_5.py`, welches die Ähnlichkeit zweier Gewebeproben anhand ihrer exprimierten Gene berechnet.
- Das Programm soll zwei Kommandozeilenargumente `genes1` und `genes2` verarbeiten.
- Jedes Argument gibt die in einer Probe exprimierten Gene an; einzelne Gene sind dabei durch einen Strichpunkt getrennt (z.B. `GENE1;GENE2;GENE3;GENE4`). Est ist egal, ob ein Gen einmal oder öfter genannt wird. (Zur Verarbeitung der Argumente werden Sie die `split()`-Methode der Strings brauchen – s. [Dokumentation](https://docs.python.org/3/library/index.html).)
- Das Programm soll drei Werte ausgeben, jeweils auf einer eigenen Zeile:
  1. Die Anzahl der in der ersten Probe detektierten Gene;
  2. die Anzahl der in der zweiten Probe detektierten Gene; und
  3. die Ähnlichkeit der beiden Proben, welche durch den Jaccard-Koeffizienten gemessen wird. (Der [Jaccard-Koeffizient](https://de.wikipedia.org/wiki/Jaccard-Koeffizient) zweier Mengen entspricht der Größe der Schnittmenge geteilt durch die Größe der Vereinigungsmenge.)

Sie können die korrekte Funktionalität Ihres Programm anhand der folgenden Beispiele testen:
```
$ python aufgabe_2_5.py RAB5C;PITX2;ZNF222;LMTK2;LMTK2 RAB5C;PABPC4;ZNF222;PKN1;PTMA
4
5
0.2857142857142857

$ python aufgabe_2_5.py THOC5;RAD23B;GPR31;PIRC85;PANO1 THOC5;ATP1A2;GPR31;THOC5
5
3
0.3333333333333333

$ python aufgabe_2_5.py RAD23B RAD23B
1
1
1.0
```
