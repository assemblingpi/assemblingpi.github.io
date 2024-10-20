# B.1 Einführung
## 1.8.2 UART: Übungsaufgabe
### Hinweise zur Implementierung der UART-Funktionen

#### k_uart0_init: Initialisierung

- Setzen Sie das UART0 Control Register (UART0_CR) zurück, um die UART zu deaktivieren.
- Konfigurieren Sie die Baudrate auf 115200 Baud durch Setzen von UART0_IBRD auf 26 und UART0_FBRD auf 0.
- Aktivieren Sie die FIFOs und setzen Sie die Wortlänge auf 8 Bit im Line Control Register (UART0_LCRH).
- Deaktivieren Sie alle UART-Interrupts durch Setzen des Interrupt Mask Set Clear Registers (UART0_IMSC) auf 0.
- Aktivieren Sie die UART0-Schnittstelle durch Schreiben von 0x301 in das Control Register (UART0_CR).
- Sichern und Wiederherstellen der Register lr und r11 am Anfang und Ende der Funktion.

#### k_uart_write_char: Senden eines Zeichens

- Überprüfen Sie das Flag Register (UART0_FR), um sicherzustellen, dass der Transmit FIFO nicht voll ist (Bit 5).
- Schreiben Sie das zu sendende Zeichen aus r0 in das Datenregister (UART0_DR).
- Sichern und Wiederherstellen der Register r1 bis r3 sowie lr.

#### k_uart_read_char: Empfangen eines Zeichens

- Überprüfen Sie das Flag Register (UART0_FR), um sicherzustellen, dass der Receive FIFO nicht leer ist (Bit 4).
- Lesen Sie das empfangene Zeichen aus dem Datenregister (UART0_DR) und speichern Sie es in r0.
- Sichern und Wiederherstellen der Register lr

|---------------------------|------------------------------------|------------------------------|
|   [zurück](k_uart_ue.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](k_uart_lsg.md)    |


|**1.8 UART**                             |
|-----------------------------------------|
| [1.8.1 Intro](uart.md)                  |
| [1.8.2 Übung ](k_uart_ue.md)            |