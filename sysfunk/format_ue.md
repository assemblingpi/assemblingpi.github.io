# B.3 Implementierung systemnaher Funktionen
## 3.1.3 Systemnahe Funktionen: Übungsaufgaben zur Implementierung von Zahlendarstellungsfunktionen 

### **Hintergrund:**
In dieser Aufgabe sollen Sie drei grundlegende Funktionen zur Zahlendarstellung in der Sourcedatei `kformat.s` implementieren: `float2ascii`, `num_2_dec` und `num2hexascii`. 


### Teilaufgabe 1: Implementierung der Funktion float2ascii

#### **Ziel:**
Implementieren Sie die Funktion `float2ascii`, die eine gegebene Gleitkommazahl in ihre ASCII-Darstellung konvertiert und in einem internen Puffer speichert. Dabei sollen mindestens 4 Nachkommastellen angezeigt werden.

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
  - `r2`: Vorzeichen: 0 = Wert ist negativ  /  1 = Wert ist Positiv
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

**Achtung: Um die Umwandlung von Float zur Asciidarstellung zu bewerkstelligen, müssen Floatingpoint-Register & Instruktionen verwendet werden, demnach müssen auch wieder entsprechende Einstellungen beim assemblieren vorgenommen werden!**

|------------------------|------------------------------------|-----------------------------|
|   [zurück](memlsg.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](formatlsg.md)    |


|**3.1 Systemnahe Funktionen**                                                                  |
|-----------------------------------------------------------------------------------------------|
| [3.1.1 Implementierung systemnaher Funktionen](sysfunkintro.md)                               |
| [3.1.2 Implementierung von Speicherfunktionen in ARM-Assembly](memue.md)                      |
| [3.1.3 Implementierung von Zahlendarstellungsfunktionen](format_ue.md)                        |
| [3.1.4 Grundlegende Grafikbibliothek](canvas_ue.md)                                           |
| [3.1.5 Implementierung von Funktionen zur Verwaltung des Textmodus](textmode_ue.md)           |
| [3.1.6 Textdarstellung via Textmode](text_ue.md)                                              |
| [3.1.7 Implementierung einer `kwrite`-Funktion](kwrite_ue.md)                                 |
| [3.1.8 Implementierung einer Eingabefunktion](kread_ue.md)                                    |
| [3.1.9 Implementierung einer formatierenden Ausgabefunktion in ARM-Assembly](kprintf_ue.md)   |
| [3.1.10 Implementiere `kscan` für formatiertes Einlesen](kscan_ue.md)                         |