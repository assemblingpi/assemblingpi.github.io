# B.2 Erweiterungen der CPU-Funktionalität
## 2.3.16 VFP und NEON: VTRN (Vector Transpose) Instruktionen

Die **VTRN**-Instruktionen dienen dazu, Elemente zwischen zwei Vektorregistern zu vertauschen. Diese Befehle sind besonders nützlich, wenn es darum geht, die Daten innerhalb von Registern so anzuordnen, dass sie für nachfolgende Verarbeitungsschritte optimal positioniert sind, zum Beispiel in der Bild- oder Audiodatenverarbeitung.

### **VTRN (Vector Transpose)**
Die **VTRN**-Instruktion behandelt die Daten in zwei Vektorregistern als Elemente von 2x2-Matrizen und transponiert diese Matrizen. Dies bedeutet, dass sie die Positionen der Elemente in den beiden Vektoren paarweise vertauscht. Dadurch entstehen zwei neue Vektoren, in denen die Elemente aus den ursprünglichen Vektoren neu angeordnet sind.

### Varianten von VTRN
Es gibt drei Hauptvarianten der VTRN-Instruktion, je nach Größe der Elemente, die transponiert werden sollen:

- **VTRN.8**: Diese Variante transponiert 8-Bit-Elemente. Das ist besonders nützlich, wenn Sie beispielsweise Bytes innerhalb von Vektoren vertauschen müssen.
- **VTRN.16**: Diese Variante transponiert 16-Bit-Elemente, was hilfreich sein kann, um zum Beispiel Halbwortpaare zu vertauschen.
- **VTRN.32**: Diese Variante transponiert 32-Bit-Elemente, wodurch ganze Wörter zwischen Vektoren getauscht werden können.

### Grundprinzip der VTRN-Instruktion
```
vtrn.size vektor1, vektor2
```
Unabhängig von der gewählten Elementbreite bleibt das grundlegende Prinzip der Transponierung bei der VTRN-Instruktion unverändert. Es werden zwei Vektoren benötigt, die jeweils vier Elemente enthalten. Dabei wird das Element an Position 1 des ersten Vektors mit dem Element an Position 0 des zweiten Vektors vertauscht, und das Element an Position 3 des ersten Vektors wird mit dem Element an Position 2 des zweiten Vektors ausgetauscht.

### Beispiel: Transponieren von 16-Bit-Elementen
Angenommen, zwei Vektorregister `D0` und `D1` enthalten jeweils vier 16-Bit-Elemente. Bei Verwendung des Befehls `VTRN.16 D0, D1` wird Elemente von `D0` mit dem entsprechenden Element von `D1` vertauscht. 

**Vor VTRN.16 D0, D1**

|d0[0] |d0[1] |d0[2] |d0[3] |
|------|------|------|------|
| 0x01 | 0x02 | 0x03 | 0x04 |

|d1[0] |d1[1] |d1[2] |d1[3] |
|------|------|------|------|
| 0x0a | 0x0b | 0x0c | 0x0d |

**Nach VTRN.16 D0, D1**

|d0[0] |d0[1] |d0[2] |d0[3] |
|------|------|------|------|
| 0x01 | 0x0a | 0x03 | 0x0c |

|d1[0] |d1[1] |d1[2] |d1[3] |
|------|------|------|------|
| 0x02 | 0x0b | 0x04 | 0x0d |

### Mehrfaches Transponieren für größere Matrizen
Um größere Matrizen, wie eine 4x4-Matrix aus 16-Bit-Elementen, zu transponieren, können mehrere VTRN-Befehle in Folge ausgeführt werden. Eine solche Transponierung lässt sich beispielsweise durch die Anwendung von drei aufeinanderfolgenden VTRN-Befehlen erreichen, wobei nacheinander verschiedene Paare von Vektoren vertauscht werden.

|-------------------------|-------------------------------|-----------------------------|
| [zurück](varithlog.md)  | [Hauptmenü](../ueberblick.md) | [weiter](trigon_ue.md)      |


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