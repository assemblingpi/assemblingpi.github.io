# B.2 Erweiterungen der CPU-Funktionalität
## 2.3.5 VFP und NEON: VFP Data Conversion Befehle

Die Armv7-Architektur bietet Instruktionen, um Werte zwischen verschiedenen Floating-Point- und Integer-Formaten zu konvertieren.

### Vcvt: Vector Convert

**vcvt** ist eine Instruktion, die verwendet wird, um Werte zwischen Floating-Point- und Integer-Formaten zu konvertieren.

**Syntax:**
```asm
vcvt{r} {<cond>}.<type>.f32 Sd, Sm
vcvt {<cond>}.f32.<type>    Sd, Sm
```

- **r**: Gibt an, dass eine Rundung entsprechend dem im FPSCR (Floating-Point Status and Control Register) angegebenen Rundungsmodus erfolgen soll.(optional)
- **cond**: Ermöglicht die bedingte Ausführung der Instruktion. (optional)
- **type**: Gibt den Typ des Ziel-Integer-Formats an und kann entweder `u32` (unsigned 32-bit Integer) oder `s32` (signed 32-bit Integer) sein.

#### Beispiele
**Konvertierung von Float nach Signed Integer ohne Rundung:**

```asm
vcvt.s32.f32    s1, s0
```
Hier wird der Wert im Register s0 (Floating-Point) in einen 32-bit signed Integer umgewandelt und das Ergebnis in s1 gespeichert.

**Konvertierung von Float nach Unsigned Integer mit Rundung:**

```asm
vcvt r.u32.f32  s2, s3
```
In diesem Beispiel wird der Wert im Register S3 (Floating-Point) in einen 32-bit unsigned Integer umgewandelt, unter Berücksichtigung des im FPSCR festgelegten Rundungsmodus, und das Ergebnis in s2 gespeichert.

**Bedingte Konvertierung von Signed Integer nach Float:**

```asm
vcvtle.f32.s32  s4, s5
```
Diese Instruktion wird nur ausgeführt, wenn die Bedingung "less than or equal" (le) erfüllt ist. Hier wird der Wert im Register s5 (32-bit signed Integer) in einen Floating-Point-Wert umgewandelt und das Ergebnis in s4 gespeichert.

|-------------------------|-------------------------------|----------------------------------|
| [zurück](vfp_intro.md)  | [Hauptmenü](../ueberblick.md) | [weiter](neonintro.md)           | 


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