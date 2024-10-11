## Aufgabenstellung: Textdarstellung via Textmode

Entwickeln Sie eine Textmodus-Verwaltungsbibliothek zur Darstellung und Verwaltung von Text im Textmodus. Diese Bibliothek soll grundlegende Funktionen bereitstellen, um Zeichen darzustellen, den aktuellen Textindex zu verwalten, Zeilenumbrüche zu handhaben sowie Zeichen zu löschen. 

Legen Sie zunächst das File `text.s` an, für die Textdarstellung benötigen sie zudem die Bitmuster der Asciizeichen, die Sie [hier](chars.md) finden.

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