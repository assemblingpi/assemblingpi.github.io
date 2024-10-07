## Aufgabenstellung: Implementierung von Zahlendarstellungsfunktionen 

### **Hintergrund:**
In eingebetteten Systemen und Low-Level-Programmierungen ist die effiziente Umwandlung von Zahlen in verschiedene Darstellungsformen essenziell. In dieser Aufgabe sollen Sie drei grundlegende Funktionen zur Zahlendarstellung implementieren: `float2ascii`, `num_2_dec` und `num2hexascii`. 


### Teilaufgabe 1: Implementierung der Funktion float2ascii

#### **Ziel:**
Implementieren Sie die Funktion `float2ascii`, die eine gegebene Gleitkommazahl in ihre ASCII-Darstellung konvertiert und in einem internen Puffer speichert.

- **Eingaben:**
  - `r1`: Floatwert, der in einen ASCII-String umgewandelt werden soll.
- **Ausgaben:**
  - `r0`: Adresse des erzeugten ASCII-Strings.
  - `r1`: Länge des erzeugten Strings.


### Teilaufgabe 2: Implementierung der Funktion num_2_dec

#### **Ziel:**
Implementieren Sie die Funktion `num_2_dec`, die eine gegebene Ganzzahl in ihre dezimale ASCII-Darstellung konvertiert und in einem internen Puffer speichert.

- **Eingaben:**
  - `r1`: Ganzzahlwert, der als Dezimalzahl dargestellt werden soll.
  - `r2`: Feldbreite (optional), um den erzeugten String entsprechend zu füllen.
- **Ausgaben:**
  - `r0`: Adresse des erzeugten ASCII-Strings.
  - `r1`: Länge des erzeugten Strings.

### Teilaufgabe 3: Implementierung der Funktion num2hexascii

#### **Ziel:**
Implementieren Sie die Funktion `num2hexascii`, die eine gegebene Ganzzahl in ihre hexadezimale ASCII-Darstellung konvertiert und in einem internen Puffer speichert.

- **Eingaben:**
  - `r1`: Ganzzahlwert, der als Hexadezimalzahl dargestellt werden soll.
  - `r2`: Feldbreite (optional), um den erzeugten String entsprechend zu füllen.
- **Ausgaben:**
  - `r0`: Adresse des erzeugten ASCII-Strings.
  - `r1`: Länge des erzeugten Strings.
- **zusätzliche Anforderungen:**
  - Fügen Sie das Präfix `"0x"` in den ASCII-String ein.

