# B.2 Erweiterungen der CPU-Funktionalität
## 2.3.12 VFP und NEON: NEON Instruktionen

Die Syntax von NEON-Instruktionen setzt sich wie folgt zusammen:

```asm
V<OpCode>.<DatenTyp> <ZielRegister>, <QuellRegister1>, <QuellRegister2>
```
Dabei gibt der OpCode die Operation an (z.B. ADD, SUB), und der Datentyp (.i8, .f32 etc.) spezifiziert die Breite der verwendeten Daten (8-bit, 32-bit, etc.). Die Register können entweder ARM-Register oder NEON-Register sein, abhängig von der jeweiligen Instruktion.

Einige NEON-Instruktionen erfordern spezifische Registertypen, während andere dem Programmierer die Wahl lassen, ob er ein Single Word-, Double Word- oder Quad Word-Register verwenden möchte. Wenn eine Instruktion etwa ein Single Precision-Register benötigt, werden die Register wie folgt bezeichnet:`Sd` für das Zielregister, `Sn` für den ersten Operand und `Sm` für den zweiten Operand. Bei Instruktionen, die nur zwei Register verwenden, wird `Sn` weggelassen. Die Bezeichnungen `Sd`, `Sn` und `Sm` stehen jeweils für eines der 32 S-Register, die von `S0` bis `S31` nummeriert sind.

Die Notation der Instruktionen umfasst folgende Elemente:
- **Ry**: Ein ARM-Integer-Register, wobei „y“ eine Zahl von 0 bis 15 sein kann.
- **Ny**: Ein NEON-Register, wobei `N` für den Typ des Registers steht: `S` für 32-Bit-Register, `D` für 64-Bit-Register oder `Q` für 128-Bit-Register. `y`“ repräsentiert die entsprechende Register-Nummer.
- **Vy**: Ein NEON-Vektorregister, wobei `V` für den Typ des Vektorregisters steht: `D` für 64-Bit-Vektorregister oder `Q` für 128-Bit-Vektorregister.
- **Vy[x]**: Ein NEON-Skalar (ein Element des Vektors). Die Größe des Skalars wird durch die Instruktion bestimmt. `V` muss entweder `D` oder `Q` sein, je nach Bedarf. `x` gibt an, welches Skalar-Element des Vektors verwendet wird.

### Datentypen

Die NEON-Instruktionen arbeiten mit verschiedenen Datentypen, die in der Notation als Suffixe angehängt werden:

- **Integer-Typen**: `i8`, `i16`, `i32`, `i64`
- **Signed Integer-Typen**: `s8`, `s16`, `s32`, `s64`
- **Unsigned Integer-Typen**: `u8`, `u16`, `u32`, `u64`
- **Floating-Point-Typen**: `f16`, `f32`, `f64`

### Registerlisten

Für einige Instruktionen können Listen von bis zu vier NEON-Registern, Vektoren oder Skalaren spezifiziert werden. 

### Immediates

Neben den Register- und Datentypenspezifikationen können manche NEON-Instruktionen auch Immediate-Werte verwenden, die in der Instruktionsnotation direkt spezifiziert werden.

|------------------------|-------------------------------|-----------------------------|
| [zurück](neonctrl.md)  | [Hauptmenü](../ueberblick.md) | [weiter](vmov.md)           |


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