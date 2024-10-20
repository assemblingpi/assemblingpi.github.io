# B.1 Einführung
## 1.8.1 UART: Intro

UART steht für Universal Asynchronous Receiver/Transmitter. Es handelt sich um eine serielle Kommunikationsschnittstelle, die häufig in der Mikrocontroller- und Computertechnik verwendet wird, um Daten zwischen zwei Geräten auszutauschen. 
Sie sendet und empfängt Daten ohne ein gemeinsames Taktsignal, wobei Start- und Stoppbits verwendet werden, um den Anfang und das Ende von Datenpaketen zu markieren.

### Relevante Konzepte für UART-Programmierung

**Serielle Kommunikation:** UART sendet Daten seriell, d.h., Bit für Bit über eine Leitung. Es gibt zwei Hauptleitungen: TX (Transmit) und RX (Receive).

**Asynchrone Übertragung:** UART verwendet keinen gemeinsamen Takt, sondern synchronisiert die Datenübertragung mit Start- und Stoppbits.

**Baudrate:** Als Baudrate bezeichnet man die Anzahl der Signalzustände pro Sekunde, die in einem Kommunikationskanal übertragen werden, und somit bestimmt diese die Übertragungsgeschwindigkeit. Die Baudrate muss auf Sender- und Empfängerseite gleich sein.

**Registersteuerung:** UART ist über eine Reihe von Registern konfigurierbar, z.B. für das Einstellen der Baudrate, das Senden und Empfangen von Daten und das Aktivieren von FIFOs (First-In-First-Out-Puffer).

**Interrupts und Polling:** UART kann so konfiguriert werden, dass es auf Interrupts reagiert oder in einem Polling-Modus arbeitet, in dem das Programm regelmäßig den Status überprüft.

Im Rahmen des Tutorials wollen wir UART benutzen, um Benutzereingaben erhalten zu können und (zum Beginn) auch um Ausgaben unserer Programme am Bildschirm darstellen zu können. Dafür müssen wir zum einem UART initialisieren und zudem auch Funktionen für die Ein- und Ausgabe entwickeln.

### UART initialisieren
**UART deaktivieren:** Setze das Control-Register auf 0, um sicherzustellen, dass UART ausgeschaltet ist.

**Baudrate konfigurieren:** Schreibe die Werte für die Integer- und Fractional-Baudrate in die entsprechenden Register.
FIFO und Datenformat einstellen: Konfiguriere die FIFOs und setze das Datenformat (z.B. 8 Datenbits).
Interrupts deaktivieren: Wenn erwünscht, deaktiviere Interrupts für einen Polling-Modus.
UART aktivieren: Setze das Control-Register, um UART zu aktivieren.


**Zeichen senden (k_uart_write_char):** Überprüfe das Flag-Register: Warte, bis der TX-Puffer bereit ist.
Zeichen senden: Schreibe das zu sendende Zeichen in das Datenregister.


**Zeichen empfangen (k_uart_read_char):**
Überprüfe das Flag-Register: Warte, bis Daten im RX-Puffer vorhanden sind.
Zeichen lesen: Lese das empfangene Zeichen aus dem Datenregister.

|------------------------------|------------------------------------|-----------------------------|
|   [zurück](../Boot/boot.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](k_uart_ue.md)    |


|**1.8 UART**                             |
|-----------------------------------------|
| [1.8.1 Intro](uart.md)                  |
| [1.8.2 Übung ](k_uart_ue.md)            |