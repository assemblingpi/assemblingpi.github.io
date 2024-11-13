# A.4 Datentypen 
## 4.3.6 Komplexe Datentypen: Mehrdimensionale Arrays
### Lösung: Mehrdimensionale Arrays

```
    mov r1, #0          @ reihenindex
    mov r2, #16         @ reihengroesse
    mov r3, #0          @ spaltenindex
    mov r4, #1          @ offset
    mov r5, #0          @ Zähler
    mov r6, #0          @ Maximaler Zählwert
```

Mehrere Register werden initialisiert, um die Schleife zu steuern und Indizes sowie Zähler zu verwalten. 

```
matrix_inc:
    @ R1 = (R1 * R2) + R3
    mla r4, r1, r2, r3
    ldr r0, =matrix     @ Basisadresse der Matrix
```

Im Abschnitt `matrix_inc` wird der Offset berechnet. Anschließend wird die Matrixbasisadresse in `r0` geladen.


```
    cmp r3, r2
    bhs row_end
    ldrb r7, [r0, r4]
    cmp r7, #0xaa
    bne not_aa
    add r5, r5, #1
```

Nun wird geprüft, ob der Spaltenindex die maximale Spaltenanzahl einer Zeile erreicht hat. Wenn ja, erfolgt ein Sprung zu `row_end`. Andernfalls wird das Byte an der Adresse (`matrix + r4`) in `r7` geladen und mit `0xaa` verglichen. Bei Ungleichheit springt das Programm zu `not_aa`, andernfalls wird der Zähler um eins erhöht.



```
col_end:
    add r3, r3, #1
    b matrix_inc
```
Nach der Überprüfung des Bytes wird der Spaltenindex (`r3`) inkrementiert, und die Schleife beginnt erneut mit `matrix_inc`, um das nächste Byte in der aktuellen Reihe zu verarbeiten.

```
row_end:
    mov r3, #0
    add r1, r1, #1
    cmp r5, r6
    movlo r5, #0
    movhi r6, r5
    cmp r1, #3
    blo matrix_inc
```

Wenn das Ende einer Reihe erreicht ist, wird der Spaltenindex auf 0 gesetzt und der Reihenindex um 1 erhöht. Danach wird `r5` (aktueller Zähler) mit `r6` (maximaler Zähler) verglichen: Ist `r5` kleiner, wird `r5` zurückgesetzt, sonst wird `r6` aktualisiert. Abschließend wird geprüft, ob alle drei Reihen verarbeitet wurden; wenn nicht, springt die Schleife zu `matrix_inc`.

```
done:
    mov r0, r6
    b .                   
```

Der Abschnitt `done` übernimmt die geforderte Speicherung der maximal gefundenen Sequenzlänge (`r6`) in das Register `r0` und führt anschließend eine Endlosschleife aus, um das Programm zu beenden.

```
not_aa:
    cmp r5, r6
    movlo r5, #0
    blo col_end
    mov r6, r5
    mov r5, #0
    b col_end
```

Im Abschnitt `not_aa` wird der aktuelle Zähler `r5` mit dem maximalen Zähler `r6` verglichen. Ist `r5` kleiner, wird er auf 0 zurückgesetzt und zu `col_end` gesprungen. Andernfalls wird `r6` auf den Wert von `r5` gesetzt, bevor auch `r5` zurückgesetzt und die Verarbeitung der nächsten Spalte fortgesetzt wird.

|------------------------------|------------------------------------|------------------------------------|
|   [zurück](array2dimue.md)   |   [Hauptmenü](../ueberblick.md)    |   [weiter](../Proz/prozprog.md)    |


| **4.3 Komplexe Datentypen**                                                   |
|-------------------------------------------------------------------------------|
| [4.3.1 Intro](komplexedtypen.md)                                              |
| [4.3.2 Structs (Strukturen)](structs.md)                                      |
| [4.3.3 Arrays in Assembler](arrays.md)                                        |
| [4.3.4 Zugriffsberechnung bei einem eindimensionalen Array](array1dim.md)     |
| [4.3.5 Lookup-Tables](lookuptable.md)                           		        |
| [4.3.6 Mehrdimensionalen Arrays](arraysmultidim.md)                           |