# A.5 Datentypen 
## 5.3.4 Komplexe Datentypen: Zugriffsberechnung bei einem eindimensionalen Array
Um auf ein bestimmtes Element eines Arrays zuzugreifen, berechnet man die Adresse des Elements mithilfe der Basisadresse und des Offsets. Der Offset ergibt sich aus dem Index des Elements multipliziert mit der Größe des Datentyps der Elemente. Die allgemeine Formel zur Berechnung der Adresse eines Elements lautet:
```
Elementadresse = Basisadresse + (Index × Elementgroesse)
```
**Basisadresse:** die Adresse des ersten Elements des Arrays.
**Index:** der Index des gewünschten Elements.
**Elementgröße:** die Elementgröße in Byte.

### Beispiel in ARM Assembler:
```asm
.data
array: .space 8, 0xaa   @ array von der größe 8 Byte mit 8 x 0 initialisiert
.text

.global _start
_start:
        ldr r0, =array
        mov r1, #5      @ offset == index, da elementgröße 1 Byte
        mov r2, #0
        strb r2, [r0, r1]
end:
        b end
```

|------------------------|------------------------------------|------------------------------|
|   [zurück](arrays.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](array1dueb.md)    |


| **5.3 Komplexe Datentypen**                                                   |
|-------------------------------------------------------------------------------|
| [5.3.1 Intro](komplexedtypen.md)                                              |
| [5.3.2 Structs (Strukturen)](structs.md)                                      |
| [5.3.3 Arrays in Assembler](arrays.md)                                        |
| [5.3.4 Zugriffsberechnung bei einem eindimensionalen Array](array1dim.md)     |
| [5.3.5 Mehrdimensionalen Arrays](arraysmultidim.md)                           |