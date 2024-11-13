# A.4 Datentypen 
## 4.3.2 Komplexe Datentypen: Structs (Strukturen)
Ein Struct ist ein komplexer Datentyp und gruppiert mehrere Elemente unterschiedlichen Typs unter einem gemeinsamen Namen. Im Speicher werden Structs als zusammenhängende Datenblöcke mit festen Offsets für jedes Feld abgelegt.

**Beispiel:** Eine Struct `struct_bsp` die eine Ganzzahl `i_zahl`und eine Fließkommazahl `f_wert` enthält, wird im Speicher wie folgt organisiert:

| Speicheradresse      |  Inhalt       |
|----------------------|---------------|
| `struct_bsp` + 0x00  |  i_zahl       |
| `struct_bsp` + 0x04  |  f_wert       |

Das Label `struct_bsp` repräsentiert hier die Basisadresse der Struktur im Speicher.

### C-Code Beispiel für eine solche Struct:
```c
struct struct_bsp {
    int   i_ zahl;        // 4 Bytes
    float f_wert;         // 4 Bytes
};
```

#### Deklaration einer Struct in Assembler:
In Assembler wird das Struct durch die Verwaltung von **Speicheradressen** und **Offsets** realisiert.
```asm
.data
@ offsets für den Zugriff auf die Elemente
    .equ i_zahl, 0
    .equ f_wert, 4        

struct_bsp:
    .space 8                @ Reserviere 8 Bytes für das Struct
```

#### Zugriff auf die Struct:
```
    LDR r0, =struct_bsp     @ Lade die Basisadresse des Structs in Register r0
    LDR r1, [r0, #i_Zahl]   @ Lade die Ganzzahl (Offset 0) in Register r1
    LDR r2, [r0, #f_wert]   @ Lade die Fließkommazahl (Offset 4) in Register r2
```

|--------------------------------|------------------------------------|--------------------------|
|   [zurück](komplexedtypen.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](arrays.md)    |


| **4.3 Komplexe Datentypen**                                                   |
|-------------------------------------------------------------------------------|
| [4.3.1 Intro](komplexedtypen.md)                                              |
| [4.3.2 Structs (Strukturen)](structs.md)                                      |
| [4.3.3 Arrays in Assembler](arrays.md)                                        |
| [4.3.4 Zugriffsberechnung bei einem eindimensionalen Array](array1dim.md)     |
| [4.3.5 Lookup-Tables](lookuptable.md)                           				|
| [4.3.6 Mehrdimensionalen Arrays](arraysmultidim.md)                           |