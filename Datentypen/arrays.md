# A.4 Datentypen 
## 4.3.3 Komplexe Datentypen: Arrays in Assembler

Arrays sind komplexe Datentypen, die eine Sammlung von Werten des gleichen Typs unter einem einzelnen Namen zusammenfassen. 
In einem Array werden die Elemente durch einen Index angesprochen. Der Index gibt an, welches Element innerhalb des Arrays ausgewählt wird, wobei in der Regel bei 0 begonnen wird. Das bedeutet, dass das erste Element des Arrays durch Array[0], das zweite durch Array[1] usw. ausgewählt wird. Das letzte Element eines Arrays mit n Elementen ist enstprechend Array[n-1].

## Array-Zugriff in ARM-Assembler
In ARM-Assembler arbeitet man bei der Zugriff auf Array-Elemente mit Pointern und Offsets. Der grundlegende Mechanismus zur Berechnung der Adresse eines Array-Elements basiert auf der Basisadresse des Arrays und dem Offset, der durch den Index des Elements und der Elementgröße bestimmt wird.

## Basisadresse eines Arrays
Die Basisadresse eines Arrays ist die Startadresse des Arrays im Speicher. Wenn ein Array im Speicher angelegt wird, wird die Adresse des ersten Elements als Basisadresse bezeichnet.

## Ein- und Mehrdimensionale Arrays
Ein eindimensionaler Array ist eine einfache, lineare Datenstruktur, bei der alle Elemente in einer einzigen Reihe oder Liste angeordnet sind und durch einen einzelnen Index zugänglich sind. Im Gegensatz dazu hat ein mehrdimensionaler Array mehrere Indizes, die es ermöglichen, Daten in mehreren Dimensionen (z. B. in einer Matrix mit Zeilen und Spalten) zu organisieren. Während ein eindimensionaler Array wie eine einfache Liste funktioniert, kann ein mehrdimensionaler Array als eine Tabelle, ein Gitter oder ein höherdimensionales Gitter betrachtet werden.

|-------------------------|------------------------------------|-----------------------------|
|   [zurück](structs.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](array1dim.md)    |


| **4.3 Komplexe Datentypen**                                                   |
|-------------------------------------------------------------------------------|
| [4.3.1 Intro](komplexedtypen.md)                                              |
| [4.3.2 Structs (Strukturen)](structs.md)                                      |
| [4.3.3 Arrays in Assembler](arrays.md)                                        |
| [4.3.4 Zugriffsberechnung bei einem eindimensionalen Array](array1dim.md)     |
| [4.3.5 Lookup-Tables](lookuptable.md)                           				|
| [4.3.6 Mehrdimensionalen Arrays](arraysmultidim.md)                           |