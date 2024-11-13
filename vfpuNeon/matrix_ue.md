# B.2 Erweiterungen der CPU-Funktionalität
## 2.3.18 VFP und NEON: Übungsaufgabe zur Implementierung einer 4x4-Matrixmultiplikationsfunktion mit NEON

### Hintergrund:
Die Multiplikation von Matrizen, spielt in vielen Bereichen wie Grafik, Signalverarbeitung und wissenschaftlichen Berechnungen eine Rolle.

### Ziel:
Entwickeln Sie eine Funktion namens `Matrix4x4_mul`, die zwei 4x4-Matrizen multipliziert und das Ergebnis in einer dritten Matrix speichert. Nutzen Sie dabei die NEON-Erweiterungen zur Optimierung der Berechnungen. Zusätzlich sollen Sie eine Hilfsfunktion zur Transponierung von Matrizen implementieren. Legen sie die Funktion in einem Sourcefile namens `matrix_mul4.s` an und beachten sie die notwendigen Änderungen in `build.sh` vorzunehmen.

   - **Parameter:**
     - `r0`: Zeiger auf die Ergebnis-Matrix `Matrix_c`.
     - `r1`: Zeiger auf die erste Eingabematrix `Matrix_a`.
     - `r2`: Zeiger auf die zweite Eingabematrix `Matrix_b`.
   
**Hinweis:** Nutzen Sie die NEON-Erweiterungen, um die Matrizenmultiplikation zu implementieren.

|--------------------------|-------------------------------|------------------------------|
| [zurück](trigon_lsg.md)  | [Hauptmenü](../ueberblick.md) | [weiter](matrix_lsg.md)      |


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