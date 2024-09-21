### Aufgabenstellung: Implementierung einer `kwrite`-Funktion

**Ziel:**  
Entwickeln Sie die Funktion `kwrite` in ARM-Assembler, die Daten an verschiedene Ausgabemedien wie Display, Datei oder UART für Fehlerausgaben schreibt. Diese Funktion soll von `kprintf` genutzt werden, um formatierte Ausgaben zu ermöglichen. Es ist zunächst nicht nötig alle Ausgabemedien ansteuern zu können, demnach können vorerst alle Optionen (außer UART) in Dauerschleifen/Fehlermeldungen resultieren!

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


