## Aufgabenstellung: Implementierung einer 4x4-Matrixmultiplikationsfunktion mit NEON

### Hintergrund:
Die Multiplikation von Matrizen,spielt in vielen Bereichen wie Grafik, Signalverarbeitung und wissenschaftlichen Berechnungen eine Rolle.

### Ziel:
Entwickeln Sie eine Funktion namens `Matrix4x4_mul`, die zwei 4x4-Matrizen multipliziert und das Ergebnis in einer dritten Matrix speichert. Nutzen Sie dabei die NEON-Erweiterungen zur Optimierung der Berechnungen. Zusätzlich sollen Sie eine Hilfsfunktion zur Transponierung von Matrizen implementieren. Legen sie die Funktion in einem Sourcefile namens `matrix_mul4.s` an und beachten sie die notwendigen Änderungen in `build.sh` vorzunehmen.

   - **Parameter:**
     - `r0`: Zeiger auf die Ergebnis-Matrix `Matrix_c`.
     - `r1`: Zeiger auf die erste Eingabematrix `Matrix_a`.
     - `r2`: Zeiger auf die zweite Eingabematrix `Matrix_b`.
   
**Hinweis:** Nutzen Sie die NEON-Erweiterungen, um die Matrizenmultiplikation zu implementieren.


|--------------------------|-------------------------------|------------------------------|
| [zurück](trigon_lsg.md)  | [Hauptmenü](../ueberblick.md) | [weiter](matrix_lsg.md)      |