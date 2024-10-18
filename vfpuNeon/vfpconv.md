# B.2 Erweiterungen der CPU-Funktionalität
## 2.3.5 VFP und NEON: VFP Data Conversion Befehle

Die Armv7-Architektur bietet Instruktionen, um Werte zwischen verschiedenen Floating-Point- und Integer-Formaten zu konvertieren.

### Vcvt: Vector Convert

**vcvt** ist eine Instruktion, die verwendet wird, um Werte zwischen Floating-Point- und Integer-Formaten zu konvertieren.

**Syntax:**
```asm
vcvt{r} {<cond>}.<type>.f32 Sd, Sm
vcvt {<cond>}.f32.<type> Sd, Sm
```

- **r**: Gibt an, dass eine Rundung entsprechend dem im FPSCR (Floating-Point Status and Control Register) angegebenen Rundungsmodus erfolgen soll.(optional)
- **cond**: Ermöglicht die bedingte Ausführung der Instruktion. (optional)
- **type**: Gibt den Typ des Ziel-Integer-Formats an und kann entweder `u32` (unsigned 32-bit Integer) oder `s32` (signed 32-bit Integer) sein.

#### Beispiele
**Konvertierung von Float nach Signed Integer ohne Rundung:**

```asm
vcvt.s32.f32 s1, s0
```
Hier wird der Wert im Register s0 (Floating-Point) in einen 32-bit signed Integer umgewandelt und das Ergebnis in s1 gespeichert.

**Konvertierung von Float nach Unsigned Integer mit Rundung:**

```asm
vcvt r.u32.f32 s2, s3
```
In diesem Beispiel wird der Wert im Register S3 (Floating-Point) in einen 32-bit unsigned Integer umgewandelt, unter Berücksichtigung des im FPSCR festgelegten Rundungsmodus, und das Ergebnis in s2 gespeichert.

**Bedingte Konvertierung von Signed Integer nach Float:**

```asm
vcvtle.f32.s32 s4, s5
```
Diese Instruktion wird nur ausgeführt, wenn die Bedingung "less than or equal" (le) erfüllt ist. Hier wird der Wert im Register s5 (32-bit signed Integer) in einen Floating-Point-Wert umgewandelt und das Ergebnis in s4 gespeichert.

|-------------------------|-------------------------------|----------------------------------|
| [zurück](vfp_intro.md)  | [Hauptmenü](../ueberblick.md) | [weiter](neonintro.md)           | 