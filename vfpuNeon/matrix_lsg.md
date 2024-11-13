# B.2 Erweiterungen der CPU-Funktionalität
## 2.3.18 VFP und NEON: Lösung zur Implementierung einer 4x4-Matrixmultiplikationsfunktion mit NEON

### **Einleitung zur Matrizenmultiplikation**

In der 4x4-Matrizenmultiplikation berechnet sich jedes Element der Ergebnis-Matrix durch das Skalarprodukt der entsprechenden Zeile der ersten Matrix `matrix_a` mit der jeweiligen Spalte der zweiten Matrix `matrix_B`. Dies bedeutet, dass für jedes Element der Ergebnis-Matrix die Summe der Produkte der passenden Elemente der Zeile und Spalte gebildet wird:


`c(ij)` **=** `a(i1)` **x** `b(1j)` **+** `a(i2)` **x** `b(2j)` **+** `a(i3)` **x** `b(3j)` **+** `a(i4)` **x** `b(4j)`


Da der gegebene Code die SIMD-Instruktionen von NEON verwendet, müssen die Berechnungen für mehrere Elemente **parallel** durchgeführt werden. Um dies zu ermöglichen, wird die Matrix `matrix_B` zunächst transponiert. Dies erlaubt es, die Spalten von `matrix_B` als Zeilen zu behandeln, was die parallele Verarbeitung durch SIMD erleichtert.

### Initialisierung der Daten

```
.data
matrix_b_tr: .word 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00
```

Im Datenabschnitt wird der Speicher für die transponierte Version von `matrix_B` reserviert. Diese transponierte Matrix `matrix_b_tr` wird später verwendet, um die Spalten von `matrix_B` als Zeilen zu verarbeiten.


### Hauptfunktion: Matrix4x4_mul

```
.text        
Matrix4x4_mul:       @ r0 = ptr auf matrix_c, r1 = ptr auf matrix_a, r2 = ptr auf matrix_B
    push {lr}
    push {r4-r6}	
    vpush {d8-d15}
```

Zu Beginn sichert der Code das Link-Register (`lr`), allgemeine Register (`r4-r6`) und NEON-Register (`d8-d15`).

#### Laden der Matrix A

```
    vld1.32 {q8}, [r1]!
    vld1.32 {q9}, [r1]!
    vld1.32 {q10}, [r1]!
    vld1.32 {q11}, [r1]!
```

Die vier Zeilen von `matrix_a` werden in die NEON-Register `q8` bis `q11` geladen. Dies bedeutet, dass `q8` die erste Zeile, `q9` die zweite Zeile und so weiter enthält. Durch den Suffix `!` wird der Zeiger `r1` nach jeder Ladung um die entsprechende Größe der Daten (eine Zeile) inkrementiert.

#### Transponierung von matrix_B

```
    ldr r1, =matrix_b_tr
    push {r0}
    mov r0, r2
    bl matr4_transp
    mov r2, r0
    pop {r0}
    mov r3, r0
```

Hier wird die Adresse von `matrix_b_tr` in `r1` geladen. Danach wird `matrix_B` transponiert, indem die Funktion `matr4_transp` aufgerufen wird. Diese Transponierung ist notwendig, um die Spalten von `matrix_B` in eine Form zu bringen, die parallel mit den Zeilen von `matrix_a` multipliziert werden kann. Die transponierte Matrix wird in `matrix_b_tr` gespeichert, und `r2` wird auf die Adresse der transponierten Matrix gesetzt.

### Laden der transponierten Matrix B

```
    vld1.32 {q12}, [r2]!
    vld1.32 {q13}, [r2]!
    vld1.32 {q14}, [r2]!
    vld1.32 {q15}, [r2]
```

Die transponierte Matrix `matrix_b_tr` wird in die Register `q12` bis `q15` geladen. Diese Register repräsentieren nun die Spalten von `matrix_B` als Zeilen, was für die weitere SIMD-Verarbeitung notwendig ist.

### Berechnung der ersten Zeile der Ergebnis-Matrix

```
    @ zeile 1 x spalte 1
    vmul.i32 q0, q8, q12 
    @ zeile 1 x spalte 2
    vmul.i32 q1, q8, q13    
    @ zeile 1 x spalte 3
    vmul.i32 q2, q8, q14
    @ zeile 1 x spalte 4
    vmul.i32 q3, q8, q15
```

Die erste Zeile von `matrix_a` (in `q8`) wird nun mit **allen** transponierten Zeilen von `matrix_B` multipliziert, was den einzelnen Spalten von `matrix_B` entspricht. Jede dieser Multiplikationen speichert das Ergebnis in einem NEON-Register (`q0` bis `q3`). Nach diesen Operationen enthält jedes dieser Register die Produkte der ersten Zeile von `matrix_a` mit den jeweiligen Spalten von `matrix_B`.

#### Transponierung der Produktmatrix

```
    vtrn.32 q0, q1
    vtrn.32 q2, q3
    vmov d8, d1
    vmov d9, d4
    vswp d8, d9
    vmov d1, d8
    vmov d4, d9
    vmov d10, d3
    vmov d11, d6
    vswp d10, d11
    vmov d3, d10
    vmov d6, d11
```

Nach der Multiplikation befinden sich die Produkte in einer Anordnung, die **nicht** direkt parallel addiert werden kann, um die Elemente der Ergebnis-Matrix zu berechnen. Daher werden die Register mit den Produkten transponiert, damit die Elemente der Lanes korrekt für die anschließende parallele Addition vorbereitet sind.

### Addition und Speicherung der ersten Zeile der Ergebnis-Matrix

```
    vadd.i32 q4, q0, q1
    vadd.i32 q5, q2, q3
    vadd.i32 q6, q4, q5
    vst1.32 {q6}, [r3]!
```

Die Produkte in den transponierten Registern werden nun parallel aufaddiert, um die endgültigen Werte der ersten Zeile der Ergebnis-Matrix `matrix_c` zu erhalten. Die Summen werden im Register `q6` gespeichert und anschließend in den Speicher für die erste Zeile von `matrix_c` geschrieben.

---

### Berechnung der restlichen Zeilen

Dieser gesamte Prozess (Multiplikation, Transponierung, Addition) wird nun für die restlichen Zeilen von `matrix_a` wiederholt:

```
    @ zeile 2 x spalte 1
    vmul.i32 q0, q9, q12 
    @ zeile 2 x spalte 2
    vmul.i32 q1, q9, q13    
    @ zeile 2 x spalte 3
    vmul.i32 q2, q9, q14
    @ zeile 2 x spalte 4
    vmul.i32 q3, q9, q15
```

Für die zweite Zeile von `matrix_a` (gespeichert in `q9`) wird ebenfalls jede Spalte von `matrix_B` (entsprechend den transponierten Zeilen in `q12` bis `q15`) multipliziert. Die Ergebnisse werden auf ähnliche Weise wie bei der ersten Zeile transponiert, addiert und gespeichert.

```
    @ transponiere produktmatrix
    vtrn.32 q0, q1
    vtrn.32 q2, q3
    vmov d8, d1
    vmov d9, d4
    vswp d8, d9
    vmov d1, d8
    vmov d4, d9
    vmov d10, d3
    vmov d11, d6
    vswp d10, d11
    vmov d3, d10
    vmov d6, d11
    vadd.i32 q4, q0, q1
    vadd.i32 q5, q2, q3
    vadd.i32 q6, q4, q5
    vst1.32 {q6}, [r3]!
```

Dieser Ablauf wiederholt sich für die dritte und vierte Zeile von `matrix_a`:

```
    @ zeile 3 x spalte 1
    vmul.i32 q0, q10, q12 
    @ zeile 3 x spalte 2
    vmul.i32 q1, q10, q13    
    @ zeile 3 x spalte 3
    vmul.i32 q2, q10, q14
    @ zeile 3 x spalte 4
    vmul.i32 q3, q10, q15
    @ transponiere produktmatrix
    vtrn.32 q0, q1
    vtrn.32 q2, q3
    vmov d8, d1
    vmov d9, d4
    vswp d8, d9
    vmov d1, d8
    vmov d4, d9
    vmov d10, d3
    vmov d11, d6
    vswp d10, d11
    vmov d3, d10
    vmov d6, d11
    vadd.i32 q4, q0, q1
    vadd.i32 q5, q2, q3
    vadd.i32 q6, q4, q5
    vst1.32 {q6}, [r3]!

    @ zeile 4 x spalte 1
    vmul.i32 q0, q11, q12 
    @ zeile 4 x spalte 2
    vmul.i32 q1, q11, q13    
    @ zeile 4 x spalte 3
    vmul.i32 q2, q11, q14
    @ zeile 4 x spalte 4
    vmul.i32 q3, q11, q15
    @ transponiere produktmatrix
    vtrn.32 q0, q1
    vtrn.32 q2, q3
    vmov d8, d1
    vmov d9, d4
    vswp d8, d9
    vmov d1, d8
    vmov d4, d9
    vmov d10, d3
    vmov d11, d6
    vswp d10, d11
    vmov d3, d10
    vmov d6, d11
    vadd.i32 q4, q0, q1
    vadd.i32 q5, q2, q3
    vadd.i32 q6, q4, q5
    vst1.32 {q6}, [r3]
```

### Abschluss der Funktion

```
    vpop {d8-d15}    
    pop {r4-r6}    
    pop {pc}
```

Am Ende werden alle gesicherten Register wiederhergestellt, und der Prozessor kehrt zu der aufrufenden Funktion zurück.

### Transponierungsfunktion: matr4_transp

```
matr4_transp: @ Transponiere 4x4 Matrix r0 = Matrix_in r1=Matrix_out
    push {lr}
    push {r0}	
    vld1.32 {q0}, [r0]!
    vld1.32 {q1}, [r0]!
    vld1.32 {q2}, [r0]!
    vld1.32 {q3}, [r0]!
    vtrn.32 q0, q1
    vtrn.32 q2, q3
    vmov d8, d1
    vmov d9, d4
    vswp d8, d9
    vmov d1, d8
    vmov d4, d9
    vmov d10, d3
    vmov d11, d6
    vswp d10, d11
    vmov d3, d10
    vmov d6, d11
    pop {r0}
    mov r0, r1
    vst1.32 {q0}, [r1]!
    vst1.32 {q1}, [r1]!
    vst1.32 {q2}, [r1]!
    vst1.32 {q3}, [r1]
    pop	{pc}
```

Die Funktion `matr4_transp` lädt die vier Zeilen der Eingabematrix `matrix_B`, transponiert sie, sodass die Spalten als Zeilen vorliegen, und speichert das Ergebnis in `matrix_b_tr`.


|-------------------------|-------------------------------|-----------------------------------|
| [zurück](matrix_ue.md)  | [Hauptmenü](../ueberblick.md) | [weiter](../GPU/grafikintro.md)   |


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