# B.2 Erweiterungen der CPU-Funktionalität
## 2.3.17 VFP und NEON: Übungsaufgabe zur Implementierung von Trigonometrischen Funktionen

### Hintergrund

**Trigonometrische Funktionen** wie Sinus, Kosinus und Tangens sind fundamentale Bestandteile vieler eingebetteter Systeme, beispielsweise in der Robotik, Signalverarbeitung oder bei der Steuerung von Bewegungsabläufen. Eine effiziente Implementierung dieser Funktionen in Assembler unter zuhilfenahme der Floating-Point und NEON-Erweiterungen ermöglicht präzise und schnelle Berechnungen.

### Aufgabenbeschreibung

Entwickeln Sie eine ARM-Assembly-Bibliothek, die die trigonometrischen Funktionen `sin`, `cos` und `tan` implementiert. Diese Funktionen sollen mithilfe der Taylor-Reihe approximiert werden. Die Implementierung soll folgende Anforderungen erfüllen:

#### **Implementierung der Funktionen `sin`, `cos` und `tan`**
   - **sin(x)**:
     - Reduziert den Eingabewert `x` auf den Bereich **[-π, π]**
     - Verwendet die Taylor-Reihe zur Approximation von `sin(x)` bis zur Potenz **x^17**:
       
**sin(x)** = `x` **-** `(x^3)/(3!)` **+** `(x^5)/(5!)`  **-** `(x^7)/(7!)`  **+** `(x^9)/(9!)` **-** `(x^11)/(11!)`  **+** `(x^13)/(13!)` **-** `(x^15)/(15!)`  **+** `(x^17)/(17!)` 
       
   - **cos(x)**:
     - Nutzt die Identität **cos(x)** = `sin(x + π/2)` zur Berechnung.
   
   - **tan(x)**:
     - Berechnet **tan(x)** als Quotient von `sin(x)` und `cos(x)`.
     - Implementiert eine Überprüfung, um eine Division durch Null zu verhindern.

### Wichtiger Hinweis beim Assemblieren

Beim Assemblieren von Code, der VFP- oder NEON-Befehle verwendet, müssen spezielle Optionen gesetzt werden:

```bash
arm-none-eabi-as -march=armv7-a -mcpu=cortex-a7 -mfloat-abi=hard -mfpu=neon-vfpv4 -c trigono.s -g -o trigono.o
```

**Bedeutung der zusätzlichen Optionen:**
- `-mfloat-abi=hard`: Nutzung der Hardware-Floating-Point-ABI.
- `-mfpu=neon-vfpv4`: Aktiviert NEON und VFPv4 Erweiterungen.



|--------------------|-------------------------------|------------------------------|
| [zurück](vtrn.md)  | [Hauptmenü](../ueberblick.md) | [weiter](trigon_lsg.md)      |


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