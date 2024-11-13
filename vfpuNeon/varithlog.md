# B.2 Erweiterungen der CPU-Funktionalität
## 2.3.15 VFP und NEON: Arithmetische und logische NEON-Operationen

### VAND: Vector AND
```asm
vand Vd, Vn, Vm
```
**V:** Muss entweder `q` oder `d` sein.

**Funktionsweise:**
Der Befehl **VAND** führt eine bitweise UND-Operation zwischen den entsprechenden Bits der Vektoren `Vn` und `Vm` durch und speichert das Ergebnis im Zielregister `Vd`.

**Beispiel:**
```asm
vand q0, q1, q2  @ q0 = q1 & q2
```
In diesem Beispiel werden `q1` und `q2` bitweise miteinander verundet, und das Ergebnis wird in `q0` gespeichert. 

### VEOR: Vector Exclusive OR
```asm
veor Vd, Vn, Vm
```
**V:** Muss entweder `q` oder `d` sein.

**Funktionsweise:**
Der Befehl **VEOR** führt eine bitweise exklusive OR-Operation (XOR) zwischen den entsprechenden Bits der Vektoren `Vn` und `Vm` durch und speichert das Ergebnis im Zielregister `Vd`.

**Beispiel:**
```asm
veor d0, d1, d2  @ d0 = d1 ^ d2
```
Hier werden `d1` und `d2` bitweise exklusiv ODER-verknüpft, und das Ergebnis wird in `d0` gespeichert. 

### VORR: Vector OR
```asm
vorr Vd, Vn, Vm
```
**type:** Bei bitweiser Logik egal  
**V:** Muss entweder `q` oder `d` sein.

**Funktionsweise:**
Der Befehl **VORR** führt eine bitweise ODER-Operation zwischen den entsprechenden Bits der Vektoren `Vn` und `Vm` durch und speichert das Ergebnis im Zielregister `Vd`.

**Beispiel:**
```asm
vorr q3, q4, q5  @ q3 = q4 | q5
```
In diesem Beispiel werden `q4` und `q5` bitweise logisch ODER-verknüpft, und das Ergebnis wird in `q3` gespeichert. 


### VADD: Vektoraddition
```asm
vadd{.<type>} Vd, Vn, Vm
```
**type:** Muss `i8`, `i16`, `i32` oder `i64` sein  
**V:** Muss entweder `q` oder `d` sein.

**Funktionsweise:**
Der Befehl **VADD** addiert die entsprechenden Elemente der Vektoren `Vn` und `Vm` und speichert die Ergebnisse im Zielregister `Vd`.

**Beispiel:**
```asm
vadd.i32 q0, q1, q2  @ q0[i] = q1[i] + q2[i] für jedes 32-Bit Element i
```
Hier werden die 32-Bit-Elemente von `q1` und `q2` addiert, und die Ergebnisse werden in den entsprechenden Elementen von `q0` gespeichert.

### VSUB: Vektorsubtraktion
```asm
vsub{.<type>} Vd, Vn, Vm
```
**type:** Muss `i8`, `i16`, `i32` oder `i64` sein  
**V:** Muss entweder `q` oder `d` sein.

**Funktionsweise:**
Der Befehl **VSUB** subtrahiert die entsprechenden Elemente des Vektors `Vm` von denen des Vektors `Vn` und speichert das Ergebnis im Zielregister `Vd`.

**Beispiel:**
```asm
vsub.i16 d1, d2, d3  @ d1[i] = d2[i] - d3[i] für jedes 16-Bit Element i
```
In diesem Beispiel werden die 16-Bit-Elemente von `d2` und `d3` subtrahiert, und die Ergebnisse werden in den entsprechenden Elementen von `d1` gespeichert.

### VMUL: Vektor-Multiplikation

#### Elementweise Multiplikation zweier Vektoren
```asm
vmul.<type> Vd, Vn, Vm
```
**type:** Muss `i8`, `i16` oder `i32` sein  
**V:** Muss entweder `q` oder `d` sein.

**Funktionsweise:**
Der Befehl **VMUL** multipliziert die entsprechenden Elemente der Vektoren `Vn` und `Vm` und speichert das Ergebnis im Zielregister `Vd`.

**Beispiel:**
```asm
vmul.i16 d3, d4, d5  @ d3[i] = d4[i] * d5[i] für jedes 16-Bit Element i
```
Hier werden die 16-Bit-Elemente von `d4` und `d5` multipliziert, und die Ergebnisse werden in den entsprechenden Elementen von `d3` gespeichert.

#### Multiplikation mit einem Skalar
```asm
vmul.<type> Vd, Vn, Dm[x]
```
**type:** Muss `i16`, `i32` oder `f32` sein  
**V:** Muss entweder `q` oder `d` sein.

**Funktionsweise:**
Der Befehl **VMUL** multipliziert jedes Element des Vektors `Vn` mit einem einzelnen Skalarwert aus dem Register `Dm[x]` und speichert das Ergebnis im Zielregister `Vd`.

**Beispiel:**
```asm
vmul.i16 q0, q1, d2[3]  @ q0[i] = q1[i] * d2[3] für jedes 16-Bit Element i
```
In diesem Beispiel wird jedes 16-Bit-Element von `q1` mit dem Skalarwert `d2[3]` multipliziert, und die Ergebnisse werden in den entsprechenden Elementen von `q0` gespeichert.

### VMLA: Vektor-Multiplikation und Akkumulation

#### Elementweise Multiplikation zweier Vektoren und Addition zum Zielvektor
```asm
vmla.<type> Vd, Vn, Vm
```
**type:** Muss `i8`, `i16` oder `i32` sein  
**V:** Muss entweder `q` oder `d` sein.

**Funktionsweise:**
Der Befehl **VMLA** multipliziert die entsprechenden Elemente der Vektoren `Vn` und `Vm` und addiert das Ergebnis zu den entsprechenden Elementen im Zielvektor `Vd`. Diese Operation wird für jedes Element im Vektor ausgeführt.

**Beispiel:**
```asm
vmla.i32 q0, q1, q2  @ q0[i] = q0[i] + (q1[i] * q2[i]) für jedes 32-Bit Element i
```
Hier wird jedes 32-Bit-Element in `q0` um das Produkt der entsprechenden Elemente in `q1` und `q2` erhöht.

#### Multiplikation mit einem Skalar
```asm
vmla.<type> Vd, Vn, Dm[x]
```
**type:** Muss `i16`, `i32` oder `f32` sein  
**V:** Muss entweder `q` oder `d` sein.

**Funktionsweise:**
Der Befehl **VMLA** multipliziert jedes Element des Vektors `Vn` mit einem einzelnen Skalarwert aus dem Register `Dm[x]` und addiert das Ergebnis zu den entsprechenden Elementen im Zielvektor `Vd`.

**Beispiel:**
```asm
vmla.f32 q0, q1, d2[1]  @ q0[i] = q0[i] + (q1[i] * d2[1]) für jedes f32 Element i
```
In diesem Beispiel wird jedes Gleitkomma-Element von `q1` mit dem Skalarwert `d2[1]` multipliziert und das Ergebnis zu den entsprechenden Elementen von `q0` addiert.

### VABS: Absoluter Wert
```asm
vabs{.<type>} Vd, Vm
```
**type:** Muss `i8`, `i16`, `i32`, `f32`, oder `f64` sein  
**V:** Muss entweder `q` oder `d` sein.

**Funktionsweise:**
Der Befehl **VABS** berechnet den absoluten Wert jedes Elements im Vektorregister `Vm` und speichert das Ergebnis im Zielregister `Vd`. Der absolute Wert eines Wertes ist der Abstand dieses Wertes zu null, d.h: negative Werte werden positiv.

**Beispiel:**
```asm
vabs.i32 q0, q1  @ q0[i] = |q1[i]| für jedes 32-Bit Element i
```
In diesem Beispiel wird der absolute Wert jedes 32-Bit-Elements in `q1` berechnet und in den entsprechenden Elementen von `q0` gespeichert.

### VNEG: Negation
```asm
vneg{.<type>} Vd, Vm
```
**type:** Muss `i8`, `i16`, `i32`, `f32`, oder `f64` sein  
**V:** Muss entweder `q` oder `d` sein.

**Funktionsweise:**
Der Befehl **VNEG** negiert jedes Element im Vektorregister `Vm` und speichert das Ergebnis im Zielregister `Vd`. Die Negation eines Wertes bedeutet, dass das Vorzeichen des Wertes umgekehrt wird (positive Werte werden negativ und umgekehrt).

**Beispiel:**
```asm
vneg.i16 d2, d3  @ d2[i] = -d3[i] für jedes 16-Bit Element i
```
Hier wird jedes 16-Bit-Element von `d3` negiert, und das Ergebnis wird in den entsprechenden Elementen von `d2` gespeichert.

### VMAX: Maximum
```asm
vmax{.<type>} Vd, Vn, Vm
```
**type:** Muss `i8`, `i16`, `i32`, `f32`, oder `f64` sein  
**V:** Muss entweder `q` oder `d` sein.

**Funktionsweise:**
Der Befehl **VMAX** vergleicht die entsprechenden Elemente der Vektoren `Vn` und `Vm` und speichert den jeweils größeren Wert in den entsprechenden Elementen des Zielregisters `Vd`.

**Beispiel:**
```asm
vmax.i32 q1, q2, q3  @ q1[i] = max(q2[i], q3[i]) für jedes 32-Bit Element i
```
In diesem Beispiel wird jedes 32-Bit-Element von `q2` mit dem entsprechenden Element von `q3` verglichen. Der größere Wert wird in den entsprechenden Elementen von `q1` gespeichert.

### VMIN: Minimum
```asm
vmin{.<type>} Vd, Vn, Vm
```
**type:** Muss `i8`, `i16`, `i32`, `f32`, oder `f64` sein  
**V:** Muss entweder `q` oder `d` sein.

**Funktionsweise:**
Der Befehl **VMIN** vergleicht die entsprechenden Elemente der Vektoren `Vn` und `Vm` und speichert den jeweils kleineren Wert in den entsprechenden Elementen des Zielregisters `Vd`.

**Beispiel:**
```asm
vmin.i16 d0, d1, d2  @ d0[i] = min(d1[i], d2[i]) für jedes 16-Bit Element i
```
Hier wird jedes 16-Bit-Element von `d1` mit dem entsprechenden Element von `d2` verglichen. Der kleinere Wert wird in den entsprechenden Elementen von `d0` gespeichert.

|-------------------------|-------------------------------|------------------------|
| [zurück](neonldstr.md)  | [Hauptmenü](../ueberblick.md) | [weiter](vtrn.md)      |


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