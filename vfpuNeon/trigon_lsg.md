# B.2 Erweiterungen der CPU-Funktionalität
## 2.3.17 VFP und NEON: Implementierung von Trigonometrischen Funktionen (Lösung)

Der folgende Code implementiert die trigonometrischen Funktionen `sin`, `cos` und `tan` mithilfe der Taylor-Reihe. 


### Globale Deklarationen und Datenbereich

```assembly
.global sine
.global cos
.global tan

.data
halfpi: .float  1.5707963
pi:     .float  3.1415927        @ π
twopi:  .float  6.2831855        @ 2π

@ Tabelle der Koeffizienten für die Taylor-Reihe von sin(x) um den Punkt 0
taylorcoeff_table: 
.float -1.6666666666666667e-01   @ -x^3  / 3!
.float  8.3333333333333333e-03   @  x^5  / 5!
.float -1.9841269841269841e-04   @ -x^7  / 7!
.float  2.7557319223985893e-06   @  x^9  / 9!
.float -2.5052108385441717e-08   @ -x^11 / 11!
.float  1.6059043836821613e-10   @  x^13 / 13!
.float -7.6471637318198164e-13   @ -x^15 / 15!
.float  2.8114572543455206e-15   @  x^17 / 17!
```

- **Globale Deklaration:** Machen die trigonometrischen Funktionen für andere Module zugänglich.
- **Konstanten:**
  - `halfpi`, `pi`, `twopi`: Wichtige Winkelwerte in Radiant.
- **Taylor-Koeffizienten:** `taylorcoeff_table` enthält die Koeffizienten der Taylor-Reihe zur Approximation von `sin(x)` bis zur Potenz **x^17**.

### Trigonometrische Funktionen

#### Funktion sine

```assembly
sine:                           @ input in s0 (f32), Ergebnis in s0 (f32) [rad]
    vpush {d8-d15}     
    @ Eingangsbereich auf [-π, π] reduzieren
    ldr r0, =twopi   
    vldr s2, [r0]    
    vdiv.f32 s1, s0, s2
    vcvt.s32.f32 s1, s1 
    vcvt.f32.s32 s1, s1
    vmul.f32 s1, s1, s2
    vsub.f32 s0, s0, s1
    ldr r0, =pi   
    vldr s2, [r0]  
    vcmp.f32 s2, s0
    vmrs APSR_nzcv, FPSCR
    bpl lesserpi
    ldr r0, =twopi
    vldr s2, [r0] 
    vsub.f32 s0, s0, s2         @ xmod > π: xmod = xmod - 2π 
```
Die Funktion `sine` berechnet den Sinus eines Gleitkommawerts, der über das Register `s0` übergeben wird. Zunächst sichert die Funktion die relevanten Gleitkommaregister. Anschließend wird der Eingabewert in `s0` durch eine Modulo-Operation auf den Bereich **`[0, 2π)`** reduziert. Dazu wird der Wert in `s0` durch `2π` geteilt, und das Vielfache von `2π`, das aus dieser Division resultiert, wird vom ursprünglichen Wert in `s0` abgezogen. Auf diese Weise wird der Wert `x` auf den Bereich `[0, 2π)` begrenzt.

Danach überprüft die Funktion, ob der reduzierte Wert größer als `π` ist. Falls ja, wird `2π` subtrahiert, sodass `x` in das Intervall `[-π, π]` verschoben wird.


```
lesserpi:
    @ Laden der Taylor-Koeffizienten
    ldr r0,=taylorcoeff_table
    vld1.32 {q6}, [r0]!
    vld1.32 {q7}, [r0]
    
    @ Berechnung der Potenzen von x
    vmul.f32 s1,  s0, s0        @ x^2
    vmul.f32 s12, s1 ,s0        @ x^3
    vmul.f32 s13, s1, s12       @ x^5
    vmul.f32 s14, s1, s13       @ x^7
    vmul.f32 s15, s1, s14       @ x^9
    vmul.f32 s16, s1, s15       @ x^11
    vmul.f32 s17, s1, s16       @ x^13  
    vmul.f32 s18, s1, s17       @ x^15
    vmul.f32 s19, s1, s18       @ x^17
    
    @ Multiplikation mit den Koeffizienten
    vmul.f32 q1, q3, q6 
    vmul.f32 q2, q4, q7
    
    @ Summierung der Terme
    vadd.f32 q3, q1, q2
    vadd.f32 d1, d6, d7
    vadd.f32 s1, s2, s3
    vadd.f32 s0, s1, s0
    vpop {d8-d15}   
    pop {pc}
```

Nach der Bereichsreduktion lädt die Funktion `sine` die Taylor-Koeffizienten aus der Tabelle `taylorcoeff_table` in die Register `q6` und `q7`.

Anschließend berechnet die Funktion die Potenzen von `x`, beginnend mit `x^2`. Die Berechnungen setzen sich fort, bis die Potenzen bis `x^17` ermittelt sind.

Im nächsten Schritt werden die berechneten Potenzen von `x` mit den entsprechenden Koeffizienten in den Registern `q6` und `q7` multipliziert. Die Potenzen bis `x^9` werden mit den Koeffizienten aus `q6` verrechnet, während die höheren Potenzen ab `x^11` die Koeffizienten aus `q7` nutzen.

Nach der Multiplikation aller Potenzen mit ihren jeweiligen Koeffizienten folgt die schrittweise Summierung der Produkte. Das finale Ergebnis der Taylor-Approximation von `sin(x)` wird nach der Summierung in `s0` abgelegt.

Zum Abschluss stellt die Funktion die zuvor gesicherten Gleitkommaregister wieder her und kehrt zur aufrufenden Funktion zurück.

#### Funktion cos

```assembly
cos:                            @ s0 input & output
    push {lr}
    ldr r0, =halfpi
    vldr s1, [r0]  
    vadd.f32 s0, s0, s1
    bl sine
    pop {pc}
```
Die Funktion `cos` berechnet den Kosinus eines Werts mithilfe der Identität `cos(x)` = `sin(x + π / 2)`. Dafür wird der Wert `π/2` in das Register `s1` geladen und zu `s0` addiert. Danach ruft die Funktion `sine` auf, um den Sinus des angepassten Werts zu berechnen. 


#### Funktion tan

```assembly
tan:
    push {lr}
    push {r11}
    mov r11, sp	
    sub sp, sp, #8
    vstr.32 s0, [r11, #-4]     
    bl sine
    vstr.32 s0, [r11, #-8]
    vldr.32 s0, [r11, #-4]  
    bl cos
    vcmp.f32 s0, #0
    vmrs APSR_nzcv, FPSCR
    movne r0, #0
    moveq r0, #1                 @ Division durch 0 verhindern
    beq tanend
    vldr.32 s1, [r11, #-8]
    vdiv.f32 s0, s1, s0
tanend:
    add sp, sp, #8
    mov	sp, r11
    pop	{r11}
    pop {lr}
    pop {pc}
```

`tan` berechnet den Tangens eines Werts mithilfe der Identität `tan(x)` = `sin(x)/cos(x)`. Zunächst werden die Register `lr` und `r11` gesichert, bevor der Eingabewert `s0` im Stack abgelegt wird. Anschließend wird die Funktion `sine` aufgerufen, um den Sinus von `x` zu berechnen, dessen Ergebnis ebenfalls im Stack gespeichert wird. Danach wird der ursprüngliche Wert von `x` wiederhergestellt und die Funktion `cos` aufgerufen, um den Kosinus zu berechnen. Um eine Division durch Null zu verhindern, wird überprüft, ob `cos(x)` gleich Null ist. Falls dies nicht der Fall ist, wird der Tangens durch Division von `sin(x)` durch `cos(x)` berechnet. Schließlich wird der Stack aufgeräumt und zur aufrufenden Funktion zurückgekehrt.

|-------------------------|-------------------------------|-----------------------------|
| [zurück](trigon_ue.md)  | [Hauptmenü](../ueberblick.md) | [weiter](matrix_ue.md)      |


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