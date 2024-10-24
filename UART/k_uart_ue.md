# B.1 Einführung
## 1.8.2 UART: Übungsaufgabe

### Aufgabenstellung: 

Implementierung von UART-Funktionen in ARM-Assembler für den Raspberry Pi 2B

#### Ziel:
Entwickeln Sie drei grundlegende Funktionen zur Steuerung der UART0-Schnittstelle:

- **k_uart0_init** – Initialisierung der UART0-Schnittstelle
- **k_uart_write_char** – Senden eines einzelnen Zeichens über UART0
- **k_uart_read_char** – Empfangen eines einzelnen Zeichens von UART0.

#### Anforderungen:

##### k_uart0_init: Initialisierung
Implementieren Sie die Initialisierung der UART0-Schnittstelle, einschließlich der Konfiguration der Baudrate und der Einstellungen für die Datenübertragung.
Stellen Sie sicher, dass die UART-Schnittstelle korrekt aktiviert und bereit für die Kommunikation ist.
##### k_uart_write_char: Senden eines Zeichens
Entwickeln Sie eine Funktion, die ein einzelnes Zeichen über die UART0-Schnittstelle sendet.
Die Funktion soll sicherstellen, dass der Sendepuffer bereit ist, bevor das Zeichen übertragen wird.
##### k_uart_read_char: Empfangen eines Zeichens
Erstellen Sie eine Funktion, die ein einzelnes Zeichen von der UART0-Schnittstelle empfängt.
Die Funktion soll überprüfen, ob Daten im Empfangspuffer verfügbar sind, bevor das Zeichen gelesen wird.

##### Hinweise

Die relevanten Adressen zur Initialisierung der UART Schnittstelle lauten:
- **Basisadresse der UART0-Register:**          0x3F201000
- **UART0 Datenregister:**                      0x3F201000
- **UART0 Flag-Register:**                      0x3F201018
- **UART0 Integer Baud Rate Divisor:**          0x3F201024
- **UART0 Fractional Baud Rate Divisor:**       0x3F201028
- **UART0 Line Control Register:**              0x3F20102C
- **UART0 Control Register:**                   0x3F201030
- **UART0 Interrupt Mask Set Clear Register:**  0x3F201038

Zudem empfiehlt sich ein Blick in das Dokument `BCM2835 Peripherals`, das sich unter `Ressourcen` finden lässt.

|----------------------|------------------------------------|-------------------------------|
|   [zurück](uart.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](k_uart_help.md)    |


|**1.8 UART**                             |
|-----------------------------------------|
| [1.8.1 Intro](uart.md)                  |
| [1.8.2 Übung ](k_uart_ue.md)            |