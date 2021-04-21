# Kurseinheit 3

- [Objektorientierte Programmierung](#objektorientierte-programmierung)
  - [Attribute und der Konstruktor](#attribute-und-der-konstruktor)
  - [Methoden](#methoden)
- [Reguläre Ausdrücke](#reguläre-ausdrücke)
  - [Einführung](#einführung)
  - [Syntax](#syntax)
  - [Pattern-Objekte](#pattern-objekte)
- [Lesen und Schreiben von Dateien](#lesen-und-schreiben-von-dateien)
  - [Lesen einzelner Zeichen](#lesen-einzelner-zeichen)
  - [Zeilenweises Lesen](#zeilenweises-lesen)
  - [Schreiben](#schreiben)
  - [Kontextobjekte](#kontextobjekte)
- [Aufgaben](#aufgaben)



## Objektorientierte Programmierung

Die objektorientierte Programmierung (OOP) zählt zu den von Python unterstützten Programmierparadigmen. Wenngleich ein Kurs über OOP unzählige Stunden füllen könnte, beschränken wir uns hier auf eine pragmatische Herangehensweise und stellen uns ein Programm als eine Sammlung von *Objekten* vor, die miteinander kommunizieren. Jedes Objekt besitzt
- *Attribute*, die seinen Zustant beschreiben, und
- *Methoden*, die es erlauben, auf die Attribute zuzugreifen oder sie zu verändern.

Um ein Objekt erzeugen zu können, müssen wir zuerst eine Vorlage (*Klasse*) erstellen, die dann quasi als „Bauplan“ für einen beliebige Anzahl von Objekten verwendet werden kann. Das Erzeugen von Objekten nach der Vorlage einer Klasse heißt *Instanziierung*. Die aus einer Klasse erzeugten Objekte haben zwar die gleichen Attribute und Methoden, aber die Attribute speichern üblicherweise verschiedene Werte.

Um diese etwas abstrakten Konzepte zu veranschaulichen, werden wir eine Klasse implementieren, mit deren Hilfe wir Proteine beschreiben können. Unsere Implementierung bleibt zunächst sehr einfach. In den Aufgaben werden Sie aber Gelegenheit haben, die Klasse um zusätzliche Funktionalitäten zu erweitern.

Wir speichern alle Anweisungen dieses Abschnitts (also die Klassendefinition) in einer Datei `simple_protein.py`.


### Attribute und der Konstruktor

Ein Protein soll durch einen Namen und eine Aminosäuresequenz beschrieben werden. Daher benötigt eine Proteinklasse zumindest zwei Attribute:

```python
class Protein:
    def __init__(self, name, sequence):
        self.name = name
        self.sequence = sequence
```

Der obige Code definiert eine neue Klasse:
- Schlüsselwort `class`
- Klassenname (hier `Protein`)
- Doppelpunkt
- Anweisungskörper eingerückt (alle Anweisungen, die zur Klassendefinition gehören)

Wir entwerfen auch einen *Konstruktor* für die Klasse. Dabei handelt es sich um eine spezielle Methode (Funktion), die `__init__()` heißen muss und jedesmal aufgerufen wird, wenn ein Objekt erzeugt wird. Innerhalb des Konstruktors werden die Attribute `self.name` und `self.sequence` mit den an den Konstruktor übergebenen Werten initialisiert.

Wir erzeugen nun ein neues Objekt `ins_A` der Klasse `Protein`. Dazu rufen wir den Klassennamen wie eine Funktion auf und übergeben Werte an die Parameter des Konstruktors. Im Anschluss können wir auf die Attribute des Objekts `ins_A` zugreifen (Objektname – Punkt – Attributname).

```python
ins_A = Protein(name="insulin A chain", sequence="GIVEQCCTSICSLYQLENYCN")
print(ins_A.name)
print(ins_A.sequence)
print(type(ins_A))
```

Die eingebaute Funktion `type()` informiert uns darüber, von welcher Klasse `ins_A` abgeleitet wurde.

Es stellt sich aber noch eine Frage: Wir haben den Konstruktor mit drei erforderlichen Parametern definiert, aber nur mit zwei Argumenten aufgerufen, und diese wurden offensichtlich den Parametern `name` und `sequence` zugeordnet. Aber was ist mit dem Parameter `self`? Jede Methode (also auch der Konstruktor) muss als ersten Parameter einen Verweis auf das Objekt enthalten, von dem aus sie aufgerufen wird. Dieser Parameter wird *nur* bei der Definition der Methode angegeben (und per Konvention als `self` bezeichnet), *nicht* aber beim Aufruf der Methode (er wird von Python quasi „automatisch“ ergänzt).



### Methoden

Fügen wir zu unserer Klasse eine Methode hinzu (es reicht, wenn wir in der Datei `simple_protein.py` die Definition von `mutate` in den Klassenkörper entsprechend eingerückt einfügen):

```python
class Protein:
    def __init__(self, name, sequence):
        self.name = name
        self.sequence = sequence

    def mutate(self, pos, residue):
        self.sequence = self.sequence[:pos] + residue + self.sequence[pos+1:]
```

Die Methode `mutate()` erzeugt eine Punktmutation, indem sie die Aminosäure an der Stelle `pos` durch `residue` ersetzt.

```python
ins_A = Protein("insulin A chain", "GIVEQCCTSICSLYQLENYCN")
print(ins_A.sequence)

ins_A.mutate(2, "W")
print(ins_A.sequence)
```

`mutate()` funktioniert wie erwartet: Die Methode hat die Aminosäure mit Index 2 (also jene an der dritten Position – Valin) durch Tryptophan ersetzt.

In den folgenden beiden Abschnitten werden wir Beispiele für Klassen kennen lernen, die in der Python-Standardbibliothek implementiert sind.



## Reguläre Ausdrücke

### Einführung

Das Modul `re` (in der Standardbibliothek) implementiert Funktionen zur Verwendung regulärer Ausdrücke. Wie uns bereits aus dem Bash-Kurs bekannt ist, ist ein *regulärer Ausdruck* (regex) eine Zeichenkette, die ein Suchmuster definiert und somit Mengen anderer Zeichenketten beschreibt.

Das Modul `re` ist sehr umfangreich und bietet eine Fülle an Varianten an, wie reguläre Ausdrücke vorbereitet und verwendet werden können. Wir beschränken uns auf einige wenige Funktionen und verweisen ansonsten auf die [Dokumentation](https://docs.python.org/3/library/re.html) und den Artikel [Regular Expression HOWTO](https://docs.python.org/3/howto/regex.html).

`findall()` sucht nach allen nicht-überlappenden Vorkommen eines regulären Ausdrucks (erstes Argument) in einem Suchstring (zweites Argument) und gibt alle Treffer als Liste zurück.

```python
from re import findall

findall(r".ython", "Python oder Jython")
```

Es empfiehlt sich, den regulären Ausdruck mittels eines *Raw-Strings* zu definieren (ein String, vor dessen erstem Anführungszeichen ein `r` steht). Innerhalb eines Raw-Strings wird der Backslash *nicht* als Escape-Zeichen interpretiert, sondern (schlicht und einfach) als ein Backslash. Wir werden unten sehen, warum das für Regex nützlich ist.


### Syntax

Die Syntax für reguläre Ausdrücke in Python ähnelt jener in Bash:

- Der Punkt `.` bezeichnet ein *beliebiges Zeichen* (s. Beispiel oben).
- *Zeichenklassen* werden gebildet, indem die gültigen Zeichen in eckige Klammern gesetzt werden.
  ```python
  findall(r"H[au]nd", "Hand Hund Hend")
  ```
  Auch die Angabe eines Zeichenbereichs sowie der Ausschluss von Zeichen sind möglich:
  ```python
  findall(r"H[a-t]nne", "Hanne Henne Hunne")  # alle Zeichen von a bis t
  findall(r"H[^e]nne", "Hanne Henne Hunne")   # alle Zeichen außer e
  ```
  Python kennt außerdem eine Reihe von vordefinierten Zeichenklassen, z.B. `\d` (alle Ziffern des Dezimalsystems) und `\w` (alle alphanumerischen Zeichen inkl. Unterstrich). Hier sind die Raw-Strings nützlich, weil wir die Zeichenklassen wie angegeben (also nur mit einem einzigen Backslash) schreiben können.
- *Quantoren* nach einem Zeichen geben an, wie oft dieses Zeichen auftreten darf.
  ```python
  target = "bt bat baat baaat baaaat baaaaaaaaat"
  findall(r"ba*t", target)     # beliebig oft
  findall(r"ba+t", target)     # mindestens einmal
  findall(r"ba?t", target)     # keinmal oder einmal
  findall(r"ba{3}t", target)   # genau 3-mal
  findall(r"ba{2,}t", target)  # 2-mal oder öfter
  findall(r"ba{,3}t", target)  # höchstens 3-mal
  findall(r"ba{2,4}t", target) # 2- bis 4-mal
  ```
- *Alternativen* suchen nach zwei verschiedenen Zeichenketten.
  ```python
  findall(r"eins|zwei", "eins zwei drei")
  ```
- *Anker* legen fest, dass ein regulärer Ausdruck nur am Anfang oder Ende eines Strings gefunden werden darf:
  ```python
  findall(r"^a..", "auf dem Haus")  # Verankerung am Beginn
  findall(r"a..$", "auf dem Haus")  # Verankerung am Ende
  ```
- *Gruppen* werden durch runde Klammern um ein oder mehrere Zeichen gebildet. Gruppen bilden Einheiten, die z.B. mit einem Quantor versehen werden können. Außerdem können wir über Gruppen bestimmte Teile eines Treffers extrahieren:
  ```python
  findall(r"Tier: (\w+) (\w+)", "ein Tier: Mus musculus (Maus)")
  ```


### Pattern-Objekte

Funktionen wie `findall()` sind für einfache Anwendungsfälle nützlich. Sein ganzes Potential entfaltet das `re`-Modul allerdings, wenn wir aus einem regulären Ausdruck ein *Pattern*-Objekt erzeugen und dann die Methoden dieses Objekts verwenden.

```python
from re import compile

p = compile(r"Tier: (\w+) (\w+)")
type(p)
print(p.pattern)  # Attribut: Suchmuster
print(p.groups)   # Attribut: Anzahl der Gruppen
```

Das Pattern-Objekt besitzt u.a. die Methode `search()`, die in einem String nach Treffern sucht und bei Erfolg ein *Match-Objekt* zurückgibt.

```python
m = p.search("ein Tier: Mus musculus (Maus)")
type(m)
```

Das Match-Objekt kann nun ausgewertet werden:

```python
m.groups()  # Tupel mit den gefundenen Gruppen
m[0]        # gesamter Treffer
m[1]        # erste Gruppe
m.span()    # Start- und Endindex des gesamten Treffers
m.start(2)  # Startindex der zweiten Gruppe
m.end(1)    # Endindex der ersten Gruppe
```



## Lesen und Schreiben von Dateien

### Lesen einzelner Zeichen

Mit der interaktiven Eingabe (`input()`), dem Auslesen von Kommandozeilenargumenten (`sys.argv`) sowie der `print()`-Funktion lassen sich bereits viele Fragen der Ein- und Ausgabe lösen. Allerdings muss ein Programm häufig auf derartig große Datenmengen zugreifen, dass eine manuelle Eingabe zu umständlich ist. Daher erlaubt Python, sowohl bereits vorhandene Dateien lesend zu öffnen, als auch neue Dateien zu erzeugen und mit Inhalten zu füllen.

Wir kopieren die Datei `codon_table.csv` aus dem Home-Verzeichnis des `bioinfo1`-Benutzers in das Unterverzeichnis `python3` unseres Home-Verzeichnisses und betrachten die ersten fünf Zeilen:

```
AAA,Lys,K,Lysine
AAC,Asn,N,Asparagine
AAG,Lys,K,Lysine
AAT,Asn,N,Asparagine
ACA,Thr,T,Threonine
```

Diese Datei enthält offenbar eine Codon-Tabelle, in der jedem Basentriplet eine Aminosäure zugeordnet ist. Wir öffnen diese Datei mittels der eingebauten Funktion `open()`, welche ein Dateiobjekt zurückgibt:

```python
f = open("codon_table.csv", "r")
f
```

Das zweite Argument von `open()` legt den Zugriffsmodus auf die Datei fest – hier wollen wir lesend zugreifen. Python behandelt die geöffnete Datei als *Datenstrom*, also eine kontinuierliche (eindimensionale) Folge von Daten (hier: Zeichen), auf die im Fall einer Datei sogar an beliebiger Stelle zugegriffen werden kann.

Python kann den Inhalt der Datei grundsätzlich wie folgt verarbeiten: Unmittelbar nach dem Öffnen steht die Leseposition auf dem ersten Zeichen (Index 0), wie uns die Methode `tell()` zeigt:

```python
f.tell()
```

Wir lesen die ersten 30 Zeichen in die Variable `s` mittels der Methode `read()`:

```python
s = f.read(30)
s
```

Die ersten 30 Zeichen umfassen also die erste Zeile und einen Teil der zweiten Zeile. (Wir erkennen, dass innerhalb einer Zeile einzelne „Spalten“ durch Kommas getrennt werden. Bereits die Dateiendung hat darauf hingewiesen: csv = comma-separated values.) Nach dieser Leseoperation steht die Leseposition auf dem 31. Zeichen (Index 30):

```python
f.tell()
```

Wir können nun z.B. auch auf Index 15 zurückgehen (Methode `seek()`) und 10 Zeichen einlesen oder auf Index 100 vorspringen und zwei Zeichen einlesen:

```python
f.seek(15)
f.read(10)
f.tell()

f.seek(100)
f.read(2)
f.tell()
```


### Zeilenweises Lesen

Während der freie Zugriff auf den Datenstrom seine Vorteile hat, wird diese Möglichkeit in vielen Fällen gar nicht benötigt. Textdateien sind aus *Zeilen* aufgebaut und werden daher normalerweise auch zeilenweise eingelesen (Methode `readline()`):

```python
f.seek(0)
l1 = f.readline()
l1

l2 = f.readline()
l2
f.tell()
```

Der String `l1` beinhaltet nun die erste Zeile der Datei, und `l2` beinhaltet die zweite Zeile (jeweils einschließlich des Zeilenendes `\n`); die Leseposition steht auf dem ersten Zeichen der 3. Zeile. Ein wichtiger Hinweis: Das Zeilenende wird leider nicht einheitlich gekennzeichnet. Während Linux das Steuerzeichen `\n` (line feed) verwendet, benutzt Windows `\r\n` (carriage return – line feed). (Um die Verwirrung komplett zu machen, haben ältere Mac-Betriebssysteme sogar `\r` verwendet.) Daher kann es zu Problemen kommen, wenn eine Textdatei unter Windows erstellt und unter Linux gelesen wird.

Wir müssen uns aber nicht durch wiederholten Aufruf von `readline()` Zeile um Zeile durch die Datei arbeiten – das geht auch einfacher: Ein Dateiobjekt ist zeilenweise iterierbar und kann z.B. in einer `for`-Schleife ausgelesen werden:

```python
f.seek(0)
for line in f:
  print(line)
```

Noch einfacher geht es, indem wir mit der Methode `readlines()` alle Zeilen in einer Liste speichern:

```python
f.seek(0)
all_lines = f.readlines()
all_lines
```

Wenn wir mit dem Lesen einer Datei fertig sind, dürfen wir nicht vergessen, die Datei auch wieder mit der Methode `close()` zu schließen:

```python
f.close()
```


### Schreiben

Damit wir Daten in eine Datei schreiben können, müssen wir sie im entsprechenden Modus öffnen (d.h., wir geben als zweites Argument von `open()` den String `"w"` an):

```python
f = open("some_lines.txt", "w")
```

Nun können wir in die Datei einzelne Strings mittels `write()` schreiben:

```python
f.write("Erste Zeile\n")
f.write("Zweite Zeile\n")
```

Falls wir mehrere Strings in einer Liste schreiben wollen, können wir dies mittels der Methode `writelines()` tun. Achtung: Auch wenn der Methodenname etwas anderes vermuten lässt, schreibt Python die Listenelemente einfach nacheinander in die Datei, ohne die Zeilen durch Steuerzeichen zu trennen. Falls jeder String auf einer eigenen Zeile stehen soll, müssen wir die Steuerzeichen also explizit angeben:

```python
lines = ["drei\n", "vier\n", "fünf\n"]
f.writelines(lines)
```

Wenn wir fertig sind, müssen wir noch daran denken, die Datei zu schließen. Dies ist besonders wichtig, falls Inhalt in die Datei geschrieben haben. Es kann nämlich sein, dass Python die Schreiboperation nicht sofort nach Aufruf der `write()`- oder `writelines()`-Methode ausführt, sondern auf später verschiebt. `close()` stellt hingegen sicher, dass sämtliche noch fehlenden Daten in der Datei gespeichert werden.

```python
f.close()
```

Abschließend überzeugen wir uns auf der Kommandozeile mittels `cat`, dass wir die Daten wie gewünscht gespeichert haben.


### Kontextobjekte

Der Zugriff auf Dateien wird über die Verwendung sog. *Kontextobjekte* vereinfacht. An Stelle der Anweisungen Öffnen – Lesen/Schreiben inkl. Ausnahmebehandlung – Schließen tritt die `with`-Anweisung:

```python
with open("codon_table.csv", "r") as f:
    all_lines = f.readlines()

for line in all_lines:
    print(line)
```

Ein Kontextobjekt wird folgendermaßen erzeugt:
- Schlüsselwort `with`
- Funktion, die ein Kontextobjekt erzeugt (hier `open()`)
- Schlüsselwort `as`
- Name für das Kontextobjekt (hier : `f`)
- Doppelpunkt

Innerhalb des eingerückten Anweisungskörpers ist nun das Objekt (`f`) definiert (hier: „die Datei offen“). Wenn der Kontrollfluss den Anweisungskörper verlässt, stellt Python sicher, dass das Kontextobjekt korrekt *deinitialisiert* wird. Im Fall eines Dateiobjekts bedeutet das: Schreibe ggf. alle noch fehlenden Daten und schließe die Datei.



## Aufgaben

Speichern Sie alle Dateien, die Sie in Kurseinheit 3 erstellen müssen, im Ordner `python3` in Ihrem Home-Verzeichnis.



#### Aufgabe 3.1 (4 P)

Diese Aufgabe haben Sie erfolgreich gelöst, wenn Sie die Code-Beispiele dieser Kurseinheit durchgearbeitet haben. Achten Sie darauf, dass sich im Ordner `python3` die folgenden Dateien befinden, die Sie im Rahmen der Übungen erstellt haben:
- `codon_table.csv`
- `simple_protein.py`
- `some_lines.txt`


#### Aufgabe 3.2 (6 P)

Erstellen Sie eine Klasse `Protein` mit den folgenden Eigenschaften:
- Der Konstruktor wird mit den Argumenten `name`, `uniprot_id` und `sequence` aufgerufen, die in gleichnamigen Attributen gespeichert werden sollen.
- Die Methode `get_length` gibt die Länge des Proteins (Anzahl der Aminosäuren) zurück.
- Die Methode `contains` hat einen Parameter `peptide` und gibt `True` zurück, falls die Proteinsequenz die durch `peptide` gegebene Zeichenkette enthält.
- Die Methode `get_mw` gibt die Molekülmasse des Proteins zurück. Diese Methode hat einen optionalen Parameter `disulfides`, mit dem die Anzahl an Disulfidbrücken angegeben wird.

Die Molekülmasse eines Proteins wird mit der folgenden Formel berechnet:
```
Summe der Massen der einzelnen Reste
+ Masse eines Wassermoleküls
- 2 * Masse von Wasserstoff * Anzahl der Disulfidbrücken
```

Bitte verwenden Sie folgende Massen:
```python
AA_MASS = dict(
    G=57.05132,
    A=71.0779,
    S=87.0773,
    P=97.11518,
    V=99.13106,
    T=101.10388,
    C=103.1429,
    L=113.15764,
    I=113.15764,
    N=114.10264,
    D=115.0874,
    Q=128.12922,
    K=128.17228,
    E=129.11398,
    M=131.19606,
    H=137.13928,
    F=147.17386,
    R=156.18568,
    Y=163.17326,
    W=186.2099
)

WATER_MASS = 18.01528
H_MASS = 1.00784
```

Speichern Sie diese Klasse in der Datei `aufgabe_3_2.py`.

Sie können die korrekte Funktionalität Ihrer Klasse anhand der folgenden Beispiele testen:
```python
galanin = Protein("Galanin", "P22466", "GWTLNSAGYLLGPHAVGNHRSFSDKNGLTS")
print(type(galanin))
#> <class '__main__.Protein'>

print(galanin.get_length())
#> 30

print(galanin.contains("CGSHLV"))
#> False

print(galanin.get_mw())
#> 3157.4102999999996


insulin_B = Protein("Insulin B chain", "P01308", "FVNQHLCGSHLVEALYLVCGERGFFYTPKT")
print(insulin_B.get_length())
#> 30

print(insulin_B.contains("CGSHLV"))
#> True

print(insulin_B.get_mw(disulfides=1))
#> 3427.9056799999994
```



#### Aufgabe 3.3 (6 P)

Implementieren Sie eine Funktion `read_masses`, die eine Tabelle von Atommassen einliest.
- Die Funktion hat einen Parameter, der den Namen der Tabellendatei angibt.
- Bei der Tabelle handelt es sich um eine CSV-Datei (`average_mass.csv` im Home-Verzeichnis des `bioinfo1`-Benutzers), deren ersten fünf Zeilen folgendermaßen aussehen:
  ```
  H,1.008
  He,4.0026
  Li,6.94
  Be,9.0122
  B,10.81
  ```
  Jede Zeile umfasst also zwei durch Kommas getrennte Felder, nämlich das Elementsymbol und die durchschnittliche Atommasse. Daher können Sie jede Zeile mit einem regulären Ausdruck verarbeiten, der zwei Gruppen enthält.
- Die Funktion soll ein Dict zurückgeben; Elementsymbole und zugehörige Atommasse bilden die Schlüssel-Wert-Paare.

Speichern Sie diese Funktion in der Datei `aufgabe_3_3.py`.

Sie können die korrekte Funktionalität Ihres Programms anhand der folgenden Beispiele testen:
```python
m = read_masses("average_mass.csv")
print(m["N"])
#> 14.007

print(m["S"])
#> 32.06

print(2 * m["H"] + m["O"])
#> 18.015
```



#### Aufgabe 3.4 (5 P)

Implementieren Sie eine Funktion `calculate_mass`, die die Masse einer chemischen Formel berechnet. Die Funktion wird mit einem String aufgerufen, der die Formel enthält (z.B. `"C6 H12 O6"` oder `"C Cl4"`), und gibt die Masse der Formel als Float zurück.

Hinweise:
- Extrahieren Sie die einzelnen Elementsymbole und ihre Anzahl über einen regulären Ausdruck mit zwei Gruppen. Die erste Gruppe beinhaltet jedenfalls einen Großbuchstaben und ggf. zusätzlich einen Kleinbuchstaben. Die zweite Gruppe enthält beliebig viele Ziffern.
- Greifen Sie auf Ihre Lösung zu Aufgabe 3.3 zurück, um ein Dict mit Atommassen zu erhalten. Sie können selbst erstellte Python-Dateien genauso wie Module der Standardbibliothek via `import` einbinden (`from aufgabe_3_3 import read_masses`).

Speichern Sie diese Funktion in der Datei `aufgabe_3_4.py`.

Sie können die korrekte Funktionalität Ihres Programms anhand der folgenden Beispiele testen:
```python
print(calculate_mass("C6 H12 O6"))
#> 180.156

print(calculate_mass("H2 O"))
#> 18.015

print(calculate_mass("C34 H46 Cl N3 O10"))
#> 692.203
```
