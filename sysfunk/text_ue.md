# B.3 Implementierung systemnaher Funktionen
## 3.1.6 Systemnahe Funktionen: Aufgabe Textdarstellung via Textmode

Entwickeln Sie eine Textmodus-Verwaltungsbibliothek zur Darstellung und Verwaltung von Text im Textmodus. Diese Bibliothek soll grundlegende Funktionen bereitstellen, um Zeichen darzustellen, den aktuellen Textindex zu verwalten, Zeilenumbrüche zu handhaben sowie Zeichen zu löschen. 

Legen Sie zunächst das File `text.s` an, für die Textdarstellung benötigen sie zudem die Bitmuster der Asciizeichen, die Sie [hier](chars.md) finden.

Untersucht man im Anschluss das Programm mit dem Disassembler, kann man dort auch die Zeichen im Speicher erkennen:

![chars.gif](char.gif)


### Zu implementierende Funktionen und Schnittstellen

1. **`text_printchar`**
   - **Parameter:**
     - Register `r1`: ASCII-Wert des darzustellenden Zeichens.
   - **Zweck:** Zeichnet ein einzelnes Zeichen an der aktuellen Position im Textmodus. Bei einem Zeilenumbruch (`\n`) wird die entsprechende Funktion aufgerufen.

2. **`text_inc_table_index`**
   - **Parameter:** Keine.
   - **Zweck:** Erhöht den aktuellen Tabellenindex, um die Position für das nächste zu druckende Zeichen festzulegen.

3. **`text_newline`**
   - **Parameter:** Keine.
   - **Zweck:** Bewegt den Tabellenindex an den Anfang der nächsten Zeile, um einen Zeilenumbruch zu realisieren.

4. **`text_del`**
   - **Parameter:** Keine.
   - **Zweck:** Verringert den aktuellen Tabellenindex, um das zuletzt eingefügte Zeichen zu entfernen.

5. **`text_current_index`**
   - **Parameter:** Keine.
   - **Zweck:** Gibt den aktuellen Tabellenindex zurück, um die aktuelle Position im Textmodus zu ermitteln.

### Anforderungen
Nutzung der globalen Variablen `table_index` zur Verfolgung der aktuellen Position im Textmodus.
Verwenden Sie zudem die externen Symbole, bzw. Funktionen `char_base` und `textmode_get_tabentry` zur Zeichenverwaltung und Speicheradressierung.


|-------------------------------|------------------------------------|----------------------------|
|   [zurück](textmode_lsg.md)   |   [Hauptmenü](../ueberblick.md)    |   [weiter](text_lsg.md)    |


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
  