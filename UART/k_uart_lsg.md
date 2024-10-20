# B.1 Einführung
## 1.8.2 UART: Übungsaufgabe (Lösung)
### Analyse und Erklärung des UART-Codes für den Raspberry Pi 2B in ARM-Assembly

Der gegebene ARM-Assembler-Code implementiert Funktionen zur Steuerung der UART0-Schnittstelle auf dem Raspberry Pi 2B. Diese Funktionen beinhalten das Initialisieren der UART-Schnittstelle, das Senden und Empfangen von Zeichen. Im Folgenden wird der Code abschnittsweise erklärt.

### Konstantendefinitionen

Zu Beginn des Codes werden die Konstanten für die Basisadressen und Register der GPIO- und UART0-Peripheriegeräte definiert.

```assembly
.equ GPIO,          0x3F200000    @ Basisadresse der GPIO-Register
.equ GPPUD,         GPIO + 0x94   @ GPIO Pull-up/down Enable Register
.equ GPPUD_CLK0,    GPIO + 0x98   @ GPIO Pull-up/down Clock Register 0

.equ UART0,         GPIO + 0x1000 @ Basisadresse der UART0-Register
.equ UART0_DR,      UART0 + 0x00  @ Datenregister
.equ UART0_FR,      UART0 + 0x18  @ Flag-Register
.equ UART0_IBRD,    UART0 + 0x24  @ Integer Baud Rate Divisor
.equ UART0_FBRD,    UART0 + 0x28  @ Fractional Baud Rate Divisor
.equ UART0_LCRH,    UART0 + 0x2C  @ Line Control Register
.equ UART0_CR,      UART0 + 0x30  @ Control Register
.equ UART0_IMSC,    UART0 + 0x38  @ Interrupt Mask Set Clear Register
```

Diese Definitionen geben die Adressen der relevanten Register an, die später in den UART-Funktionen verwendet werden. Beispielsweise wird das **UART0_CR** (Control Register) für die Steuerung der UART-Funktionalitäten verwendet.

### Funktion `k_uart0_init`

Die Funktion `k_uart0_init` initialisiert die UART0-Schnittstelle, indem sie die Steuerungsregister zurücksetzt, die Baudrate konfiguriert und die UART aktiviert.

```assembly
.global k_uart0_init
k_uart0_init:
    push    {r0-r1, lr}          @ Sichert r0, r1 und lr

uart_ctrl_reset:
    mov     r0, #0
    ldr     r1, =UART0_CR
    str     r0, [r1]             @ Deaktiviert UART, indem Control Register auf 0 gesetzt wird
```

Zunächst wird die UART deaktiviert, indem das Control Register auf 0 gesetzt wird. Dies sichert einen definierten Ausgangszustand, bevor weitere Einstellungen vorgenommen werden.

```assembly
setbaudrate:
    mov     r0, #26
    ldr     r1, =UART0_IBRD
    str     r0, [r1]             @ Setzt den Integer Baud Rate Divisor auf 26

    mov     r0, #0
    ldr     r1, =UART0_FBRD
    str     r0, [r1]             @ Setzt den Fractional Baud Rate Divisor auf 0
```

Die Baudrate wird als Nächstes durch Setzen des Integer- und Fractional-Teils des Baudraten-Divisors konfiguriert. Der Wert `26` für den Integer-Teil und `0` für den Fractional-Teil ergeben eine Baudrate von 115200 Baud.

```assembly
enable_t_s:
    mov     r0, #7
    lsl     r0, r0, #4
    ldr     r1, =UART0_LCRH
    str     r0, [r1]             @ Aktiviert FIFOs und setzt die Wortlänge auf 8 Bit
```

Die Funktion aktiviert die FIFOs (First In, First Out Speicher) und setzt die Wortlänge auf 8 Bit, indem das Line Control Register (`UART0_LCRH`) konfiguriert wird.

```assembly
disable_int:
    mov     r0, #0
    ldr     r1, =UART0_IMSC
    str     r0, [r1]             @ Deaktiviert alle Interrupts (UART arbeitet im Polling-Modus)
```

Um die UART im Polling-Modus zu betreiben, werden alle Interrupts durch das Schreiben von `0` in das Interrupt Mask Set Clear Register (`UART0_IMSC`) deaktiviert.

```assembly
uart_enable:
    ldr     r0, =0x301
    ldr     r1, =UART0_CR
    str     r0, [r1]             @ Aktiviert die UART mit RXE und TXE
    pop     {r0-r1, lr}          @ Stellt Register wieder her
    bx      lr                   @ Rückkehr zur aufrufenden Funktion
```

Schließlich wird die UART wieder aktiviert, indem das Control Register mit dem Wert `0x301` beschrieben wird. Dieser Wert aktiviert die UART (`UARTEN`), das Senden (`TXE`) und das Empfangen (`RXE`) von Daten.

### Funktion `k_uart_write_char`

Die Funktion `k_uart_write_char` sendet ein einzelnes Zeichen über die UART0-Schnittstelle. Bevor das Zeichen gesendet wird, prüft die Funktion, ob der Sendepuffer bereit ist.

```assembly
.global k_uart_write_char
k_uart_write_char:
    push    {r1-r3}              @ Sichert r1-r3
    mov     r2, #0x20
    ldr     r1, =UART0_FR
```

Zuerst wird das Register `r1` mit der Adresse des Flag-Registers (`UART0_FR`) geladen, und `r2` wird mit `0x20` gesetzt, um Bit 5 (Transmit FIFO Full) zu maskieren.

```assembly
uart_wr_checkfr:
    ldr     r3, [r1]
    tst     r3, r2
    bne     uart_wr_checkfr       @ Wartet, bis der Sendepuffer bereit ist
```

Die Funktion prüft, ob das Bit für den vollen FIFO (Transmit FIFO Full) gesetzt ist. Solange dieses Bit gesetzt ist, bleibt die Funktion in einer Schleife und wartet darauf, dass der Sendepuffer frei wird.

```assembly
uart_wr_print:
    ldr     r1, =UART0_DR
    strb    r0, [r1]              @ Schreibt das zu sendende Zeichen in das Datenregister
    pop     {r1-r3}               @ Stellt Register wieder her
    bx      lr                    @ Rückkehr zur aufrufenden Funktion
```

Wenn der Sendepuffer bereit ist, wird das zu sendende Zeichen (das sich in `r0` befindet) in das Datenregister (`UART0_DR`) geschrieben, und die gesicherten Register werden wiederhergestellt. Anschließend kehrt die Funktion zurück.


### Funktion `k_uart_read_char`

Die Funktion `k_uart_read_char` liest ein einzelnes Zeichen von der UART0-Schnittstelle, indem sie prüft, ob der Empfangspuffer bereit ist.

```assembly
.global k_uart_read_char
k_uart_read_char:
    mov     r2, #0x10
    ldr     r1, =UART0_FR
```

Zunächst wird `r2` mit `0x10` gesetzt, um Bit 4 (Receive FIFO Empty) zu maskieren. `r1` wird mit der Adresse des Flag-Registers (`UART0_FR`) geladen.

```assembly
uart_rd_checkfr:
    ldr     r3, [r1]
    tst     r3, r2
    bne     uart_rd_checkfr       @ Wartet, bis Daten im Empfangspuffer verfügbar sind
```

Die Funktion prüft, ob das Bit für einen leeren Empfangspuffer gesetzt ist. Solange der Empfangspuffer leer ist, bleibt die Funktion in einer Schleife und wartet, bis Daten empfangen wurden.

```assembly
uart_rd_ret:
    ldr     r1, =UART0_DR
    ldrb    r0, [r1]              @ Liest das empfangene Zeichen aus dem Datenregister
    bx      lr                    @ Rückkehr zur aufrufenden Funktion
```

Sobald Daten verfügbar sind, wird das Zeichen aus dem Datenregister (`UART0_DR`) gelesen und in `r0` gespeichert. Danach kehrt die Funktion zurück.


|-----------------------------|------------------------------------|------------------------------------------|
|   [zurück](k_uart_help.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](../Interrupts/intintro.md)    |


|**1.8 UART**                             |
|-----------------------------------------|
| [1.8.1 Intro](uart.md)                  |
| [1.8.2 Übung ](k_uart_ue.md)            |