Die Syntax von NEON-Instruktionen setzt sich wie folgt zusammen:

```asm
V<OpCode>.<DatenTyp> <ZielRegister>, <QuellRegister1>, <QuellRegister2>
```
Dabei gibt der OpCode die Operation an (z.B. ADD, SUB), und der Datentyp (.i8, .f32 etc.) spezifiziert die Breite der verwendeten Daten (8-bit, 32-bit, etc.). Die Register können entweder ARM-Register oder NEON-Register sein, abhängig von der jeweiligen Instruktion.

Einige NEON-Instruktionen erfordern spezifische Registertypen, während andere dem Programmierer die Wahl lassen, ob er ein Single Word-, Double Word- oder Quad Word-Register verwenden möchte. Wenn eine Instruktion etwa ein Single Precision-Register benötigt, werden die Register wie folgt bezeichnet:`Sd` für das Zielregister, `Sn` für den ersten Operand und `Sm` für den zweiten Operand. Bei Instruktionen, die nur zwei Register verwenden, wird `Sn` weggelassen. Die Bezeichnungen `Sd`, `Sn` und `Sm` stehen jeweils für eines der 31 S-Register, die von `S0` bis `S31` nummeriert sind.

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

Neben den Register- und Datentypenspezifikationen können NEON-Instruktionen auch Immediate-Werte verwenden, die in der Instruktionsnotation direkt spezifiziert werden.


