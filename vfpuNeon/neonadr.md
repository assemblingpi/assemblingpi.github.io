## Registeradressierung in NEON

In ARMv7 NEON bezieht sich die Adressierung von verschiedenen Elementen innerhalb der Register auf die Art und Weise, wie Daten in den Q-, D- und S-Registern organisiert und durch Instruktionen verarbeitet werden. NEON ist darauf ausgelegt, mehrere Datenoperationen gleichzeitig durchzuführen, indem es Register in kleinere Datenstücke unterteilt.

### Registerstruktur in ARMv7 NEON

ARMv7 NEON stellt drei Hauptregistertypen zur Verfügung:
- **Q-Register (128 Bit)**: Diese bestehen aus zwei D-Registern. Es gibt 16 Q-Register (Q0 bis Q15).
- **D-Register (64 Bit)**: Diese können als eigenständige 64-Bit-Register verwendet werden oder als Paare, die ein Q-Register bilden. Es gibt 32 D-Register (D0 bis D31).
- **S-Register (32 Bit)**: Diese sind die kleinsten adressierbaren Einheiten. Jedes D-Register besteht aus zwei S-Registern. Es gibt 32 S-Register (S0 bis S31), die auf die D-Register D0 bis D15 abgebildet sind.

### Adressierung in Instruktionen

#### 1. **Q-Register (128-Bit)**
- **Qn**: Wenn man ein Q-Register in einer Instruktion verwendet, wird das gesamte 128-Bit-Register angesprochen. Ein Q-Register kann als eine Sammlung von:
  - 16 8-Bit-Werten
  - 8 16-Bit-Werten
  - 4 32-Bit-Werten
  - 2 64-Bit-Werten
  behandelt werden, abhängig von der Art der Instruktion.
  
  **Beispiel**:
  ```assembly
  VADD.I8 Q0, Q1, Q2  ; Addiere 16 8-Bit-Werte von Q1 und Q2 und speichere das Ergebnis in Q0
  ```

#### 2. **D-Register (64-Bit)**
- **Dn**: Ein D-Register repräsentiert 64 Bit und kann als Sammlung von:
  - 8 8-Bit-Werten
  - 4 16-Bit-Werten
  - 2 32-Bit-Werten
  betrachtet werden. Es ist die halbe Größe eines Q-Registers.

  **Beispiel**:
  ```assembly
  VMUL.F32 D0, D1, D2  ; Multipliziere zwei 32-Bit-Gleitkommawerte in D1 und D2 und speichere das Ergebnis in D0
  ```

#### 3. **S-Register (32-Bit)**
- **Sn**: S-Register sind die kleinsten Einheiten und repräsentieren 32 Bit. Jedes D-Register enthält zwei S-Register:
  - D0 enthält S0 und S1
  - D1 enthält S2 und S3

  **Beispiel**:
  ```assembly
  VADD.F32 S0, S1, S2  ; Addiere zwei 32-Bit-Gleitkommawerte von S1 und S2 und speichere das Ergebnis in S0
  ```

### Umgang mit den verschiedenen Datenformaten in ARMv7 NEON

Die Art der Daten, die in den Registern gespeichert sind, wird durch die Instruktion bestimmt, nicht durch das Register selbst. Wenn eine Instruktion beispielsweise als `.I8`, `.I16`, `.I32`, oder `.F32` definiert ist, wird das Register entsprechend als Sammlung von 8-Bit-, 16-Bit-, 32-Bit- oder 64-Bit-Werten betrachtet.

**Beispiele für Instruktionssuffixe**:
- **`.I8`**: Verarbeitet die Register als Sammlung von 8-Bit-Werten.
- **`.I16`**: Verarbeitet die Register als Sammlung von 16-Bit-Werten.
- **`.F32`**: Verarbeitet die Register als 32-Bit-Gleitkommawerte.

