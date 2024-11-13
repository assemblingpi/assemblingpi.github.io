# B.3 Implementierung systemnaher Funktionen
## 3.1.8 Systemnahe Funktionen: Aufgabe Implementierung einer Eingabefunktion 

### Beschreibung

Implementieren Sie eine Funktion `kread` im File `kread.s`, die Daten von verschiedenen Eingabequellen einliest und in einem bereitgestellten Puffer speichert. Die Funktion soll sowohl Eingaben von der UART-Schnittstelle als auch von Dateien verarbeiten können, letzteres ist optional, da wir im Rahmen dieses Tutorials kein Dateisystem implementieren werden. Dabei sollen spezielle Eingaben wie Carriage Return (Enter-Taste), Backspace und die Anpassung der Tastatureingaben zwischen amerikanischer und deutscher Belegung berücksichtigt werden. Am Ende soll die Funktion die Länge des eingelesenen Strings zurückgeben oder einen Fehlercode, falls ein Fehler aufgetreten ist.

### Spezifikationen

- **Funktionsprototyp:**

  ```assembly
  kread:
    @ Eingabe:
    @   r0 - Eingabestream (0 für Datei, 1 für UART)
    @   r1 - Zieladresse für den eingelesenen String
    @   r2 - Maximale Anzahl der zu lesenden Bytes
    @ Ausgabe:
    @   r0 - Länge des eingelesenen Strings oder Fehlercode (0xFFFFFFFF bei Fehler)
  ```

- **Anforderungen:**

  - 1. **Eingabestream-Auswahl:**
     - Die Funktion soll anhand des Wertes in `r0` entscheiden, ob von einer Datei (0) oder von der UART-Schnittstelle (1) gelesen wird.
     - Bei nicht implementiertem Dateihandling soll der entsprechende Handler eine Endlosschleife enthalten oder einen geeigneten Fehlercode zurückgeben.

  - 2. **Eingabe von der UART-Schnittstelle:**
     - Verwenden Sie die Funktion `k_uart_read_char`, um ein Zeichen von der UART-Schnittstelle zu lesen.
     - Geben Sie jedes gelesene Zeichen mit `k_uart_write_char` wieder aus (Echo-Funktion).

  - 3. **Sonderzeichenbehandlung:**
     - **Carriage Return (Enter-Taste, ASCII 0x0D):**
       - Beenden Sie die Eingabe, wenn ein Carriage Return erkannt wird.
       - Geben Sie einen Zeilenvorschub aus, indem Sie `kwrite` mit der Zeichenkette `"/n"` aufrufen (siehe Hinweis).
     - **Backspace (ASCII 0x08):**
       - Löschen Sie das zuletzt eingegebene Zeichen aus dem Puffer und aktualisieren Sie die Anzeige entsprechend.
       - Achten Sie darauf, dass kein Unterlauf auftritt, wenn kein Zeichen zum Löschen vorhanden ist.
     - **Nicht druckbare Zeichen:**
       - Ignorieren Sie alle Zeichen mit ASCII-Werten unter 0x20 (außer Backspace und Enter).

  - 4. **Tastaturanpassung:**
     - Passen Sie die Eingaben für die Tasten 'Y' und 'Z' (sowohl Groß- als auch Kleinbuchstaben) an, um Unterschiede zwischen amerikanischer und deutscher Tastaturbelegung auszugleichen:
       - Ersetzen Sie 'Y' durch 'Z' und umgekehrt.
       - Ersetzen Sie 'y' durch 'z' und umgekehrt.

  - 5. **Speichern der Eingabe:**
     - Speichern Sie gültige Zeichen in dem Puffer, dessen Adresse in `r1` übergeben wird.
     - Stellen Sie sicher, dass der Puffer nicht überläuft. Überschreiten Sie nicht die maximale Anzahl von Bytes, die in `r2` angegeben ist.
     - Nullterminieren Sie den String nach Abschluss der Eingabe.

  - 6. **Textmodus-Unterstützung (optional):**
     - Wenn das System im Textmodus arbeitet (Variable `textmode_state` ungleich 0), verwenden Sie die Funktionen:
       - `text_printchar` zum Ausgeben von Zeichen im Textmodus.
       - `text_newline` zum Einfügen eines Zeilenvorschubs.
       - `text_del` zum Löschen von Zeichen.
       - `text_current_index`, um den aktuellen Cursor-Index zu erhalten.

  - 7. **Fehlerbehandlung:**
     - Geben Sie den Fehlercode `0xFFFFFFFF` zurück, wenn ein Fehler auftritt.


- **Hinweise:**
Da es bei der Ausgabe von QEMU über UART erfahrungsgemäß zu Problemen mit Zeilenumbrüchen kommt, wenn `newline` verwendet wird, verwenden wir statt des üblichen `\n` die Zeichenfolge `/n`.

|----------------------------|------------------------------------|-----------------------------|
|   [zurück](kwrite_lsg.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](kread_lsg.md)    |


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