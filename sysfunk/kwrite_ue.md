# B.3 Implementierung systemnaher Funktionen
## 3.1.7 Systemnahe Funktionen: Aufgabe Implementierung einer `kwrite`-Funktion

**Ziel:**  
Entwickeln Sie die Funktion `kwrite` in ARM-Assembler, die Daten an verschiedene Ausgabemedien wie Display, Datei oder UART für Fehlerausgaben schreibt. Diese Funktion soll von `kprintf` genutzt werden, um formatierte Ausgaben zu ermöglichen. Es ist an dieser Stelle weder nötig noch möglich die Dateiausgabe zu implementieren, da wir im Rahmen des Tutorials kein Dateisystem implementieren werden.

**Anforderungen:**

1. **Eingabeparameter:**
   - **`r0`**: Ausgabe-Stream ([0] für Display, [1] für Datei, [2/3] für Fehlerausgabe über UART).
   - **`r1`**: Adresse des auszugebenden Strings.
   - **`r2`**: Anzahl der auszugebenden Bytes.

2. **Funktionalität:**
   - Verarbeiten und Weiterleiten der Daten an das ausgewählte Ausgabemedium.
   - Erkennen und korrekte Handhabung von Zeilenumbrüchen im Text.
   - Sicherstellen eines stabilen und effizienten Ablaufs ohne Pufferüberläufe.

3. **Weitere Anforderungen:**
   - Verwenden Sie die definierten Konstanten für den Zugriff auf die entsprechenden Register.
   - Implementieren Sie ein effizientes Stack-Management zur Verwaltung von lokalen Variablen und Registerzuständen.
   - Stellen Sie sicher, dass die Funktion robust gegenüber ungültigen Eingaben ist und entsprechende Fehler behandelt.

**Hinweise:**
- Testen Sie die Funktion mit verschiedenen Ausgabeströmen und Daten, um die korrekte Funktionalität sicherzustellen.


### Hinweise zur Implementierung von kwrite

1. **Eingabeparameter:**
   - `r0`: Ausgabe-Stream ([0] für Display, [1] für Datei, [2/3] für Fehlerausgabe über UART).
   - `r1`: Adresse des auszugebenden Strings.
   - `r2`: Anzahl der auszugebenden Bytes.

2. **Funktionalität:**
   - **Sichern der Register:** Sichern von `lr` und `r11` auf dem Stack.
   - **Sprungtabelle:** Verwenden Sie eine Sprungtabelle (`stdout_tbl`), um basierend auf dem Wert in `r0` den entsprechenden Ausgabehandler (`Out_0_handler`, `Out_1_handler`, `Out_2_handler`) aufzurufen.
   - **Handler für Ausgabemedien:**
     - **`Out_0_handler` (Display):**  
       - Durchlaufen des Strings und Ausgeben jedes Zeichens über `text_printchar`.
       - Erkennen und Verarbeiten von Zeilenumbrüchen (`/n`) durch Aufruf von `out_0_print_newline`.
     - **`Out_2_handler` (UART für Fehlerausgabe):**  
       - Durchlaufen des Strings und Senden jedes Zeichens über `k_uart_write_char`.
       - Erkennen und Verarbeiten von Zeilenumbrüchen (`/n`) durch Aufruf von `out_2_print_newline`.
     - **`Out_1_handler` (Datei):**  
       - (Optional) Implementierung der Dateiausgabe.
   - **Zeilenumbruch-Funktionen:**
     - **`out_0_print_newline`:** Ausgabe eines einfachen Line Feed (`0x0A`) auf dem Display.
     - **`out_2_print_newline`:** Ausgabe von Carriage Return und Line Feed (`0x0D, 0x0A`) über UART.
   - **Abschluss der Funktion:**  
     - Rücksetzen des Stack-Pointers.
     - Wiederherstellen der Register `r11` und `lr`.
     - Rückkehr zum Aufrufer mittels `bx lr`.

3. **Fehlerbehandlung:**
   - Sicherstellen, dass der Ausgabepuffer nicht überläuft.
   - Bei ungültigen Spezifikatoren oder Pufferüberläufen entsprechende Fehlermeldungen ausgeben.

|--------------------------|------------------------------------|------------------------------|
|   [zurück](text_lsg.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](kwrite_lsg.md)    |


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