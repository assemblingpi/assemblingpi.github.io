# Structs (Strukturen)
Ein Struct ist ein komplexer Datentyp und gruppiert mehrere Elemente unterschiedlichen Typs unter einem gemeinsamen Namen. Im Speicher werden Structs als zusammenhängende Datenblöcke mit festen Offsets für jedes Feld abgelegt.

Beispiel: Ein Struct, das eine Ganzzahl und eine Fließkommazahl enthält, wird im Speicher wie folgt organisiert:

### C-Code Beispiel für eine solche Struct:
```c
struct struct_bsp {
    int zahl;        // 4 Bytes
    float wert;      // 4 Bytes
};
```
Im ARM-Assembler wird das Struct durch die Verwaltung von Speicheradressen und Offsets realisiert.
```asm
.data
@ offsets für den Zugriff auf die Elemente
   .equ Zahl, 0
   .equ Wert, 4        

struct_bsp:
    .space 8         // Reserviere 8 Bytes für das Struct

Zugriff auf die Struct:
.text
    LDR r0, =struct_bsp   @ Lade die Basisadresse des Structs in Register r0
    LDR r1, [r0, #Zahl]   @ Lade die Ganzzahl (Offset 0) in Register r1
    LDR r2, [r0, #Wert]   @ Lade die Fließkommazahl (Offset 4) in Register r2

```