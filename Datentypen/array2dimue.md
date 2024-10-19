# A.5 Datentypen 
## 5.3.5 Komplexe Datentypen: Mehrdimensionalen Arrays
### Übungaufgabe

Gegeben ist der folgende Codeabschnitt:
```asm
.data

matrix: .word 0x0, 0xaa, 0xbbff0000, 0x0
        .word 0xaaaa, 0x0, 0xffff00ff, 0xaaaa
        .word 0x0, 0xaaaaaaaa, 0xaaaabbff, 0x0


.text

.global _start
_start:
        
        ldr r0, =matrix
        @ code hier
```

#### Aufgabenstellung:
Kopiere den oben abgebildeten Code in den CPUlator und ergänze ihn so, dass er die Daten in Matrix als einen zweidimensionalen Array verarbeitet.
Der array matrix[3][16] soll in jeder Zeile nach der längsten Reihe von 0xaa durchsucht werden und diese soll dann jeweils durch eine ebensolange Kette von 0xcc ersetzt werden.

|--------------------------------|------------------------------------|------------------------------------|
|   [zurück](arraysmultidim.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](../Proz/prozprog.md)    |


| **5.3 Komplexe Datentypen**                                                   |
|-------------------------------------------------------------------------------|
| [5.3.1 Intro](komplexedtypen.md)                                              |
| [5.3.2 Structs (Strukturen)](structs.md)                                      |
| [5.3.3 Arrays in Assembler](arrays.md)                                        |
| [5.3.4 Zugriffsberechnung bei einem eindimensionalen Array](array1dim.md)     |
| [5.3.5 Mehrdimensionalen Arrays](arraysmultidim.md)                           |