# B.3 Implementierung systemnaher Funktionen
## 3.1.2 Systemnahe Funktionen: Übungsaufgaben zur Implementierung von Speicherfunktionen in ARM-Assembly

Diese Übungsaufgaben zielen darauf ab, grundlegende Speicherfunktionen (`memset`, `memcpy`, `memcmp`) in einem Sourcefile namens `kmem.s`zu implementieren. Entsprechende Änderungen sind in `build.sh` vorzunehmen. 

### **Aufgabe 1: Implementierung der Funktion `memset`**

**Ziel:**
Schreiben Sie eine Funktion namens `memset`, die einen Speicherbereich mit einem bestimmten Wert initialisiert.

**Beschreibung:**
Die Funktion `memset` setzt eine gegebene Anzahl von Bytes (`size`) ab einer bestimmten Speicheradresse (`ptr`) auf einen festgelegten Wert (`initval`). 


- **Parameter:**
  - `r0`: Zeiger auf den Zielbereich (`ptr`).
  - `r1`: Wert, mit dem der Speicher gefüllt werden soll (`initval`).
  - `r2`: Anzahl der zu setzenden Bytes (`size`).


### **Aufgabe 2: Implementierung der Funktion `memcpy`**

**Ziel:**
Schreiben Sie eine Funktion namens `memcpy`, die Daten von einer Quelladresse zu einer Zieladresse kopiert.

### Beschreibung:
Die Funktion `memcpy` kopiert eine bestimmte Anzahl von Bytes von einer Quelladresse zu einer Zieladresse. Die Kopie erfolgt in aufsteigender Reihenfolge, es sei denn, die Zieladresse liegt nach der Quelladresse und die Bereiche überschneiden sich. In diesem Fall wird in absteigender Reihenfolge kopiert, um zu verhindern, dass bereits kopierte Daten die noch zu kopierenden Bytes überschreiben, was zu Datenkorruption führen würde.


- **Parameter:**
  - `r0`: Zieladresse (`dst`).
  - `r1`: Quelladresse (`src`).
  - `r2`: Anzahl der zu kopierenden Bytes (`size`).


### **Aufgabe 3: Implementierung der Funktion `memcmp`**

**Ziel:**
Schreiben Sie eine Funktion namens `memcmp`, die zwei Speicherbereiche miteinander vergleicht.

**Beschreibung:**
Die Funktion `memcmp` vergleicht zwei Speicherbereiche (`src1` und `src2`) der Größe (`size`) byteweise. Sie gibt an, ob die Speicherbereiche identisch sind oder sich unterscheiden. Bei einem Unterschied wird `0` zurückgegeben, ansonsten wird `1` zurückgegeben.

**Anforderungen:**
- **Globale Deklaration:** Die Funktion muss global sichtbar sein.
- **Parameter:**
  - `r0`: Erste Quelladresse (`src1`).
  - `r1`: Zweite Quelladresse (`src2`).
  - `r2`: Anzahl der zu vergleichenden Bytes (`size`).

|------------------------------|------------------------------------|--------------------------|
|   [zurück](sysfunkintro.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](memlsg.md)    |


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