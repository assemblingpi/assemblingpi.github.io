## Aufgabenstellung: Implementierung von UART-Funktionen in ARM-Assembler für den Raspberry Pi 2B
### Ziel:
Entwickeln Sie drei grundlegende Funktionen zur Steuerung der UART0-Schnittstelle:

**k_uart0_init** – Initialisierung der UART0-Schnittstelle.
**k_uart_write_char** – Senden eines einzelnen Zeichens über UART0.
**k_uart_read_cha**r – Empfangen eines einzelnen Zeichens von UART0.

### Anforderungen:

#### k_uart0_init: Initialisierung
Implementieren Sie die Initialisierung der UART0-Schnittstelle, einschließlich der Konfiguration der Baudrate und der Einstellungen für die Datenübertragung.
Stellen Sie sicher, dass die UART-Schnittstelle korrekt aktiviert und bereit für die Kommunikation ist.
#### k_uart_write_char: Senden eines Zeichens
Entwickeln Sie eine Funktion, die ein einzelnes Zeichen über die UART0-Schnittstelle sendet.
Die Funktion soll sicherstellen, dass der Sendepuffer bereit ist, bevor das Zeichen übertragen wird.
#### k_uart_read_char: Empfangen eines Zeichens
Erstellen Sie eine Funktion, die ein einzelnes Zeichen von der UART0-Schnittstelle empfängt.
Die Funktion soll überprüfen, ob Daten im Empfangspuffer verfügbar sind, bevor das Zeichen gelesen wird.
