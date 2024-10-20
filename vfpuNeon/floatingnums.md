# B.2 Erweiterungen der CPU-Funktionalität
## 2.3.3 VFP und NEON: Floating Point Format nach IEEE 754

Die binäre Codierung einer Gleitkommazahl nach IEEE 754 besteht aus 3 Feldern
- **Mantisse:** Das Mantissenfeld repräsentiert den Bruchteil einer Zahl
- **Exponent:** Der Exponent gibt die Position des binären "Dezimalpunkts" an, der die Größenordnung bestimmt
- **Vorzeichenbit:** selbsterklärend

Bei single precision 32-bit Gleitkommazahlen hat die Mantisse 23 Bit, der Exponent 8 Bit und das Vorzeichenbit besteht aus 1 Bit

### Konvertierung einer binären Gleitkommazahl in das single Precision Floating Point format:

#### 1. Normalisierung 

Das **Normalisieren** im Kontext von Floating-Point-Zahlen (Gleitkommazahlen) bezieht sich auf den Prozess, bei dem die Mantisse einer Zahl so angepasst wird, dass der führende Stellenwert vor dem Komma (oder im Falle der binären Darstellung: vor dem Punkt) eine 1 ist. Dies wird als **normalisierte Form** bezeichnet. Entsprechend muss auch der Exponent angepasst werden, um den tatsächlichen Wert der Zahl beizubehalten. Da die führende 1 der Mantisse bei der normalisierten Form vorausgesetzt wird, kann sie in der Darstellung der Zahl letzlich weggelassen werden. 

#### Warum ist Normalisierung wichtig?
Die Normalisierung ist entscheidend, weil sie sicherstellt, dass die Gleitkommazahlen auf eine standardisierte Weise gespeichert werden. Dies hat zwei wesentliche Vorteile:
- **Maximale Präzision**: Durch die Normalisierung wird sichergestellt, dass die Mantisse die maximale Anzahl von signifikanten Bits enthält, was zu einer genaueren Darstellung der Zahl führt.  
- **Einheitlichkeit**: Eine normalisierte Darstellung erleichtert den Vergleich und die Berechnung von Gleitkommazahlen, da die Zahlen in einem einheitlichen Format vorliegen.

**Beispiel:**
Die Dezimalzahl `9.75` enstpricht der binären Gleitkommazahl `1001.110`.
Normalisiert man diese Gleitkommazahl, dann erhält man die Zahl `1.001110` mit dem Exponenten `3`.

#### 2. Exponentenbias berücksichtigen

##### Was bedeutet Bias in diesem Kontext?
In der Gleitkommazahlendarstellung nach IEEE 754 wird der Exponent in einer bestimmten Form gespeichert, um sowohl positive als auch negative Exponenten abbilden zu können. Anstatt den Exponenten direkt in seiner Zwei-Komplement-Darstellung (wie bei ganzen Zahlen) zu speichern, wird ein sogenannter Bias verwendet.

##### Bias in Single Precision
In der Single-Precision-Darstellung (32 Bit) werden 8 Bits für den Exponenten reserviert. Der Bias-Wert für diesen Exponenten beträgt **127**. Dies bedeutet, dass der tatsächliche Exponent, der im Speicher abgelegt wird, um 127 verschoben wird. Diese Verschiebung wird als **Bias** bezeichnet.

##### Berechnung des gespeicherten Exponenten
Der im Exponentenfeld gespeicherte Exponent entspricht dem `tatsächlichen Exponent + 127`.

##### Warum Bias?
Der Vorteil der Verwendung eines Bias besteht darin, dass der Exponent immer als eine positive Zahl dargestellt werden kann, was die Handhabung und Speicherung vereinfacht. Dies ermöglicht es, sowohl sehr kleine als auch sehr große Zahlen in einem standardisierten und effizienten Format zu speichern.

**Beispiel:**
Die Dezimalzahl `9.75` enstpricht der normalisierten Gleitkommazahl `1.001110` mit dem Exponenten `3`.
Um den Exponent nach IEEE 754 zu bestimmen muss man den Bias berücksichtigen, sprich `3` + `127`.

#### 3. Umwandlung fertigstellen
Zum Abschluss der Umwandlung in das Single Precision Floating Point Format müssen die entsprechenden 32-Bit-Felder belegt werden. Die Anordnung der Bits erfolgt wie folgt:

**single Precision Floating Point Format**

|31 |30 ... 23|22 ...   0|  
|---|---------|----------|
| S | Exponent| Mantisse | 


**Beispiel:** Bei der Zahl 9.75:
- Da die Zahl positiv ist, ist das S-Bit 0.
- Der Exponent 130 wird als 10000010 in Binär dargestellt.
- Die Mantisse beginnt mit 0011100, die führende 1 wird verworfen und der Rest wird mit Nullen aufgefüllt.

|S | Exponent | Mantisse                |  
|--|----------|-------------------------|
| 0| 10000010 | 00111000000000000000000 |

|------------------------|-------------------------------|----------------------------------|
| [zurück](bingleit.md)  | [Hauptmenü](../ueberblick.md) | [weiter](vfp_intro.md)           | 


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