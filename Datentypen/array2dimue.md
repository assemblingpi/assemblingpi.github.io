# A.5 Datentypen 
## 5.3.5 Komplexe Datentypen: Mehrdimensionale Arrays
### Übungaufgabe

Gegeben ist der folgende Codeabschnitt:
```
.data

matrix: 
    .word 0x00000000, 0x000000aa, 0xbbff0000, 0x00000000
    .word 0x0000aaaa, 0x00000000, 0xffff00ff, 0x0000aa0a
    .word 0x00000000, 0xaaaaaaaa, 0xaaaabbff, 0x00000000


.text

.global _start
_start:
        
        ldr r0, =matrix
        @ code hier
```

#### Aufgabenstellung:
Kopiere den oben stehenden Code in den CPUlator und ergänze ihn so, dass die Matrix als zweidimensionales Array verarbeitet wird. Der Array `matrix[3][16]` soll dabei nach der längsten Sequenz von `0xaa` in jeder Zeile durchsucht werden. Die Länge dieser Sequenzen in Byte soll anschließend in `r0`.

|--------------------------------|------------------------------------|------------------------------------|
|   [zurück](arraysmultidim.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](../Proz/prozprog.md)    |


| **5.3 Komplexe Datentypen**                                                   |
|-------------------------------------------------------------------------------|
| [5.3.1 Intro](komplexedtypen.md)                                              |
| [5.3.2 Structs (Strukturen)](structs.md)                                      |
| [5.3.3 Arrays in Assembler](arrays.md)                                        |
| [5.3.4 Zugriffsberechnung bei einem eindimensionalen Array](array1dim.md)     |
| [5.3.5 Mehrdimensionalen Arrays](arraysmultidim.md)                           |