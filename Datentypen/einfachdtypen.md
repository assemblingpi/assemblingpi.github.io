# A.4 Datentypen 
## 4.2.1 Einfache Datentypen: Intro

Einfache Datentypen repräsentieren grundlegende Datenarten, die direkt von der Hardware unterstützt werden. Diese Datentypen unterscheiden sich in ihrer Größe und dem Wertebereich, den sie abdecken können.

### Integer (Ganzzahlen)
Ganzzahlen werden in verschiedenen Größen dargestellt, die jeweils unterschiedliche Bereiche abdecken. Es wird unterschieden zwischen signed (vorzeichenbehafteten) und unsigned (vorzeichenlosen) Ganzzahlen:
```
Byte: 8 Bit.

Halfword: 16 Bit.

Word: 32 Bit.
```
### Floating Point (Gleitkommazahlen)
Gleitkommazahlen speichern Werte mit Dezimalstellen. In Assembler werden oft folgende Formate unterstützt:
```
Single Precision: 32 Bit

Double Precision: 64 Bit
```

### Character (Zeichen)
Zeichen werden typischerweise als 8-Bit-Werte gespeichert und repräsentieren ASCII-Zeichen. (Daneben gibt es auch noch weitere Datentypen für Zeichen, aber diese werden im Rahmen dieses Tutorials nicht behandelt)
```
Beispiel: Der Buchstabe 'A' wird als Wert 65 im ASCII-Code gespeichert.
```

### Pointer (Zeiger)
Pointer sind Variablen, die die Adresse eines Wertes speichern. Sie ermöglichen dadurch den Zugriff auf Daten im Speicher und werden häufig verwendet, um auf Datenstrukturen oder Arrays zu verweisen, wodurch der Programmfluss dynamisch gesteuert werden kann.
Bei den LDR und STR Instruktionen, die bereits vorgestellt wurden, hat ein Register (gegebenenfalls mit einem Offset) - die Rolle eines solchen Zeigers übernommen. Zeiger müssen jedoch nicht zwangsläufig in Registern vorliegen, sondern können auch selbst im Speicher platziert sein.
Um einen solchen Pointer für Speicherzugriff zu **benutzen**, muss er jedoch zwangsläufig in eines der CPU-Register geladen werden.

Beispiel:
```asm
.data
ptr:  .word ptr2
ptr2: .word val
val:  .word 0xffffffff


.text
.global _start
_start:
        ldr r0, =pointer       @ r0 zeigt(!) auf pointer  
        ldr r0, [r0]           @ r0 == pointer == zeigt auf ptr
        ldr r0, [r0]           @ r0 == ptr zeigt auf val
        ldr r0, [r0]           @ r0 == val
end:
        b end
```


|----------------------------|------------------------------------|-------------------------|
|   [zurück](datentypen.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](ascii.md)    |


| **4.2 Einfache Datentypen**                                           |
|-----------------------------------------------------------------------|
| [4.2.1 Intro](einfachdtypen.md)                                       |
| [4.2.2 ASCII](ascii.md)                                               |
