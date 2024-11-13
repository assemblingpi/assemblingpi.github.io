# A.4 Datentypen 
## 4.3.6 Komplexe Datentypen: Mehrdimensionale Arrays
### Übungaufgabe

Gegeben ist der folgende Codeabschnitt:
```
.data

matrix: 
    .word 0x00000000, 0x000000aa, 0xbbffaa00, 0x00aaaa00
    .word 0x0000aa0a, 0x00000000, 0xffff00ff, 0xaaaaaa00
    .word 0x0000aaaa, 0xaaaaaaaa, 0xaaaabbff, 0x00000000


.text

.global _start
_start:
    
    ldr r0, =matrix
    @ code hier
```

#### Aufgabenstellung:
Kopiere den oben stehenden Code in den CPUlator und ergänze ihn so, dass die Matrix als zweidimensionales Array verarbeitet wird. Der Array `matrix[3][16]` soll dabei nach der längsten Sequenz von `0xaa` durchsucht werden. Diese Länge in Byte soll anschließend in `r0` abgelegt werden.

|--------------------------------|------------------------------------|--------------------------------|
|   [zurück](arraysmultidim.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](arrays2dimlsg.md)   |


| **4.3 Komplexe Datentypen**                                                   |
|-------------------------------------------------------------------------------|
| [4.3.1 Intro](komplexedtypen.md)                                              |
| [4.3.2 Structs (Strukturen)](structs.md)                                      |
| [4.3.3 Arrays in Assembler](arrays.md)                                        |
| [4.3.4 Zugriffsberechnung bei einem eindimensionalen Array](array1dim.md)     |
| [4.3.5 Lookup-Tables](lookuptable.md)                           		        |
| [4.3.6 Mehrdimensionalen Arrays](arraysmultidim.md)                           |