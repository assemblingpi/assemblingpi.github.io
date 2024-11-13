# A.4 Datentypen 
## 4.3.6 Komplexe Datentypen: Mehrdimensionale Arrays

Mehrdimensionale Arrays sind Datenstrukturen, die Daten in mehreren Dimensionen organisieren, ähnlich wie eine Tabelle oder Matrix. Während ein eindimensionales Array eine einfache Liste von Werten darstellt, organisiert ein mehrdimensionales Array diese Werte in mehreren Reihen und Spalten. Dies ist besonders nützlich in Anwendungen wie mathematischen Berechnungen oder grafischer Datenverarbeitung.

Speicherung in einem eindimensionalen Speicherbereich
Obwohl mehrdimensionale Arrays eine strukturierte, mehrdimensionale Sicht auf Daten bieten, werden sie physisch in einem eindimensionalen Speicherbereich gespeichert. Die Daten werden linear abgelegt, aber durch spezielle Zugriffsregeln als mehrdimensional interpretiert. Die Zuordnung der Array-Elemente zu Speicheradressen muss konsistent erfolgen, sodass jedes Element an der gleichen Adresse liegt, wenn es über seine Indizes angesprochen wird.

### Row-major und Column-major Ordering

Es gibt zwei Hauptmethoden zur Anordnung von mehrdimensionalen Arrays im Speicher:
Row-major Ordering: Die Elemente einer gesamten Reihe werden nacheinander gespeichert, bevor zur nächsten Reihe übergegangen wird. Dies ist die Methode, die in den meisten Programmiersprachen wie C und C++ verwendet wird.

**Column-major Ordering**: Hier werden die Elemente einer gesamten Spalte nacheinander abgelegt. Diese Methode wird häufig in Programmiersprachen wie Fortran verwendet.

### Zugriff auf zweidimensionale Arrays nach Row Major ordering
Beim Zugriff auf ein Element eines mehrdimensionalen Arrays in Hochsprachen, wie `array[i][j]`, wird intern eine Formel verwendet, um die Speicheradresse des Elements zu berechnen. Diese Formel lautet:
```
Elementadresse = Basisadresse + (Spaltenindex + (Reihengröße × Reihenindex)) × Elementgröße
```

Hierbei gilt:
- Spaltenindex: entspricht i bei array[i][j] 
- Reihenindex:  entspricht j bei array[i][j] 
- Reihengröße:  entspricht der Anzahl der Elemente pro Reihe
- Elementgröße: entspricht der Größe eines einzelnen Array-Elements in Bytes

Beispiel in ARM-Assembler:
```asm
.data
array: .space 16, 0xbb  @ 16 Elemente, die je 1 Byte groß sind und mit 0xbb
                        @ initialisiert wurden

.text
.global _start
_start:
        
        ldr r0, =array  @ basisadresse
        mov r1, #1      @ reihenindex
        mov r2, #4      @ reihengroesse
        mov r3, #0      @ spaltenindex
        mov r4, #1      @ elementgroesse in byte
        
        @ mla Rd, Rm, Rs, Rn entspricht Rd = (Rm * Rs) + Rn
        mla r1, r1, r2, r3
        mul r1, r4
        
        mov r2, #0xaa
        strb r2, [r0, r1]
        
end:
        b end
```

### Anwendungsfälle von mehrdimensionalen Arrays

Mehrdimensionale Arrays sind besonders nützlich in der low-level Grafikprogrammierung. Beim direkten Zeichnen auf den Bildschirm wird der Bildschirmspeicher als mehrdimensionales Array betrachtet, wobei jede Position im Array einem Pixel auf dem Bildschirm entspricht. Auch in wissenschaftlichen Berechnungen, wie der Arbeit mit Matrizen, und in der Bildverarbeitung sind mehrdimensionale Arrays unverzichtbar.

|----------------------------|------------------------------------|-------------------------------|
|   [zurück](lookuplsg.md)   |   [Hauptmenü](../ueberblick.md)    |   [weiter](array2dimue.md)    |


| **4.3 Komplexe Datentypen**                                                   |
|-------------------------------------------------------------------------------|
| [4.3.1 Intro](komplexedtypen.md)                                              |
| [4.3.2 Structs (Strukturen)](structs.md)                                      |
| [4.3.3 Arrays in Assembler](arrays.md)                                        |
| [4.3.4 Zugriffsberechnung bei einem eindimensionalen Array](array1dim.md)     |
| [4.3.5 Lookup-Tables](lookuptable.md)                           		|
| [4.3.6 Mehrdimensionalen Arrays](arraysmultidim.md)                           |