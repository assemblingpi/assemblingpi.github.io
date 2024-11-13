# B.2 Erweiterungen der CPU-Funktionalität
## 2.3.9 VFP und NEON: Registeradressierung in NEON

In ARMv7 NEON bezieht sich die Adressierung von verschiedenen Elementen innerhalb der Register auf die Art und Weise, wie Daten in den Q-, D- und S-Registern organisiert und durch Instruktionen verarbeitet werden. NEON ist darauf ausgelegt, mehrere Datenoperationen gleichzeitig durchzuführen, indem es Register in kleinere Datenstücke unterteilt.

### Adressierung in Instruktionen

#### 1. **Q-Register (128-Bit)**
- **Qn**: Wenn man ein Q-Register in einer Instruktion verwendet, wird das gesamte 128-Bit-Register angesprochen. Ein Q-Register kann als:
  - 16 x 8-Bit-Werte
  - 8 x 16-Bit-Werte
  - 4 x 32-Bit-Werte
  - 2 x 64-Bit-Werte
  behandelt werden, abhängig von der Art der Instruktion.
  
  **Beispiel**:
  ```assembly
  VADD.I8 Q0, Q1, Q2  @ Addiere 16 8-Bit-Werte von Q1 und Q2 und speichere das Ergebnis in Q0
  ```

#### 2. **D-Register (64-Bit)**
- **Dn**: Ein D-Register repräsentiert 64 Bit und kann als:
  - 8 x 8-Bit-Werte
  - 4 x 16-Bit-Werte
  - 2 x 32-Bit-Werte
  betrachtet werden. Es ist die halbe Größe eines Q-Registers.

  **Beispiel**:
  ```assembly
  VMUL.F32 D0, D1, D2  @ Multipliziere zwei 32-Bit-Gleitkommawerte in D1 und D2 und speichere das Ergebnis in D0
  ```

#### 3. **S-Register (32-Bit)**
- **Sn**: S-Register sind die kleinsten Einheiten und repräsentieren 32 Bit. Jedes D-Register enthält zwei S-Register:
  - D0 enthält S0 und S1
  - D1 enthält S2 und S3

  **Beispiel**:
  ```assembly
  VADD.F32 S0, S1, S2  @ Addiere zwei 32-Bit-Gleitkommawerte von S1 und S2 und speichere das Ergebnis in S0
  ```

### Umgang mit den verschiedenen Datenformaten in ARMv7 NEON

Die Art der Daten, die in den Registern gespeichert sind, wird durch die Instruktion bestimmt, nicht durch das Register selbst. Wenn eine Instruktion beispielsweise als `.I8`, `.I16`, `.I32`, oder `.F32` definiert ist, wird das Register entsprechend als Sammlung von 8-Bit-, 16-Bit-, 32-Bit- oder 64-Bit-Werten betrachtet.

**Beispiele für Instruktionssuffixe**:
- **`.I8`**: Verarbeitet die Register als Sammlung von 8-Bit-Werten.
- **`.I16`**: Verarbeitet die Register als Sammlung von 16-Bit-Werten.
- **`.F32`**: Verarbeitet die Register als 32-Bit-Gleitkommawerte.

|------------------------|-------------------------------|---------------------------------|
| [zurück](scalvekt.md)  | [Hauptmenü](../ueberblick.md) | [weiter](neonstat.md)           |


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