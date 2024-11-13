# B.2 Erweiterungen der CPU-Funktionalität
## 2.3.13 VFP und NEON: Datentransfer

### VMOV: Datentransfer zu/von NEON-Registern
Der Befehl VMOV in der ARM-Architektur steht für **Vector Move**. Er wird verwendet, um Daten zwischen verschiedenen Registern zu verschieben, insbesondere zwischen ARM- und NEON-Registern. Dabei können verschiedene Datentypen und Registergrößen involviert sein, aber es findet keine Datenkonvertierung statt – die Bits werden einfach kopiert.

#### VMOV: 32-Bit Datentransfer 
Dieser Befehl verschiebt Daten zwischen einem 32-Bit-ARM-Register und einem 32-Bit-NEON-Register.

**Syntax:**
```
vmov {<cond>} Rd, Sn    @ move Sn to Rd
vmov {<cond>} Sn, Rd    @ move Rd to Sn
```
- **cond:** Bedingte Ausführung (optional)
- **Rd:** ein ARM-Integer Register
- **Sd:** eines der 32-Bit Single Precision Register

##### Beispiele
**Transfer von einem NEON-Register zu einem ARM-Register:**

```asm
vmov R0, S1     @ Überträgt den 32-Bit-Wert aus dem NEON-Register S1 in das ARM-Register R0
```
**Transfer von einem ARM-Register zu einem NEON-Register:**

```asm
vmov S2, R3     @ Überträgt den 32-Bit-Wert aus dem ARM-Register R3 in das NEON-Register S2
```


#### VMOV: 64-Bit Datentransfer 
Dieser Befehl ermöglicht den Transfer von 64-Bit-Daten zwischen zwei 32-Bit-ARM-Registern und einem 64-Bit-NEON-Register.

```asm
vmov {<cond>} Dd, Rl, Rh    @ move Rh and Rl to Dd
vmov {<cond>} Rl, Rh, Dd    @ move Dd to Rl and Rh
```
- **cond:** Bedingte Ausführung (optional)
- **Rl:** ein ARM-Integer Register, das die niedrigwertigsten 32-Bit beinhaltet
- **Rh:** ein ARM-Integer Register, das die höchstwertigsten 32-Bit beinhaltet
- **Dd:** eines der 64-Bit Vector-Register

##### Beispiele
**Transfer von zwei ARM-32-Bit-Registern in ein 64-Bit-NEON-Register:**
```asm
vmov D0, R1, R2  @ Überträgt die Werte aus den ARM-Registern R1 (niedrigwertig) und R2 (höchstwertig) in das NEON-Register D0
```

**Transfer von einem 64-Bit-NEON-Register in zwei ARM-32-Bit-Register:**
```asm
vmov R4, R5, D1  @ Überträgt den 64-Bit-Wert aus D1 in die ARM-Register R4 (niedrigwertig) und R5 (höchstwertig)
```

### VMOV: Datentransfer zwischen NEON-Skalar und Integer Register
Dieser Befehl ermöglicht den Transfer zwischen einem NEON-Skalar und einem ARM-Register.

```
vmov {<cond>}.<size> Dd[x], Rs      @ move Rs to Dd[x]
vmov {<cond>}.<size> Rd, Ds[x]      @ move Ds[x] to Rd
```

- **cond:** Bedingte Ausführung (optional)
- **size:** Gibt die Größe in Bits an, die übertragen werden soll. Typische Werte sind 8, 16, 32, oder 64. Es werden immer die niederwertigsten Bits dieser Größe übertragen.
- **Rd/Rs:** ein ARM-Integer Register
- **Ds/Dd[x]:** Ein Skalarwert innerhalb eines NEON-Vektorregisters. Dd und Ds stehen für die NEON-Register D0 bis D31. Der Index x gibt das spezifische Element innerhalb des Registers an, auf das zugegriffen wird.

#### Beispiele
**Transfer von einem ARM-Register in ein Skalarwert innerhalb eines NEON-Registers:**

```asm
vmov D3[0], R6  @ Überträgt den Wert aus R6 in das erste Element (Skalar) des NEON-Registers D3
```

**Transfer von einem Skalarwert innerhalb eines NEON-Registers in ein ARM-Register:**

```asm
vmov R7, D4[1]  @ Überträgt den Wert aus dem zweiten Element des NEON-Registers D4 in das ARM-Register R7
```

### VMOV: Datentransfer eines immediate Wertes
NEON-MOV-Befehle beinhalten die Möglichkeit immediate Werte in ein NEON-Register zu kopieren.

```asm 
vmov.<type> Vd, #imm    @ MOV immediate to Vd
vmvn.<type> Vd, #imm    @ MOV negated immediate to Vd
```

**type:** Gibt die Größe der Elemente im Vektor an. Dieser Typ muss einer der folgenden sein:
- i8: 8-Bit Integer
- i16: 16-Bit Integer
- i32: 32-Bit Integer
- f32: 32-Bit Floating-Point
- f64: 64-Bit Floating-Point

**Vd:** Das Zielregister. Dies kann eines der folgenden Register sein:
- S für ein 32-Bit Floating-Point Register
- D für ein 64-Bit Register
- Q für ein 128-Bit Register

**imm:** Der Immediate-Wert, der in **jedes** Element des Zielregisters kopiert wird.

#### Beispiele
**Kopieren eines Immediate-Wertes in ein 64-Bit NEON-Register:**

```asm
vmov.i32 D0, #0xFF      @ Kopiert den 32-Bit Wert 0xFF in jedes 32-Bit Element des D0-Registers
```

**Kopieren eines negierten Immediate-Wertes in ein 128-Bit NEON-Register:**

```asm
vmvn.i16 Q1, #0x1F      @ Kopiert den negierten 16-Bit Wert (0xFFE0) in jedes 16-Bit Element des Q1-Registers
```

**Kopieren eines Floating-Point-Wertes in ein 32-Bit NEON-Register:**

```asm
vmov.f32 S2, #1.5       @ Kopiert den 32-Bit Floating-Point Wert 1.5 in das S2-Register
```

**Kopieren eines Immediate-Wertes in ein 128-Bit Vektorregister:**

```asm
vmov.i8 Q0, #0xAB       @ Kopiert den 8-Bit Wert 0xAB in jedes 8-Bit Element des Q0-Registers
```

|-------------------------|-------------------------------|-----------------------------|
| [zurück](neoninstr.md)  | [Hauptmenü](../ueberblick.md) | [weiter](neonldstr.md)      |


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