# A.2 Basic Blocks implementieren
## 2.1.2  Datentransfer zwischen Registern und Transfer von direkten Werten
### Die MOV-Instruktion

Der `MOV`-Befehl in ARMv7 Assembler wird verwendet, um Daten von einer Quelle in ein Zielregister zu kopieren. Die Quelle kann dabei entweder ein anderes Register oder aber ein Wert sein, der direkt in den Maschinenbefehl eingebettet ist. 
Der `MOV`-Befehl beeinflusst nicht den Speicher, sondern arbeitet nur mit den Registern der CPU.

#### Syntax:

```asm
MOV <Zielregister>, <Quelle>
```

Hierbei ist `<Zielregister>` das Register, in das die Daten kopiert werden sollen, und `<Quelle>` kann ein anderes Register oder ein sofortiger (konstanter) Wert sein.

##### Laden eines unmittelbaren Wertes in ein Register

```asm
MOV Rd, #imm
```

Hierbei wird der unmittelbare Wert #imm in das Register Rd geladen. Das #-Symbol kennzeichnet, dass es sich um einen unmittelbaren (konstanten) Wert handelt. Rd steht stellvertretend für eines der General Purpose Register von R0 bis R10.

Achtung: Somit können nur 8-bit Werte in Register gespeichert werden!


##### Kopieren eines Wertes von einem Register in ein anderes Register

```asm
MOV Rd, Rn
```

Hierbei wird der Wert, der sich in `Rn` befindet, nach `Rd` kopiert. 
Sowohl Rd, als auch Rn stehen hier stellvertretend für die General Purpose Register.
Nach der Ausführung dieses Befehls enthalten sowohl `Rd` als auch `Rn` denselben Wert.

#### Anwendungsbeispiel
Betrachten wir nun ein vollständiges Beispiel, das veranschaulicht, wie der `MOV`-Befehl verwendet wird, um Werte zwischen Registern zu kopieren und unmittelbare Werte in Register zu laden:
```asm
MOV R0, #5        @ Lade den Wert 5 in Register R0 
MOV R1, R0        @ Kopiere den Wert von R0 nach R1 
MOV R2, #10       @ Lade den Wert 10 in Register R2
```

### LDR Pseudo-Instruktion
Wie zuvor erwähnt,  kann man mit der Instruktion `MOV Rd, #imm` nur 8-bit Werte in Register speichern. Um 32-Bit-Werte mittels einem Befehl in ein Register abzulegen, verwendet man die LDR-Instruktion. 

#### Syntax

```
LDR <Zielregister>, =imm
```
Hierbei ist `<Zielregister>` das Register, in das der konstante Wert `imm` kopiert werden soll. Anders als bei MOV wird hier ein `=` anstelle von `#` verwendet, um eine Konstante zu kennzeichnen.

**Achtung:** `LDR <Zielregister>, =imm` ist eine Pseudo-Instruktion, weil sie nicht direkt von der Hardware unterstützt wird. Stattdessen übersetzt der Assembler sie in eine Reihe von echten Maschinenbefehlen, um den konstanten Wert in das Register zu laden. Der echte LDR-Befehl hingegen lädt ausschließlich Daten aus bestehenden Speicheradressen in ein Register, worauf an späterer Stelle genauer eingegangen wird.

#### Beispiel
```
LDR R0, =0xaabb			@ Lade 0xaabb als 32-Bit-Wert in Register R0 (also 0x0000aabb)
LDR R1, =0xaabbccdd		@ Lade 32-Bit-Wert in Register R1
MOV R2, #0xabc			@ Wird nicht funktionieren, weil #imm-Wert zu groß
```

|---------------------|-------------------------------|-------------------------|
| [zurück](datentr.md)| [Hauptmenü](../ueberblick.md) | [weiter](Immediates.md) |


| **2.1 Datentransfer**                                                                     |
|-------------------------------------------------------------------------------------------|
| [2.1.1 Datentransfer in Assembler](datentr.md)                                            |
| [2.1.2 Datentransfer zwischen Registern und Transfer von direkten Werten](MOV.md)         |
| [2.1.3 Immediate-Werte in ARM-Assembler](Immediates.md)                                   |
| [2.1.4 Datentransfer zwischen Register und Speicher](LDR_STR.md)                          |