
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


