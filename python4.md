# Kurseinheit 4

## NumPy

Die zentrale Datenstruktur von NumPy ist der n-dimensionale *Array*. Dieser kann Vektoren (eine Dimension), Matrizen (zwei Dimensionen) und höherdimensionale Strukturen darstellen. (Wir werden uns in diesem Kurs der Einfachheit halber auf zwei Dimensionen beschränken.) Daneben stellt NumPy eine große Anzahl an Funktionen und Methoden zur Verfügung, um diese Arrays zu manipulieren und um Berechnungen mit ihnen durchzuführen. NumPy ist dabei so effizient implementiert, dass selbst riesige Arrays mit mehreren Millionen Elementen verwendet werden können.

Es ist üblich, das NumPy-Modul in einen Namensraum `np` zu importieren:

```python
import numpy as np
```


### Erzeugen eines Arrays

Es gibt mehrere Arten, um einen Array zu erzeugen, z.B. aus anderen Datenstrukturen (Liste, dicts, …), durch Funktionen (etwa `np.zeros()`), oder indem eine Datei eingelesen wird.

```python
some_numbers = [1, 4, 2, 5, 7]
a = np.array(some_numbers)  # Array aus Liste erzeugen
a

b = np.arange(24) # mit einer Funktion
b

c = np.zeros((3, 5))  
c
```

`np.arange()` funktioniert wie dir eingebaute Funktion `range()`. `np.zeros()` erzeugt einen Array mit lauter Nullen. Der Tupel `(3, 5)`, der als Argument an diese Funktion gegeben wird, legt die *Form* des Arrays fest, also die Anzahl der Elemente in den einzelnen Dimensionen (in NumPy als *Achsen* bezeichnet; hier: drei Reihen, fünf Spalten).

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
b.reshape((6, 4), order="C")  # row-major order, wie in C
b.reshape((6, 4), order="F")  # column-major oder, wie in Fortran
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

Interessant ist auch die Möglichkeit, Berechnungen mit Arrays ungleicher Formen durchzuführen. Wie soll beispielsweise der Skalar x = 10 mit dem Vektor y = (4, 1, 7) multipliziert werden? NumPy verwendet dazu eine Konvention namens *Broadcasting*, mithilfe derer kleinere Dimensionen auf die Länge der größeren gestreckt werden. Im obigen Beispiel hat die erste (und einzige) Dimension von x die Länge 1, während die erste (einzige) Dimension von y die Länge 3 hat. Daher kann die erste Dimension von x auf die Länge 3 erweitert werden, indem der Wert 10 dreimal wiederholt wird.

```pandas
x = np.array(10)
y = np.array([4, 1, 7])
x * y
```

Ähnliches passiert auch bei der Multiplikation eines Vektors mit einer Matrix:

```python
x = np.array([0, 10, 100, 1000])
y = np.array([[3, 2, 8, 9],
              [6, 5, 4, 1],
              [3, 9, 8, 1]])
x + y
```

Hier ist x ein Vektor der Länge 4 und y eine 3x4-Matrix. NumPy vergleicht die Dimensionen der Operanden von rechts nach links. (Besser erklären!)



## Pandas

### Erzeugen eines DataFrame

Pandas baut auf den Arrays von NumPy auf und implementiert Datentypen, die für umfangreiche Datenanalysen besonders gut geeignet sind: `Series` (ein eindimensionaler Array mit einem Index) und `DataFrame`. (Wir werden auf den Typ `Series` hier nicht weiter eingehen.) Jede Zeile eines `DataFrame` beschreibt eine Beobachtung, jede Spalte eine Variable (Eigenschaft) dieser Beobachtungen.

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

Besonders wichtig ist allerdings die Möglichkeit, einen DataFrame aus einer tabellarischen Datei zu erzeugen. Ein typisches Beispiel für ein entsprechendes Dateiformat ist CSV, welches eine sehr einfache rechteckige Datenstruktur mit Zeilen und Spalten darstellt. Wir kopieren die Datei `amino_acids.csv` aus dem Homeverzeichnis des `bioinfo1`-Benutzers in unser Home-Verzeichnis und betrachten ihre ersten Zeilen. Offenbar handelt es sich bei dieser Datei um eine Zusammenstellung verschiedener chemischer Eigenschaften der proteinogenen Aminosäuren. Jede Zeile beschreibt eine Aminosäure (= Beobachtung); jede Spalte enthält Daten zu einer Eigenschaft (= Variable; z.b. Abkürzung, Polarität, isoelektrischer Punkt), die für alle Aminosäuren bekannt ist.

Wir lesen den Inhalt der Datei mittels der Funktion `pd.read_csv()` in einen DataFrame:

```python
df = pd.read_csv("amino_acids.csv")
df
```

(`read_csv()` kennt über 50 verschiedene optionale Parameter und lässt sich so in jeder denkbaren Hinsicht anpassen – wie erwähnt ist das CSV-Format nicht hinreichend standardisiert, sodass CSV-Dateien in den verschiedensten Ausprägungen vorkommen.)


### Eigenschaften eines DataFrame

Wir erkunden den erstellten DataFrame:

```python
df.head()  # head und tail funktionieren
df.tail()  # wie die gleichnamigen Shell-Befehle

df.index    # (Reihen-)Index
df.columns  # Spaltenindex
df.values   # Werte der Elemente - ein NumPy-Array

df.size   # Anzahl der Elemente
df.shape  # Form
```

Außerdem berechen wir desktiptive Statistiken für die numerischen Spalten:

```python
df.median()  # Median
df.mean()    # Mittelwert
df.std()     # Standardabweichung
```


### Datenauswahl

Pandas biete mehrere Möglichkeiten, um auf Teile der Daten in einem DataFrame zuzugreifen, u.a.:
- Auswahl einzelner Spalten durch Indizierung mit Strings
  ```python
  df["code"]                 # eine Spalte über deren Namen als String
  df[["name", "abundance"]]  # mehrere Spalten über eine Liste
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
  1. Die Spalte `pi` (isoelektrischer Punkt) wird ausgewählt
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


### Datenänderungen

Im vorherigen Abschnitt haben wir gesehen, dass wir anhand des Index gezielt Daten auswählen können. Daher kann es nützlich sein, eine Spalte als *neuen Index* zu verwenden, was mittels der Methode `set_index()` möglich ist:

```python
df = df.set_index("abbreviation")
df

df.loc["Ala"]  # Zugriff auf die entsprechende Zeile über den neuen Index
```

Umgekehrt kann ein Zeilenindex in eine Spalte umgewandelt werden:

```python
df.reset_index()
```

Ein DataFrame kann entweder anhand eines Index oder anhand einer Spalte *sortiert* werden. Dabei kann üver Schlüsselwortargumente u.a. gewählt werden, welcher Index verwendet werden soll (`axis`) und ob der DataFrame auf- oder absteigend sortiert werden soll (`ascending`).

```python
df.sort_index()                        # anhand des Reihenindex
df.sort_index(axis=1)                  # anhand des Spaltenindex

df.sort_values("code")                 # anhand der Werte in Spalte "code"
df.sort_values("pi", ascending=False)  # absteigend anhand der Spalte "pi"
```

Einzelne Spalten können hinzugefügt oder entfernt werden:

```python
df["new_col"] = 3  # neue Spalte "new_col" hinzufügen
df

df = df.drop("new_col", axis=1)  # Spalte wieder entfernen; axis=1 legt fest,
df                               # dass wir eine Spalte löschen wollen
```


### Verknüpfen von Daten

Pandas bietet die Möglichkeit, mehrere DataFrames über einen sog. *Join* zu verknüpfen (der deutsceh Begriff *Verbund* ist nicht besonders geläufig). Um diese Operation zu erläutern, laden wir die Aminosäuredaten nochmals und verwenden die Spalte `code` als Index:

```python
df_left = pd.read_csv("amino_acids.csv").set_index("code")
df_left
```

Wir laden außerdem einen zweiten Datensatz (die CSV-Datei finden wir ebenso im HOME-Verzeichnis von `bioinfo1`):

```python
df_right = pd.read_csv("amino_acids_chemdata.csv").set_index("code")
df_right
```

Der zweite DataFrame enthält offenbar nicht Daten zu allen Aminosäuren (u.a. fehlt Glycin), dafür haben sich zwei andere chemische Verbindungen eingeschlichen (X und Z).

Ein Join ist eine binäre Operation und benötigt daher zwei Operanden, die meist als „linke Tabelle“ und „rechte Tabelle“ bezeichnet werden. In jeder Tabelle werden eine oder mehrere Spalten als Schlüssel festgelegt, deren Werte beim Join verglichen werden. Wir verwenden jeweils die Spalte `code` als Schlüssel und haben sie deshalb als Index verwendet (Joins in Pandas verwenden standardmäßig die Indices der beiden Tabellen als Schlüssel).

Wir stellen nun vier verschiedene Arten von Joins vor.

- Der *Left Join* verwendet alle Beobachtungen (Reihen) aus der linken Tabelle und fügt Informationen aus der rechten Tabelle hinzu, falls diese eine Beobachtung mit dem gleichen Schlüssel enthält
  ```python
  df_left.join(df_right)
  ```
  Das Ergebnis enthält 20 Zeilen (wie die ursprüngliche linke Tabelle). Die rechte Tabelle enthielt u.a. Daten zum Schlüssel `"A"`, daher konnten die entsprechenden Daten in den Spalten `flexibility` und `i_vdw` hinzugefügt werden. Die rechte Tabelle enthielt allerdings keine Daten u.a. zum Schlüssel `"F"`, daher wurden die rechten Spalten mit dem Wert `NaN` gefüllt. (NaN bedeuet „not a number“ und wird von Pandas verwendet, um einen fehlenden Wert anzuzeigen.)

- Der *Right Join* funktioniert analog zum Left Join, allerdings sind die Rollenn von linker und rechter Tabelle vertauscht.
  ```python
  df_left.join(df_right, how="right")
  ```

- Der *Inner Join* führt nur jene Beobachtungen im Ergebnis an, deren Schlüssel sowohl in der linken als auch n der rechten Tabelle vorkommen:
  ```python
  df_left.join(df_right, how="inner")
  ```
- Der *Outer Join* führt alle Beobachtungen an, die zumindest in einer der ursprünglichen Tabellen vorkommen:
  ```python
  df_left.join(df_right, how="outer")
  ```



## Aufgaben


df_aa.to_csv("data/aa_out.csv", sep=";")
