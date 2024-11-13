# A.2 Basic Blocks implementieren
## 2.1.3 Datentransfer: Immediate-Werte in ARM-Assembler

In ARM Assembler kann man immediates (unmittelbare Werte) verwenden, um Werte direkt in Maschinenbefehlen einzubetten. Diese Werte werden durch das Stichwort # gefolgt von der Zahl dargestellt, z.B. #value. 

Arithmetische Befehle: Man kann solche "unmittelbaren Werte" Beispielsweise in arithmetischen Befehlen wie ADD, SUB oder MOV verwenden: 
```asm
MOV r0, #5          @ Setzt das Register r0 auf den Wert 5
ADD r1, r0, #10     @ Addiert 10 zu r0 und speichert das Ergebnis in r1
```
Vergleiche: Immediates werden auch in Vergleichsbefehlen verwendet, wie z.B. CMP:
```asm
CMP r0, #100        @ Vergleicht den Wert im Register r0 mit 100
```
Bitmasken und Schiebebefehle: Bei Befehlen wie AND, ORR oder LSL können Immediates verwendet werden, um Bitmasken zu spezifizieren oder Bits zu verschieben:
```asm
AND r2, r1, #0xFF   @ Wendet eine Bitmaske mit dem Wert 0xFF auf r1 an und speichert das Ergebnis in r2
LSL r3, r2, #2      @ Schiebt den Wert in r2 um 2 Bit nach links und speichert das Ergebnis in r3
```
**Formate von Immediate Werten**

- **Dezimal:** Der Wert wird als normale Zahl ohne Präfix angegeben, z.B. `MOV R0, #42` setzt den Wert `42` in das Register `R0`.
- **Hexadezimal:** Werte können durch das Präfix `0x` angegeben werden, z.B. `MOV R0, #0x1A` setzt den Wert `26` (hexadezimal `0x1A`) in das Register `R0`.

### Machine Code und Immediate Values

Bei der Immediate Addressierung, bei der Konstanten direkt in den Maschinen-Code eingebettet werden, ist die Größe der Konstanten, die verwendet werden kann, durch die fixe Länge der Instruktionen in RISC-CPUs wie ARM begrenzt. Diese Begrenzung ergibt sich daraus, dass nur eine bestimmte Anzahl von Bits für die Konstante zur Verfügung steht. Wer mehr darüber wissen möchte oder sich allgemein über die Details der Instruktionsformate informieren will, sollte die ARM-Referenzhandbücher ansehen und dort nach dem Format der Instruktionen suchen.

Aus dieser Beschränkung ergibt sich, dass größere Konstanten in mehreren Schritten geladen werden müssen.

#### Kombiniertes Laden von Halfword-Immmediates

MOVT (Move Top): Diese Instruktion lädt einen 16-Bit-Wert (Halfword) in die oberen 16 Bits eines Registers. Es wird üblicherweise in Kombination mit `MOVW` verwendet, um ein 32-Bit-Register vollständig zu initialisieren. Die unteren 16 Bit, bleiben unverändert.

Syntax: 
```asm
`MOVT <register>, #<imm>`
```
Beispiel: `MOVT R0, #0x1234` lädt `0x1234` in die oberen 16 Bits von `R0`, wobei die unteren 16 Bits unverändert bleiben.

MOVW (Move Wide): Diese Instruktion lädt einen 16-Bit-Wert in die unteren 16 Bits eines Registers und kann auch als Grundlage verwendet werden, um den gesamten 32-Bit-Wert zu setzen, wenn sie zusammen mit `MOVT` verwendet wird. Die oberen 16 Bits des Registers werden zu 0 gesetzt!

Syntax: 
```asm
`MOVW <register>, #<imm>`
```
Beispiel: `MOVW R0, #0x5678` lädt `0x5678` in die unteren 16 Bits von `R0`, wobei die oberen 16 Bits unverändert bleiben.

Beispiele zur Verwendung von `MOVW` und `MOVT`

Laden eines 32-Bit-Werts: Angenommen, man möchte `0x12345678` in das Register `R0` laden. Dazu musst man `MOVW` und `MOVT` kombinieren:
```asm
MOVW R0, #0x5678   @ Setzt die unteren 16 Bits auf 0x5678
MOVT R0, #0x1234   @ Setzt die oberen 16 Bits auf 0x1234
@ Nach diesen beiden Instruktionen enthält `R0` den Wert `0x12345678`.
```
Achtung: Um mit MOVW und MOVT 32-Bit Werte zu laden, muss man die Reihenfolge der Instruktionen aufgrund deren Eigenarten beachten: **Zuerst MOVT, dann MOVW!**

|-----------------|-------------------------------|----------------------|
| [zurück](MOV.md)| [Hauptmenü](../ueberblick.md) | [weiter](LDR_STR.md) |


| **2.1 Datentransfer**                                                                     |
|-------------------------------------------------------------------------------------------|
| [2.1.1 Datentransfer in Assembler](datentr.md)                                            |
| [2.1.2 Datentransfer zwischen Registern und Transfer von direkten Werten](MOV.md)         |
| [2.1.3 Immediate-Werte in ARM-Assembler](Immediates.md)                                   |
| [2.1.4 Datentransfer zwischen Register und Speicher](LDR_STR.md)                          |