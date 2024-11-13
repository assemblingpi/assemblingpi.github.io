# A.4 Datentypen 
## 4.3.4 Komplexe Datentypen: Zugriffsberechnung bei einem eindimensionalen Array

### Übungsaufgabe zu eindimensionalen Arrays
Gegeben ist der folgende Codeabschnitt mit einem kleinen eindimensionalen Array. 
Die Aufgabe ist nun, diesen mittels einfacher Sortieralgorithmen zu sortieren.
```asm
.data
   array: .byte 0x5, 0x12, 0x4, 0xa, 0x2, 0x1, 0x09, 0x00
   .align 4
.text
.global _start
_start:
@ code hier
```

#### Was sind Sortieralgorithmen?
Sortieralgorithmen sind Verfahren, um Daten in eine bestimmte Reihenfolge zu bringen, wie etwa Zahlen aufsteigend oder Wörter alphabetisch zu ordnen. Das Ziel ist es, die Daten so zu organisieren, dass sie einfacher zu durchsuchen oder zu bearbeiten sind.

##### Bubble Sort
Bubble Sort funktioniert, indem benachbarte Elemente in der Liste verglichen und bei Bedarf getauscht werden. Dieser Vorgang wird wiederholt, bis die Liste keine weiteren Tauschvorgänge mehr benötigt. Der Algorithmus bewegt sich durch die Liste und sorgt dafür, dass die größeren Elemente nach oben "aufsteigen", ähnlich wie Blasen in einem Glas Wasser.

##### Insertion Sort
Insertion Sort funktioniert, indem jedes Element einzeln an die richtige Stelle in einer bereits sortierten Teilliste eingefügt wird. Man beginnt mit dem ersten Element als sortiert und fügt dann nacheinander die folgenden Elemente an der passenden Stelle ein, sodass die Teilliste nach jedem Schritt sortiert bleibt. 

#### Aufgabenstellung:
Kopiere den oben abgebildeten Code in den CPUlator und ergänze ihn so, dass er
1. mittels **Bubblesort** sortiert wird
2. mittels **Insertionsort** sortiert wird

|---------------------------|------------------------------------|------------------------------|
|   [zurück](array1dim.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](array1dlsg.md)    |


| **4.3 Komplexe Datentypen**                                                   |
|-------------------------------------------------------------------------------|
| [4.3.1 Intro](komplexedtypen.md)                                              |
| [4.3.2 Structs (Strukturen)](structs.md)                                      |
| [4.3.3 Arrays in Assembler](arrays.md)                                        |
| [4.3.4 Zugriffsberechnung bei einem eindimensionalen Array](array1dim.md)     |
| [4.3.5 Lookup-Tables](lookuptable.md)                           				  |
| [4.3.6 Mehrdimensionalen Arrays](arraysmultidim.md)                           |