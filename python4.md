# Kurseinheit 4

- [NumPy](#numpy)
  - [Erzeugen eines Arrays](#erzeugen-eines-arrays)
  - [Indizierung und Slicing](#indizierung-und-slicing)
  - [Grundlegende Operationen](#grundlegende-operationen)
  - [Laden und Speichern](#laden-und-speichern)
- [Pandas](#pandas)
  - [Erzeugen eines DataFrame](#erzeugen-eines-dataframe)
  - [Eigenschaften eines DataFrame](#eigenschaften-eines-dataframe)
  - [Datenauswahl](#datenauswahl)
  - [Datenänderungen](#datenänderungen)
  - [Verknüpfen von Daten](#verknüpfen-von-daten)
  - [Speichern](#speichern)
- [Aufgaben](#aufgaben)



## NumPy

Die zentrale Datenstruktur von NumPy ist der n-dimensionale *Array*. Dieser kann Vektoren (eine Dimension), Matrizen (zwei Dimensionen) und höherdimensionale Strukturen darstellen. (Wir werden uns in diesem Kurs der Einfachheit halber auf zwei Dimensionen beschränken.) Daneben stellt NumPy eine große Anzahl an Funktionen und Methoden zur Verfügung, um diese Arrays zu manipulieren und um Berechnungen mit ihnen durchzuführen. NumPy ist dabei so effizient implementiert, dass selbst riesige Arrays mit mehreren Millionen Elementen verwendet werden können.

Es ist üblich, das NumPy-Modul in einen Namensraum `np` zu importieren:

```python
import numpy as np
```


### Erzeugen eines Arrays

Es gibt mehrere Arten, um einen Array zu erzeugen:
- aus anderen Datenstrukturen (Liste, Dicts, …)
- durch Funktionen (wie `np.zeros()`)
- indem eine Datei eingelesen wird.

```python
some_numbers = [1, 4, 2, 5, 7]
a = np.array(some_numbers)  # Array aus Liste erzeugen
a

b = np.arange(24) # mit einer Funktion
b

c = np.zeros((3, 5))  
c
```

`np.arange()` hat die gleichen Parameter wie die eingebaute Funktion `range()`. `np.zeros()` erzeugt einen Array mit lauter Nullen. Der Tupel `(3, 5)`, der als Argument an diese Funktion gegeben wird, legt die *Form* des Arrays fest, also die Anzahl der Elemente in den einzelnen Dimensionen (in NumPy als *Achsen* bezeichnet; hier: drei Reihen, fünf Spalten).

Wir können grundlegende Eigenschaften eines Arrays mithilfe der folgenden Attribute abfragen:

```python
c.ndim   # Anzahl der Achsen (Dimensionen)
c.size   # Anzahl der Elemente
c.shape  # Form
```

Die Form eines Arrays ist keine Konstante, sondern kann geändert werden – beispielsweise um eine Matrix aus einem Vektor zu erzeugen. Die einzige Restriktion ist dabei, dass die Anzahl der Elemente gleich bleiben muss. Beispielsweise können wir den Vektor `b` (24 Elemente) in eine 6x4-Matrix oder eine 3x8-Matrix umformen, nicht jedoch in eine 5x5-Matrix:

```python
b.reshape((6, 4))   # ok
b.reshape((3, -1))  # ok; -1 -> diese Dimension wird automatisch berechnet
b.reshape((5, 5))   # Fehler
```

Wir beachten außerdem, dass beim Umformen von einer in zwei Dimensionen entweder die Reihen oder die Spalten zuerst befüllt werden können:

```python
b.reshape((6, 4), order="C")  # row-major order, wie in der Programmiersprache C
b.reshape((6, 4), order="F")  # column-major order, wie in Fortran
```

Eine wichtige Anmerkung: Wenn wir eine Methode eines Arrays aufrufen, dann gibt diese Methode einen *neuen* Array zurück – der ursprüngliche Array bleibt unverändert.

```python
b2 = b.reshape((6, 4))
b2  # neuer 6x4-Array
b   # alter 24x1-Array
```



### Indizierung und Slicing

Mithilfe von Indizierung und Slicing können wir auf einzelne Elemente des Arrays zugreifen oder Teilarrays erstellen. Achtung: Auch hier beachten wir, dass Indizes bei Null starten, und dass beim Slicing einzelne Werte weggelassen werden dürfen.

```python
d = np.arange(12).reshape(4)
d

d[0, 0]  # Element in der ersten Reihe und ersten Spalte
d[2, 1]  # Element in der dritten Reihe und zweiten Spalte

d[0]       # erste Reihe
d[-1]      # vorletzte Reihe
d[1:, :2]  # ab der 2. Reihe (inklusive), bis zur 3. Spalte (exklusive)
d[:, 0]    # alle Reihen, erste Spalte
d[::2]     # jede zweite Reihe
```



### Grundlegende Operationen

Arrays ermöglichen Vektorarithmetik, z.B. elementweise Addition und Multiplikation, oder das Skalarprodukt:

```python
x = np.array([0, 1, 2, 10, 100])
y = np.array([3, 8, 12, 5, 13])

x + y
x * y
np.dot(x, y)
```

Ein Array stellt Methoden zur Verfügung, um sein Minimum und Maximum zu bestimmen, sowie Summe, Produkt oder Mittelwert seiner Elemente zu berechnen:

```python
x.min()
x.max()
y.sum()
y.prod()
y.mean()
```

Interessant ist auch die Möglichkeit, Berechnungen mit Arrays ungleicher Formen durchzuführen. Wie soll beispielsweise der Skalar `x` = 10 mit dem Vektor `y` = (4, 1, 7) multipliziert werden? NumPy verwendet dazu eine Konvention namens *Broadcasting*, mithilfe derer kleinere Dimensionen auf die Länge der größeren gestreckt werden. Im obigen Beispiel hat die erste (und einzige) Dimension von `x` die Länge 1, während die erste (einzige) Dimension von `y` die Länge 3 hat. Daher kann die erste Dimension von `x` auf die Länge 3 erweitert werden, indem der Wert 10 dreimal wiederholt wird.

```python
x = np.array(10)
y = np.array([4, 1, 7])
x * y
```

Ähnliches passiert auch bei der Multiplikation eines Vektors mit einer Matrix und in höheren Dimensionen; wir gehen hier jedoch nicht weiter darauf ein.


### Laden und Speichern

NumPy bietet verschiedene Arten an, um einen Array zu speichern. So erzeugt `save()` beispielsweise eine Binärdatei im NPY-Format, während `savetxt()` eine lesbare Textdatei speichert.

```python
p = np.arange(10, 32, 3).reshape((4, 2))
np.save("my_array.npy", p)
np.savetxt("my_array.txt", p)
```

Für beide Dateiformate gibt es wiederum entsprechende Lesefunktionen:

```python
np.load("my_array.npy")
np.loadtxt("my_array.txt")

```




## Pandas

### Erzeugen eines DataFrame

Pandas baut auf den Arrays von NumPy auf und implementiert Datentypen, die für umfangreiche Datenanalysen besonders gut geeignet sind: `Series` (ein eindimensionaler Array mit einem Index) und `DataFrame`. Jede Zeile eines `DataFrame` beschreibt eine Beobachtung, jede Spalte eine Variable (Eigenschaft) dieser Beobachtungen.

Auch bei Pandas hat es sich eingebürgert, das Modul in einen abgekürzten Namensraum `pd` zu importieren:

```python
import pandas as pd
```

Pandas biete vielfältige Möglichkeiten, um ein `DataFrame`-Objekt zu erzeugen. Beispielsweise lassen sich verschachtelte Listen und Dicts zu einem DataFrame konvertieren:

```python
nested_list = [["a", 2, True],
               ["b", 5, True],
               ["c", 8, False]]
pd.DataFrame(nested_list)

my_dict = dict(A=["a", "b", "c"],
               B=[2, 5, 8],
               C=[True, True, False])
pd.DataFrame(my_dict)
```

Bei der letzteren Variante werden die Schlüssel des Dicts als Spaltennamen verwendet.

Besonders wichtig ist allerdings die Möglichkeit, einen DataFrame aus einer tabellarischen Datei zu erzeugen. Ein typisches Beispiel für ein entsprechendes Dateiformat ist CSV, welches eine sehr einfache rechteckige Datenstruktur mit Zeilen und Spalten darstellt. Wir kopieren die Datei `amino_acids.csv` aus dem Home-Verzeichnis des `bioinfo1`-Benutzers in das Unterverzeichnis `python4` unseres Home-Verzeichnisses und betrachten die ersten fünf Zeilen:

```
code,abbreviation,name,chem1,chem2,chem3,abundance,pi
A,Ala,alanine,nonpolar,methylene,aliphatic,8.84,6.0
C,Cys,cysteine,polar,sulfur,aliphatic,1.24,5.0
D,Asp,aspartic acid,negative,methylene,aliphatic,5.39,3.0
E,Glu,glutamic acid,negative,methylene,aliphatic,6.24,3.2
```

Offenbar handelt es sich bei dieser Datei um eine Zusammenstellung verschiedener chemischer Eigenschaften der proteinogenen Aminosäuren. Jede Zeile beschreibt eine Aminosäure (= Beobachtung); jede Spalte enthält Daten zu einer Eigenschaft (= Variable; z.B. Ein-Buchstaben-Code und isoelektrischer Punkt), die für alle Aminosäuren bekannt ist.

Wir lesen den Inhalt der Datei mittels der Funktion `pd.read_csv()` in einen DataFrame:

```python
df = pd.read_csv("amino_acids.csv")
df
```

(`read_csv()` kennt über 50 verschiedene optionale Parameter und lässt sich so in jeder denkbaren Hinsicht anpassen – wie erwähnt ist das CSV-Format nicht hinreichend standardisiert, sodass CSV-Dateien in den verschiedensten Ausprägungen vorkommen.)


### Eigenschaften eines DataFrame

Wir erkunden den erstellten DataFrame:

```python
df.head()   # head und tail funktionieren
df.tail()   # wie die gleichnamigen Shell-Befehle

df.index    # (Reihen-)Index
df.columns  # Spaltenindex (Spaltennamen)
df.values   # Werte der Elemente - ein NumPy-Array

df.size     # Anzahl der Elemente
df.shape    # Form
```

Außerdem berechnen wir deskriptive Statistiken für die numerischen Spalten:

```python
df.median()  # Median
df.mean()    # Mittelwert
df.std()     # Standardabweichung
```

Wie für den NumPy-Array gilt auch für den Pandas-DataFrame: Eine Methode gibt ein neues, verändertes Objekt zurück; der ursprüngliche DataFrame wird *nicht* verändert.


### Datenauswahl

Pandas biete mehrere Möglichkeiten, um auf Teile der Daten in einem DataFrame zuzugreifen, u.a.:
- Auswahl einzelner Spalten durch Indizierung mit Strings
  ```python
  df["code"]                 # eine Spalte über ihren Namen als String wählen
  df[["name", "abundance"]]  # mehrere Spalten über eine Liste wählen
  ```

- Auswahl einzelner Zeilen durch Slicing mit Integers
  ```python
  df[:4]   # Zeilen mit Index 0 bis 3
  df[2:6]  # Zeilen mit Index 2 bis 5
  ```

- Auswahl einzelner Zeilen durch Boolesche Datentypen
  ```python
  df[df["pi"] > 7]  # wählt alle basischen Aminosäuren
  ```
  Diese Anweisung besteht quasi aus drei Teilen:
  1. Die Spalte `pi` (isoelektrischer Punkt) wird ausgewählt.
     ```python
     df["pi"]
     ```
  2. Überprüfen, ob die Elemente dieser Spalte größer als 7 sind (ein Beispiel für Broadcasting: Der Skalar 7 wird auf die Länge der Spalte gestreckt).
    ```python
    df["pi"] > 7
    ```
  3. Auswahl jener Zeilen, bei denen der Vergleich (2) wahr ist.

- Labelbasierende Indizierung mittels des Attributs `.loc`
  ```python
  df.loc[2, "code"]            # Element in der 3. Zeile, Spalte "code"
  df.loc[3:6, "code":"chem1"]  # Elemente in der 4. bis 7. Zeile,
                               # Spalten "code" bis "chem1"
  ```
  (Achtung: Beim Slicing wird hier ausnahmsweise das Endelement mitgenommen!).


- Positionsbasierende Indizierung mittels des Attributs `.iloc`
  ```python
  df.iloc[4]          # Zeile mit Index 4
  df.iloc[:, 4]       # Spalte mit Index 4
  df.iloc[10:13, :4]  # Zeilen mit Indices 10 bis 12, Spalten mit Indices 0 bis 3
  ```

Wenn wir in einem DataFrame eine einzelne Spalte wählen, gibt Python ein Objekt der Klasse `Series` zurück. Auch diese Klasse implementiert eine Reihe von Methoden, u.a.:

```python
df["abundance"].sum()  # Summe der Werte in Spalte "abundance"
df["pi"].mean()        # durchschnittlicher isoelektrischer Punkt
```



### Datenänderungen

Im vorherigen Abschnitt haben wir gesehen, dass wir anhand des Index gezielt Daten auswählen können. Daher kann es nützlich sein, eine Spalte als *neuen Index* zu verwenden, was mittels der Methode `set_index()` möglich ist:

```python
df = df.set_index("abbreviation")
df

df.loc["Ala"]  # Zugriff auf die entsprechende Zeile über den neuen Index
```

Umgekehrt kann ein Zeilenindex in eine Spalte umgewandelt werden:

```python
df = df.reset_index()
df
```

Ein DataFrame kann entweder anhand eines Index oder anhand einer Spalte *sortiert* werden. Dabei kann über Schlüsselwortargumente u.a. gewählt werden, ob die Zeilen oder Spalten sortiert werden sollen (`axis`) und ob die Sortierung auf- oder absteigend sein soll (`ascending`).

```python
df = df.set_index("abbreviation")
df.sort_index()                        # anhand des Reihenindex
df.sort_index(axis=1)                  # anhand des Spaltenindex

df.sort_values("abundance")            # anhand der Werte in Spalte "abundance"
df.sort_values("pi", ascending=False)  # absteigend anhand der Spalte "pi"
```

Einzelne Spalten können hinzugefügt oder entfernt werden:

```python
df["new_col"] = 3                # neue Spalte "new_col" hinzufügen
df

df = df.drop("new_col", axis=1)  # Spalte wieder entfernen; axis=1 legt fest,
df                               # dass wir eine Spalte löschen wollen
```


### Verknüpfen von Daten

Pandas bietet die Möglichkeit, mehrere DataFrames über einen sog. *Join* zu verknüpfen (zu Deutsch *Verbund*; dieser Begriff ist aber nicht besonders geläufig). Um diese Operation zu erläutern, laden wir erneut die Aminosäuredaten:

```python
df_left = pd.read_csv("amino_acids.csv")
df_left
```

Wir laden außerdem einen zweiten Datensatz (die CSV-Datei finden wir ebenso im Home-Verzeichnis von `bioinfo1`):

```python
df_right = pd.read_csv("amino_acids_chemdata.csv")
df_right
```

Der zweite DataFrame enthält offenbar nicht Daten zu allen Aminosäuren (u.a. fehlt Glycin), dafür haben sich zwei andere chemische Verbindungen eingeschlichen (X und Z).

Ein Join ist eine binäre Operation und benötigt daher zwei Operanden, die meist als „linke Tabelle“ und „rechte Tabelle“ bezeichnet werden. In jeder Tabelle werden eine oder mehrere Spalten als Schlüssel festgelegt, deren Werte beim Join verglichen werden. Wir verwenden jeweils die Spalte `code` als Schlüssel.

Wir stellen nun vier verschiedene Arten von Joins vor, die jeweils über die Methode `merge()` durchgeführt werden können:

- Der *Left Join* verwendet alle Beobachtungen (Reihen) aus der linken Tabelle und fügt Informationen aus der rechten Tabelle hinzu, falls diese eine Beobachtung mit dem gleichen Schlüssel enthält.
  ```python
  df_left.merge(df_right, how="left", on="code")
  ```
  Das Ergebnis enthält 20 Zeilen (wie die ursprüngliche linke Tabelle). Die rechte Tabelle enthielt u.a. Daten zum Schlüssel `"A"`, daher konnten die entsprechenden Daten in den Spalten `flexibility` und `i_vdw` hinzugefügt werden. Die rechte Tabelle enthielt allerdings keine Daten u.a. zum Schlüssel `"F"`, daher wurden die rechten Spalten mit dem Wert `NaN` gefüllt. (NaN bedeutet „not a number“ und wird von Pandas verwendet, um einen fehlenden Wert anzuzeigen.)

- Der *Right Join* funktioniert analog zum Left Join, allerdings sind die Rollen von linker und rechter Tabelle vertauscht:
  ```python
  df_left.merge(df_right, how="right", on="code")
  ```

- Der *Inner Join* übernimmt nur jene Beobachtungen in das Ergebnis, deren Schlüssel sowohl in der linken als auch in der rechten Tabelle vorkommen:
  ```python
  df_left.merge(df_right, how="inner", on="code")
  ```
- Der *Outer Join* übernimmt alle Beobachtungen, die zumindest in einer der ursprünglichen Tabellen vorkommen:
  ```python
  df_outer = df_left.merge(df_right, how="outer", on="code")
  df_outer
  ```


### Speichern

Die Methode `to_csv()` speichert den Inhalt eines DataFrames in einer Textdatei im CSV-Format:

```python
df_outer.to_csv("joined.csv")
```

Wie die entsprechende Funktion zum Lesen einer CSV-Datei kann auch `to_csv()` mittels einer Vielzahl an optionalen Parametern konfiguriert werden.



## Aufgaben

Speichern Sie alle Dateien, die Sie in Kurseinheit 3 erstellen müssen, im Ordner `python4` in Ihrem Home-Verzeichnis.



#### Aufgabe 4.1 (3 P)

Diese Aufgabe haben Sie erfolgreich gelöst, wenn Sie die Code-Beispiele dieser Kurseinheit durchgearbeitet haben. Achten Sie darauf, dass sich im Ordner `python4` die folgenden Dateien befinden, die Sie im Rahmen der Übungen erstellt haben:
- `my_array.npz`
- `my_array.txt`
- `amino_acids.csv`
- `amino_acids_chemdata.csv`
- `joined.csv`



#### Aufgabe 4.2 (3 P)

Implementieren Sie eine Funktion `is_normal`, die überprüft, ob zwei Spalten einer Matrix normal zueinander sind.
- Die Funktion hat drei Parameter:
  1. Name einer Datei, in der die Matrix gespeichert ist. Die Datei hat ein Format, das mit `np.loadtxt()` gelesen werden kann.
  2. Index der ersten Spalte.
  3. Index der zweiten Spalte.
- Die Funktion gibt einen Booleschen Wert zurück.
- Zwei Vektoren sind normal, wenn ihr Skalarprodukt verschwindet.
- Seien Sie vorsichtig, wenn Sie überprüfen, ob das Skalarprodukt gleich null ist. Eine Gleitkommazahl sollte niemals mit dem `==`-Operator verglichen werden. Verwenden Sie besser die Funktion `isclose()` aus dem Modul `math`.

Speichern Sie diese Funktion in der Datei `aufgabe_4_2.py`.

Sie können die korrekte Funktionalität Ihres Programms anhand der folgenden Beispiele testen (die Datei `vectors.txt` befindet sich im Home-Verzeichnis von `bioinfo1`):
```python
# vergleicht die Vektoren [1 0 0 0 0] und [0 0 1 0 0]
print(is_normal("vectors.txt", 0, 2))  
#> True

# vergleicht die Vektoren [3 -2 7 0 1] und [0 0 1 0 0]
print(is_normal("vectors.txt", 1, 2))
#> False

# vergleicht die Vektoren [5 0 -2 8 -1] und [3 -2 7 0 1]
print(is_normal("vectors.txt", 4, 1))
#> True
```


#### Aufgabe 4.3 (4 P)

Implementieren Sie eine Funktion `translate()`, die eine DNA-Sequenz in eine Proteinsequenz übersetzt und deren mittleren isoelektrischen Punkt berechnet.

Es gibt viele verschiedene Möglichkeiten, so eine Funktion zu entwerfen; bitte verwenden Sie folgende Variante:
- Die Funktion hat einen einzigen Parameter: ein String mit einer DNA-Sequenz. Sie können davon ausgehen, dass die Länge dieser Sequenz durch drei teilbar ist.
- Erzeugen Sie in dieser Funktion zunächst einen DataFrame mit einer Spalte aus Codons, indem Sie die DNA-Sequenz in Basentriplets „zerlegen“. Dies ist auf mehrere Arten möglich, z.B. über eine `for`-Schleife oder mit der Funktion `wrap()` aus dem Modul [`textwrap`](https://docs.python.org/3/library/textwrap.html).
- Führen Sie dann einen Join zu den beiden DataFrames aus, die Sie durch Laden der Dateien `amino_acids.csv` (bereits im Ordner) und `codon_table_small.csv` (im Home-Verzeichnis von `bioinfo1`) erhalten. Überlegen Sie, welche Art von Joins Sie benötigen, in welcher Reihenfolge Sie die Joins durchführen müssen, und welche Spalten jeweils als Schlüssel dienen sollen.
- Die Funktion soll zwei Werte zurückgeben:
  1. die Proteinsequenz als String (dazu die String-Methode `join()` auf eine der Spalten des Join-Ergebnisses anwenden)
  2. den mittleren isoelektrischen Punkt als Float (diesen haben Sie in dieser Übung schon einmal berechnet)

Speichern Sie diese Funktion in der Datei `aufgabe_4_3.py`.

Sie können die korrekte Funktionalität Ihres Programms anhand der folgenden Beispiele testen:
```python
print(translate("AGCCCTCCAGGACAGGCTGCATCAGAAGAGGCCATCAAGCAGATCACTGTCCTTCTGCCA"))
#> ('SPPGQAASEEAIKQITVLLP', 5.869999999999999)

print(translate("TGCGCCTCCTGCCCCTGCTGGCGCTGCTGGCCCTCTGGGGACCTGACC"))
#> ('CASCPCWRCWPSGDLT', 5.824999999999999)
```


#### Aufgabe 4.4 (3 P)

Sie haben in einer Datenbank einige Eigenschaften von Aminosäuren abgefragt und die Ergebnisse in einer Textdatei heruntergeladen. Obwohl die Datenbanksoftware nach eigenen Angaben eine CSV-Datei zum Download zur Verfügung stellt, sind Sie einigermaßen überrascht, als Sie die folgende Datei vorfinden (`data_ugly.csv` im Home-Verzeichnis von `bioinfo1`; die ersten zehn Zeilen sind gezeigt):

```
# Spalte 1: Ein-Buchstaben-Code
# Spalte 2: Bezeichnung
# Spalte 3: Ist die Aminosäure geladen?
# Spalte 4: isoelektrischer Punkt
# Spalte 5: für Debugging-Zwecke
A;alanine;nope;6_0;-1
C;cysteine;nope;5_0;-1
D;aspartic acid;sure;3_0;-1
E;glutamic acid;sure;3_2;-1
F;phenylalanine;nope;5_5;-1
```

Sie erkennen gleich ein paar Probleme in dieser Datei:
- Die Spalten sind durch Strichpunkte getrennt.
- Die erste Zeile beinhaltet keine Spaltennamen; dafür stehen am Beginn der Datei offenbar fünf Kommentarzeilen, die mit einer Raute beginnen und den INhalt der einzelnen Spalten erklären.
- In der dritten Spalte steht, ob die jeweilige Aminosäure geladen ist oder nicht. Obwohl Sie hier Boolesche Werte erwartet haben, hat das Datenbankprogramm geladene Aminosäuren mit „sure“ und ungeladene mit „nope“ gekennzeichnet.
- In der vierten Spalte steht der isoelektrische Punkt. Aus unerfindlichen Gründen wurde aber ein Unterstrich als Dezimaltrennzeichen verwendet!
- Die fünfte Spalte enthält völlig nutzlose Informationen und sollte daher weggelassen werden.

Glücklicherweise wissen Sie, dass die `read_csv()`-Funktion von Pandas durch optionale Parameter derart angepasst werden kann, dass sie auch mit dieser Datei zurecht kommen sollte. Schlagen Sie in der [Dokumentation](https://pandas.pydata.org/docs/) nach, welche Parameter dies sind, sodass Sie den folgenden DataFrame laden können (die ersten fünf Zeilen sind gezeigt):

```
>>> df
   code           name  charged    pi
0     A        alanine    False   6.0
1     C       cysteine    False   5.0
2     D  aspartic acid     True   3.0
3     E  glutamic acid     True   3.2
4     F  phenylalanine    False   5.5
```

Kopieren Sie die Datei `data_ugly.csv` in Ihren Unterordner `python4`. Speichern Sie Ihren Programmcode in der Datei `aufgabe_4_4.py` und schreiben Sie den richtig formatierten DataFrame in die Datei `data_tidy.csv`.
