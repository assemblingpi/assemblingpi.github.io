## Aufgabenstellung: Implementierung von Trigonometrischen Funktionen

### Hintergrund

**Trigonometrische Funktionen** wie Sinus, Kosinus und Tangens sind fundamentale Bestandteile vieler eingebetteter Systeme, beispielsweise in der Robotik, Signalverarbeitung oder bei der Steuerung von Bewegungsabläufen. Eine effiziente Implementierung dieser Funktionen in Assembler unter zuhilfenahme der Floating-Point und NEON-Erweiterungen ermöglicht präzise und schnelle Berechnungen.

### Aufgabenbeschreibung

Entwickeln Sie eine ARM-Assembly-Bibliothek, die die trigonometrischen Funktionen `sin`, `cos` und `tan` implementiert. Diese Funktionen sollen mithilfe der Taylor-Reihe approximiert werden. Die Implementierung soll folgende Anforderungen erfüllen:

#### 1. **Globale Funktion `trigon`**
   - **Prototyp:** `.global trigon`
   - **Parameter:**
     - **`r0`**: Integer-Wert, der in einen Gleitkommawert umgewandelt wird und als Eingabe für die trigonometrische Berechnung dient.
     - **`r1`**: Auswahlparameter, der bestimmt, welche trigonometrische Funktion berechnet werden soll:
       - `0` für `sin`
       - `1` für `cos`
       - `2` für `tan`
   - **Funktionsweise:**
     - Wandelt den Integer-Wert aus `r0` in einen Gleitkommawert um.
     - Wählt basierend auf `r1` die entsprechende Funktion (`sin`, `cos`, `tan`) aus einer Sprungtabelle aus und führt diese aus.
     - Implementiert eine Fehlerbehandlung für ungültige Auswahlwerte.

#### 2. **Implementierung der Funktionen `sin`, `cos` und `tan`**
   - **sin(x)**:
     - Reduziert den Eingabewert `x` auf den Bereich \([-π, π]\) mittels Modulo-Operation.
     - Verwendet die Taylor-Reihe zur Approximation von `sin(x)` bis zur Potenz \(x^{17}\):
       \[
       sin(x) = x - (x^3)/(3!) + (x^5)/(5!)  - (x^7)/(7!)  + (x^9)/(9!) - (x^11)/(11!)  + (x^13)/(13!) - (x^15)/(15!)  + (x^17)/(17!) 
       \]
     - Nutzt eine Tabelle mit den entsprechenden Taylor-Koeffizienten zur effizienten Berechnung.
   
   - **cos(x)**:
     - Nutzt die Identität \( \cos(x) = \sin(x + \frac{\pi}{2}) \) zur Berechnung.
   
   - **tan(x)**:
     - Berechnet \( \tan(x) \) als Quotient von `sin(x)` und `cos(x)`.
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