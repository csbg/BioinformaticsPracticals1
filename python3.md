# Kurseinheit 3

<!-- TOC -->

- [Kurseinheit 3](#kurseinheit-3)
  - [Objektorientierte Programmierung](#objektorientierte-programmierung)
    - [Attribute und der Konstruktor](#attribute-und-der-konstruktor)
    - [Methoden](#methoden)
  - [Ausnahmebehandlung](#ausnahmebehandlung)
    - [Abfangen von Ausnahmen](#abfangen-von-ausnahmen)
    - [Werfen von Ausnahmen](#werfen-von-ausnahmen)
    - [Erweiterungen von try-except](#erweiterungen-von-try-except)
  - [Lesen und Schreiben von Dateien](#lesen-und-schreiben-von-dateien)
    - [Lesen einzelner Zeichen](#lesen-einzelner-zeichen)
    - [Zeilenweises Lesen](#zeilenweises-lesen)
    - [Schreiben](#schreiben)
    - [Kontextobjekte](#kontextobjekte)
  - [Aufgaben](#aufgaben)

<!-- /TOC -->

## Reguläre Ausdrücke

Das Modul `re` (in der Standardbibliothek) implementiert Funktionen zur Verwendung regulärer Ausdrücke. Wie uns bereits aus dem Bash-Kurs bekannt ist, ist ein *regulärer Ausdruck* (regex) eine Zeichenkette, die ein Suchmuster definiert und somit Mengen anderer Zeichenketten beschreibt.

Das Modul `re` ist sehr umfangreich und bietet eine Fülle an Varianten an, wie reguläre Ausdrücke vorbereitet und verwendet werden können. Wir beschränken uns auf einige wenige Funktionen und verweisen ansonsten auf die [Dokumentation](https://docs.python.org/3/library/re.html) und den Artikel [Regular Expression HOWTO](https://docs.python.org/3/howto/regex.html). `findall()` beispielsweise sucht nach allen nicht-überlappenden Vorkommen eines regulären Ausdrucks (erstes Argument) in einem Suchstring (zweites Argument) und gibt alle Treffer als Liste zurück.

```python
from re import findall

findall(r".ython", "Python oder Jython")
```

Es empfiehlt sich, den regulären Ausdruck mittels eines *Raw-Strings* zu definieren (ein String, vor dessen erstem Anführungszeichen ein `r` steht), um den Backslash ohne Escaping verwenden zu können.

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
  Python kennt außerdem eine Reihe von vordefinierten Zeichenklassen, z.B. `\d` (alle Ziffern des Dezimalsystems) und `\w` (alle alphanumerischen Zeichen inkl. Unterstrich).
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
  findall(r"Tier: (.+) (.+)", "Tier: Mus musculus")
  ```


## Objektorientierte Programmierung

Die objektorientierte Programmierung (OOP) zählt zu den von Python unterstützten Programmierparadigmen. Wenngleich ein Kurs über OOP unzählige Stunden füllen könnte, beschränken wir uns hier auf eine pragmatische Herangehensweise und stellen uns ein Programm als eine Sammlung von *Objekten* vor, die miteinander kommunizieren. Jedes Objekt besitzt
- *Attribute*, die seinen Zustant beschreiben, und
- *Methoden*, die es erlauben, auf die Attribute zuzugreifen oder sie zu verändern.

Um ein Objekt erzeugen zu können, müssen wir zuerst eine Vorlage (*Klasse*) erstellen, die dann quasi als „Bauplan“ für einen beliebige Anzahl von Objekten verwendet werden kann. Das Erzeugen von Objekten nach der Vorlage einer Klasse heißt *Instanziierung*. Die aus einer Klasse erzeugten Objekte haben zwar die gleichen Attribute und Methoden, aber die Attribute speichern üblicherweise verschiedene Werte.

Um diese etwas abstrakten Konzepte zu veranschaulichen, werden wir eine Klasse implementieren, mit deren Hilfe wir Proteine beschreiben können. (Unsere Implementierung bleibt zunächst sehr einfach. In den Aufgaben werden Sie aber Gelegenheit haben, die Klasse um zusätzliche Funktionalitäten zu erweitern).


### Attribute und der Konstruktor

Ein Protein soll durch einen Namen und eine Aminosäuresequenz beschrieben werden. Daher benötigt eine Proteinklasse zumindest zwei Attribute:

```python
class Protein:
    def __init__(self, name, sequence):
        self.name = name
        self.sequence = sequence
```

Der obige Code definiert eine neue Klasse `Protein`, indem auf das Schlüsselwort `class` der Klassenname sowie ein Doppelpunkt folgt. Wie üblich müssen alle darauf folgenden Anweisungen, die zur Klassendefinition gehören, eingerückt sein.

Wir entwerfen auch einen *Konstruktor* für die Klasse. Dabei handelt es sich um eine spezielle Methode (Funktion), die `__init__()` heißen muss und jedesmal aufgerufen wird, wenn ein Objekt erzeugt wird. Innerhalb des Konstruktors werden die Attribute `self.name` und `self.sequence` mit den an den Konstruktor übergebenen Werten initialisiert.

Wir probieren unsere Klasse gleich aus:

```python
ins_A = Protein("insulin A chain", "GIVEQCCTSICSLYQLENYCN")
print(type(ins_A))
print(ins_A.name)
print(ins_A.sequence)
```

Hier erzeugen wir ein neues Protein-Objekt, indem wir den Klassennamen wie eine Funktion aufrufen (`Protein( ... )`) und einen Wert für die Parameter `name` (`"insulin A chain"`) und `sequence` (`"GIVEQCCTSICSLYQLENYCN"`) des Konstruktors übergeben. Im Anschluss können wir auf die Attribute des Objekts `ins_A` zugreifen (Objektname – Punkt – Attributname). Die eingebaute Funktion `type()` informiert uns darüber, von welcher Klasse `ins_A` abgeleitet wurde.

Es stellt sich aber noch eine Frage: Wir haben den Konstruktor mit drei erforderlichen Parametern definiert, aber nur mit zwei Argumenten aufgerufen, und diese wurden offensichtlich den Parametern `name` und `sequence` zugeordnet. Aber was ist mit dem Parameter `self`?

Jede Methode (also auch der Konstruktor) enthält als ersten Parameter einen Verweis auf das Objekt, von dem aus sie aufgerufen wird. Dieser Parameter wird *nur* bei der Definition der Methode angegeben (und per Konvention als `self` bezeichnet), *nicht* aber beim Aufruf der Methode (er wird von Python quasi „automatisch“ ergänzt).



### Methoden

Fügen wir zu unserer Klasse eine Methode hinzu:

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



## Ausnahmebehandlung

Selbst wenn Programmcode syntaktisch einwandfrei ist und sorgfältig geschrieben wurde, kann nicht ausgeschlossen werden, dass es während der Ausführung des Programms zu einem Fehler kommt. Um zu verhindern, dass das Programm in diesem Fall einfach abstürzt, können solche Laufzeitfehler abgefangen und behandelt werden.

Wir verdeutlichen einige Möglichkeiten der Ausnahmebehandlung in Python anhand der unten definierten Funktion `get_molmass()`, welche die molekulare Masse für eine chemische Formel berechnet. (Unsere Implementierung ist sehr einfach; in den Aufgaben werden Sie aber die Gelegenheit haben, die Funktion realistischer zu gestalten).

```python
mass_number = dict(C=12, H=1, N=14, O=16, P=31, S=32)

def get_molmass(formula):
    mass = 0
    for atom in formula:
        mass += mass_number[atom]
    return mass

print(get_molmass("HOH"))  # Wasser
print(get_molmass("HF"))   # Flusssäure
```

Der zweite Aufruf der Funktion führt zu einem `KeyError` (man sagt: Python *wirft* eine Ausnahme), da das Dictionary `mass_number` keinen Schlüssel `"F"` enthält.


### Abfangen von Ausnahmen

Für unsere Zwecke wollen wir die Funktion aber so implementieren, dass sie unbekannte Elemente einfach bei der Massenberechnung ignoriert. Zu diesem Zweck fügen wir eine *try-except-Anweisung* ein:

```python
def get_molmass(formula):
    mass = 0
    for atom in formula:
        try:
            mass += mass_number[atom]
        except KeyError as e:
            print("Unknown element: ", e.args[0])
    return mass
```

Die try-except-Anweisung besteht aus zwei Teilen:
1. Das Schlüsselwort `try` (gefolgt von einem Doppelpunkt) leitet den ersten eingerückten Anweisungskörper ein. Die Anweisungen in diesem Block werden der Reihe nach ausgeführt. Falls eine Ausnahme auftritt, wird die Ausführung dieses Blocks unterbrochen, und der Kontrollfluss springt zum except-Teil.
2. Das Schlüsselwort `except` leitet einen except-Zweig ein. Der Anweisungskopf besteht aus:
  - Schlüsselwort `except`
  - zu behandelnde Ausnahme (hier `KeyError`)
  - Schlüsselwort `as`
  - Bezeichner (hier `e`; `as` und der Bezeichner sind optional)

  Wurde der try-Block aufgrund einer Ausnahme abgebrochen, sucht Python in den folgenden except-Blöcken nach dieser Ausnahme und führt die Anweisungen im zugehörigen except-Block aus. Falls mittels des Schlüsselwortes `as` ein Bezeichner festgelegt wurde, kann im except-Block auf die gefangene Ausnahme zugegriffen werden. In unserem Beispiel nutzen wir diese Möglichkeit, um das Attribut `args` der Ausnahme auszulesen, welches den Wert des ungültigen Schlüssels enthält.

Nun testen wir die neue Funktion:

```python
get_molmass("HOH")
get_molmass("HF")
```


### Werfen von Ausnahmen

Während der erste Aufruf (wie zuvor) das richtige Ergebnis liefert, informiert uns der zweite Aufruf nun darüber, dass das Element `"F"` unbekannt ist. Im Gegensatz zu unserer ersten Implementierung tritt aber kein Fehler auf, sondern die Funktion gibt die berechnete Masse `1` zurück.

Auch wenn es auf den ersten Blick vielleicht nicht erkennbar ist: Eine geworfene Ausnahme ist ein *Objekt*, dass von einer Ausnahmen*klasse* abgeleitet wird. Daher können wir ein Ausnahmeobjekt selber auf dem üblichen Weg instanziieren:

```python
k = KeyError("a")
type(k)
```

Außerdem können wir in unseren Funktionen eigene Ausnahmen werfen:

```python
def select(a, b, which):
    if which == 1:
        return a
    elif which == 2:
        return b
    else:
        raise KeyError(which)
```

`select()` gibt abhängig vom Wert des Parameters `which` entweder das erste oder das zweite Argument zurück. Falls `which` keinen der beiden zulässigen Werte (`1` oder `2`) annimmt, wirft die Funktion mittels des Schlüsselwortes `raise` ein `KeyError`-Objekt. (Bei dessen Instanziierung wird der Wert von `which` als Argument übergeben. Falls die Ausnahme von einem anderen Teil des Programms aufgefangen wird, kann also nachvollzogen werden, warum `select()` die Ausnahme geworfen hat.)


### Erweiterungen von try-except

Die try-except-Anweisung kann um mehrere except-Blöcke erweitert werden, um verschiedene Ausnahmen abzufangen und entsprechend unterschiedliche Maßnahmen zu treffen. Außerdem kann auf den letzten except-Block u.a. ein *else-Block* folgen, dessen Anweisungskörper nur dann ausgeführt wird, falls im try-Block keine Ausnahme aufgetreten ist. Wir bauen so einen else-Block in unsere Funktion `get_molmass()` ein:

```python
def get_molmass(formula):
    mass = 0
    for atom in formula:
        try:
            mass += mass_number[atom]
        except KeyError:
            print("Unknown atom: ", atom)
        else:
            print("Known atom: ", atom)
    return mass

get_molmass("HOH")
get_molmass("HF")
```


## Lesen und Schreiben von Dateien

### Lesen einzelner Zeichen

Mit der interaktiven Eingabe (`input()`), dem Auslesen von Kommandozeilenargumenten (`sys.argv`) sowie der `print()`-Funktion lassen sich bereits viele Fragen der Ein- und Ausgabe lösen. Allerdings muss ein Programm häufig auf derartig große Datenmengen zugreifen, dass eine manuelle Eingabe zu umständlich ist. Daher erlaubt Python, sowohl bereits vorhandene Dateien lesend zu öffnen, als auch neue Dateien zu erzeugen und mit Inhalten zu füllen.

Wir kopieren die Datei `codon_table.tsv` aus dem Home-Verzeichnis des `bioinfo1`-Benutzers in unser Home-Verzeichnis und betrachten die ersten fünf Zeilen:

```
AAA	Lys	K	Lysine
AAC	Asn	N	Asparagine
AAG	Lys	K	Lysine
AAT	Asn	N	Asparagine
ACA	Thr	T	Threonine
```

Diese Datei enthält offenbar eine Codon-Tabelle, in der jedem Basentriplet eine Aminosäure zugeordnet ist. Wir öffen diese Datei mittels der eingebauten Funktion `open()`, welche ein Dateiobjekt zurückgibt:

```python
f = open("codon_table.tsv", "r")
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

Die ersten 30 Zeichen umfassen also die erste Zeile und einen Teil der zweiten Zeile. (Wir erkennen, dass innerhalb einer Zeile einzelne „Spalten“ durch Tabulatoren `"\t"` getrennt werden. Bereits die Dateiendung hat darauf hingewiesen: TSV = tab-separated values.) Nach dieser Leseoperation steht die Leseposition auf dem 31. Zeichen (Index 30):

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

Während der freie Zugriff auf den Datenstrom viele Vorteile hat, wird diese Möglichkeit in vielen Fällen gar nicht benötigt. Textdateien sind aus *Zeilen* aufgebaut und werden daher normalerweise auch zeilenweise eingelesen (Methode `readline()`):

```python
f.seek(0)
l1 = f.readline()
l1

l2 = f.readline()
l2
f.tell()
```

Der String `l1` beinhaltet nun die erste Zeile der Datei, und `l2` beinhaltet die zweite Zeile (jeweils einschließlich des Zeilenendes); die Leseposition steht auf dem ersten Zeichen der 3. Zeile. Das Zeilenende wird in dieser Datei mittels des Steuerzeichens `"\n"` (line feed) dargestellt. (Wichtiger Hinweis: Das Zeilenende wird leider nicht einheitlich gekennzeichnet. Während Linux `\n` verwendet, benutzt Windows `\r\n` (carriage return – line feed). Daher kann es zu Problemen kommen, wenn eine Textdatei unter Windows erstellt und unter Linux gelesen wird.)

Wir müssen uns aber nicht durch wiederholten Aufruf von `readline()` Zeile um Zeile durch die Datei arbeiten – das geht auch einfacher: Ein Dateiobjekt ist zeilenweise iterierbar und kann z.B. in einer `for`-Schleife ausgelesen werden:

```python
f.seek(0)
for line in f:
  print(line)
```

Wir können auch alle Zeilen in einer Liste speichern, indem wir die Methode `readlines()` aufrufen:

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

Wenn wir Daten in eine Datei schreiben wollen, müssen wir sie im entsprechenden Modus öffnen (d.h., wir geben als zweites Argument von `open()` den String `"w"` an):

```python
f = open("some_lines.txt", "w")
```

Nun können wir in die Datei einzelne Strings mittels `write()` schreiben:

```python
f.write("Erste Zeile\n")
f.write("Zweite Zeile\n")
```

Falls wir mehrere Strings in einer Liste schreiben wollen, können wir dies mittels der Methode `writelines()` tun:

```python
lines = ["drei", "vier", "fünf"]
f.writelines(lines)
```

Wenn wir fertig sind, müssen wir noch daran denken, die Datei zu schließen. (Dies ist besonders wichtig, wenn wir Inhalt in die Datei geschrieben haben: Es kann nämlich sein, dass Python die Schreiboperation nicht sofort nach Aufruf der `write()`- oder `writelines()`-Methode ausführt, sondern auf später verschiebt. `close()` stellt hingegen sicher, dass sämtliche noch fehlenden Daten in der Datei gespeichert werden.)

```python
f.close()
```


### Kontextobjekte

Der Zugriff auf Dateien wird über die Verwendung sog. *Kontextobjekte* vereinfacht. An Stelle der Anweisungen Öffnen – Lesen/Schreiben inkl. Ausnahmebehandlung – Schließen tritt die `with`-Anweisung:

```python
with open("codon_table.tsv", "r") as f:
    all_lines = f.readlines()

all_lines
```

Ein Kontextobjekt wird folgendermaßen erzeugt:
- Schlüsselwort `with`
- Funktion, die ein Kontextobjekt erzeugt (hier `open()`)
- Schlüsselwort `as`
- Name für das Kontextobjekt (hier : `f`)
- Doppelpunkt

Innerhalb des eingerückten Anweisungskörpers ist nun das Objekt (`f`) definiert. Wenn der Kontrollfluss den Anweisungskörper verlässt, stellt Python sicher, dass das Kontextobjekt korrekt *deinitialisiert* wird. Im Fall eines Dateiobjekts bedeutet das: Schreibe alle noch fehlenden Daten und schließe die Datei.



## Aufgaben

Massenzahlen aus Tabelle einlesen
finally


#### Aufgabe 2.5 (3 P)

Implementieren Sie eine Funktion `find_sites`, welche die Anzahl an N-Glykosylierungsstellen in einer Proteinsequenz zählt.
- Die Funktion wird mit einem Parameter aufgerufen, der Proteinsequenz.
- Die Funktion gibt die Anzahl der N-Glykosylierungsstellen zurück. Eine solche Stelle ist durch das folgende, vier Aminosäuren umfassende Sequenzmotif charakterisiert:
  1. Asparagin
  2. beliebige Aminosäure außer Prolin
  3. Serin oder Threonin
  4. beliebige Aminosäure außer Prolin

Speichern Sie diese Funktion in der Datei `aufgabe_2_5.py`.

Sie können die korrekte Funktionalität Ihres Programm anhand der folgenden Beispiele testen:
```python
find_sites("ALDTNYSFSSTEKNPSVRQLYIDFRKDLGWNATAEPKGYHANFCLGNATPIWSLDTQYS")  #> 2
find_sites("MVHLTPEEKSAVTALWGKVNVSEVGGEALGRLLVVYPNNSNFFESNATTS")  #> 3
```
