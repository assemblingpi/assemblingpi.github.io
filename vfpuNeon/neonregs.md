# B.2 Erweiterungen der CPU-Funktionalität
## 2.3.7 VFP und NEON: Überblick über die ARMv7 NEON-Register

In ARMv7 NEON können die Register in drei verschiedenen Arten betrachtet werden:
1. **128-Bit Q-Register (Q0 - Q15)**: Es gibt 16 Q-Register, die jeweils 128 Bit breit sind.
2. **64-Bit D-Register (D0 - D31)**: Es gibt 32 D-Register, die jeweils 64 Bit breit sind. Zwei aufeinanderfolgende D-Register bilden ein Q-Register.
3. **32-Bit S-Register (S0 - S31)**: Es gibt 32 S-Register, die jeweils 32 Bit breit sind. Zwei aufeinanderfolgende S-Register bilden ein D-Register.

### Register-Mapping

Die folgende Tabelle zeigt, wie die verschiedenen Registertypen (Q, D, S) zueinander in Beziehung stehen:

| **Q-Register (128-Bit)**  |**D-Register (64-Bit)** | **S-Register (32-Bit)** | **Anmerkung**         |
|---------------------------|------------------------|-------------------------|-----------------------|
| Q0                        | D0, D1                 | S0, S1, S2, S3          | Q0 = {D0, D1}         |
| Q1                        | D2, D3                 | S4, S5, S6, S7          | Q1 = {D2, D3}         |
| Q2                        | D4, D5                 | S8, S9, S10, S11        | Q2 = {D4, D5}         |
| Q3                        | D6, D7                 | S12, S13, S14, S15      | Q3 = {D6, D7}         |
| Q4                        | D8, D9                 | S16, S17, S18, S19      | Q4 = {D8, D9}         |
| Q5                        | D10, D11               | S20, S21, S22, S23      | Q5 = {D10, D11}       |
| Q6                        | D12, D13               | S24, S25, S26, S27      | Q6 = {D12, D13}       |
| Q7                        | D14, D15               | S28, S29, S30, S31      | Q7 = {D14, D15}       |
| Q8                        | D16, D17               | -                       |                       |
| Q9                        | D18, D19               | -                       |                       |
| Q10                       | D20, D21               | -                       |                       |
| Q11                       | D22, D23               | -                       |                       |
| Q12                       | D24, D25               | -                       |                       |
| Q13                       | D26, D27               | -                       |                       |
| Q14                       | D28, D29               | -                       |                       |
| Q15                       | D30, D31               | -                       |                       |

### Erläuterungen:

- **Q-Register** (z.B. Q0) sind 128-Bit breit und bestehen aus zwei D-Registern (z.B. D0 und D1).
- **D-Register** (z.B. D0) sind 64-Bit breit und bestehen aus zwei S-Registern (z.B. S0 und S1).
- **S-Register** (z.B. S0) sind 32-Bit breit und repräsentieren die kleinste Einheit. (u.a relevant für single precision floating point arithmetik)

### Visualisierung

Ein Q-Register (z.B. Q0) kann als 128-Bit Register dargestellt werden, das aus zwei 64-Bit D-Registern (D0, D1) besteht, die wiederum aus jeweils zwei 32-Bit S-Registern bestehen (S0, S1, S2, S3).

```
Q0: [ S0 | S1 | S2 | S3 ]   ← 128-Bit
D0: [ S0 | S1 ]             ← 64-Bit
D1: [ S2 | S3 ]             ← 64-Bit
S0: [ 32-Bit ]
```

### NEON Register mit GDB inspizieren
Um NEON-Register in GDB zu inspizieren, kann man verschiedene Formate verwenden, um die Daten auf unterschiedliche Weise anzuzeigen:

1. **Byteweise Inspektion (8-Bit unsigned integer):**
   ```bash
   p /x $d0.u8
   ```
   Zeigt den Inhalt von `d0` als 8-Bit unsigned integers im hexadezimalen Format an. Jedes Byte wird separat dargestellt.

2. **Halfword-weise Inspektion (16-Bit unsigned integer):**
   ```bash
   p /x $d0.u16
   ```
   Zeigt den Inhalt von `d0` als 16-Bit unsigned integers im hexadezimalen Format an. Jedes Halbwort wird separat dargestellt.

3. **Word-weise Inspektion (32-Bit unsigned integer):**
   ```bash
   p /x $d0.u32
   ```
   Zeigt den Inhalt von `d0` als 32-Bit unsigned integers im hexadezimalen Format an.

4. **Doubleword-weise Inspektion (64-Bit unsigned integer):**
   ```bash
   p /x $d0.u64
   ```
   Zeigt den Inhalt von `d0` als 64-Bit unsigned integers im hexadezimalen Format an.

5. **Floating-Point Inspektion (32-Bit floating point):**
   ```bash
   p /f $d0.f32
   ```
   Zeigt den Inhalt von `d0` als 32-Bit Gleitkommazahlen an.

6. **Floating-Point Inspektion (64-Bit floating point):**
   ```bash
   p /f $d0.f64
   ```
   Zeigt den Inhalt von `d0` als 64-Bit Gleitkommazahlen an.

7. **Signed Byteweise Inspektion (8-Bit signed integer):**
   ```bash
   p /d $d0.s8
   ```
   Zeigt den Inhalt von `d0` als 8-Bit signed integers im Dezimalformat an.

8. **Signed Halfword-weise Inspektion (16-Bit signed integer):**
   ```bash
   p /d $d0.s16
   ```
   Zeigt den Inhalt von `d0` als 16-Bit signed integers im Dezimalformat an.

9. **Signed Word-weise Inspektion (32-Bit signed integer):**
   ```bash
   p /d $d0.s32
   ```
   Zeigt den Inhalt von `d0` als 32-Bit signed integers im Dezimalformat an.

10. **Signed Doubleword-weise Inspektion (64-Bit signed integer):**
    ```bash
    p /d $d0.s64
    ```
    Zeigt den Inhalt von `d0` als 64-Bit signed integers im Dezimalformat an.

Diese Befehle erlauben es die NEON-Register in GDB in verschiedenen Formaten zu inspizieren, abhängig davon, wie die Daten im Register interpretiert werden sollen.

|-------------------------|-------------------------------|---------------------------------|
| [zurück](neonintro.md)  | [Hauptmenü](../ueberblick.md) | [weiter](scalvekt.md)           |


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