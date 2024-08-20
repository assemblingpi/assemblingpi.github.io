## Zugriffsberechnung bei einem eindimensionalen Array
Um auf ein bestimmtes Element eines Arrays zuzugreifen, berechnet man die Adresse des Elements mithilfe der Basisadresse und des Offsets. Der Offset ergibt sich aus dem Index des Elements multipliziert mit der Größe des Datentyps der Elemente. Die allgemeine Formel zur Berechnung der Adresse eines Elements lautet:

Elementadresse = Basisadresse + (Index × Elementgroesse)
Hierbei gilt: 
Basisadresse die Adresse des ersten Elements des Arrays.
Index der Index des gewünschten Elements.
Elementgröße die Elementgröße in Byte.

### Beispiel in ARM Assembler:
```asm
.data
array: .space 8, 0xaa @ array von der größe 8 Byte mit 8 x 0 initialisiert
.text

.global _start
_start:
        ldr r0, =array
        mov r1, #5 // offset == index, da elementgröße 1 Byte
        mov r2, #0
        strb r2, [r0, r1]
end:
        b end
```
[Übung zu Eindimensionalen Arrays](array1dueb.md)

[Mehrdimensionale Arrays](arraymultidim.md)