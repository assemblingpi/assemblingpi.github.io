# B.2 Erweiterungen der CPU-Funktionalität
## 2.3.11 VFP und NEON: Steuerung und Statusübertragung zwischen ARM- und NEON/VFP-Statusregistern (VMSR und VMRS)

**VMSR (Vector Move to Special Register)**
```asm
VMSR FPSCR, Rx
```
Der Befehl VMSR überträgt den Inhalt eines ARM-Registers in das FPSCR (Floating-Point Status and Control Register).
Dieser Befehl ist vonnöten um den Zustand von NEON- und Gleitkommaoperationen durch direkte Manipulation des FPSCR zu steuern. Da NEON- und Gleitkommaoperationen spezifische Anforderungen haben, die vom Statusregister gesteuert werden (z.B. Rundungsmodi, Ausnahmebedingungen), ermöglicht VMSR den Entwicklern, diese Modi direkt zu setzen.


**VMRS (Vector Move from Special Register)**
```asm
VMRS Rd, FPSCR
```
Der Befehl VMRS überträgt den aktuellen Status aus einem NEON- oder VFP-Statusregister (z.B. FPSCR) in ein ARM-Register.
Dies ist wichtig, um den aktuellen Status und die Ergebnisse von NEON- oder Gleitkommaoperationen zu lesen. Da das FPSCR Zustände wie Überlauf, Unterlauf, ungültige Operationen, usw. speichert, ermöglicht VMRS es, diesen Status auszulesen und entsprechende Entscheidungen im Programm zu treffen.

### Achtung: Bedingte Ausführung von NEON-Code hängt vom CPSR ab!
Wichtig zu beachten ist, dass die Bedingungsflags im FPSCR (wie N, Z, C, V) zuerst in das CPSR übertragen werden müssen, um sie für bedingte Ausführungen von NEON-Code verwenden zu können! NEON-Instruktionen selbst unterstützen bis auf einige Ausnahmen keine bedingte Ausführung, demnach muss die bedingte Ausführung mit Sprüngen implementiert werden.

**1. Übertragen der Flags vom FPSCR in ein ARM-Register**
Zuerst müssen die Flags aus dem FPSCR in ein ARM-Register übertragen werden. Dies geschieht mithilfe des Befehls VMRS:

```asm
VMRS r0, fpscr  @ Übertrage die FPSCR-Flags in das ARM-Register r0
```
**2. Übertragen der Flags vom ARM-Register in das CPSR**
Nachdem die Flags im ARM-Register gespeichert sind, können sie in das CPSR übertragen werden. Dies erfolgt mit dem Befehl MSR:

```asm
MSR cpsr_f, r0  @ Übertrage die Flags vom ARM-Register r0 in das CPSR
```

**Alternativ kann man auch nur die NZCV Flags in das CPSR übertragen**
Das ist mit einem einzelnen Befehl möglich:

```asm
 VMRS APSR_nzcv, FPSCR
```
Dieser Befehl kopiert die Statusbits **N, Z, C, V** des Floating-Point Status and Control Register (FPSCR) in das Application Program Status Register (APSR).
Das APSR (Application Program Status Register) speichert die Statusflags (z.B. N, Z, C, V) und ist immer aktiv, unabhängig vom aktuellen Modus des Prozessors. Es spiegelt nicht den Modus wider, in dem sich der Prozessor befindet, sondern nur die aktuellen Zustand-Flags, die nach jeder Anweisung aktualisiert werden. DAS APSR ist somit eine Teilemnge des CPSR.
Dies ermöglicht es, die Ergebnisse von Gleitkommaberechnungen mit den Statusbits zu überprüfen.

|-----------------------|-------------------------------|----------------------------------|
| [zurück](neonstat.md) | [Hauptmenü](../ueberblick.md) | [weiter](neoninstr.md)           |


|**2.3 VFP und NEON**                                                                                               |
|-------------------------------------------------------------------------------------------------------------------|
| [2.3.1 Intro](floatingintro.md)                                                                                   |
| [2.3.2 Gleitkommazahlen](bingleit.md)                                                                             |
| [2.3.3 Floating Point Format nach IEEE 754](floatingnums.md)                                                      |
| [2.3.4 VFP (Vector Floating Point) in der ARM-Architektur](vfp_intro.md)                                          |
| [2.3.5 VFP Data Conversion Befehle](vfpconv.md)                                                                   |
| [2.3.6 Was ist NEON?](neonintro.md)                                                                               |
| [2.3.7 Überblick über die ARMv7 NEON-Register](neonregs.md)                                                       |
| [2.3.8 Vektoren und Skalare](scalvekt.md)                                                                         |
| [2.3.9 Registeradressierung in NEON](neonadr.md)                                                                  |
| [2.3.10 Das NEON und Floatingpoint Status Register](neonstat.md)                                                  |
| [2.3.11 Steuerung und Statusübertragung zwischen ARM- und NEON/VFP-Statusregistern (VMSR und VMRS)](neonctrl.md)  |
| [2.3.12 NEON Instruktionen](neoninstr.md)                                                                         |
| [2.3.13 Datentransfer](vmov.md)                                                                                   |
| [2.3.14 NEON Load/Store Instruktionen](neonldstr.md)                                                              |
| [2.3.15 Arithmetische und logische NEON-Operationen](varithlog.md)                                                |
| [2.3.16 VTRN (Vector Transpose) Instruktionen](vtrn.md)                                                           |
| [2.3.17 Implementierung von Trigonometrischen Funktionen](trigon_ue.md)                                           |
| [2.3.18 Implementierung einer 4x4-Matrixmultiplikationsfunktion mit NEON](matrix_ue.md)                           |