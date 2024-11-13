# B.3 Implementierung systemnaher Funktionen
## 3.1.7 Systemnahe Funktionen: Lösung Implementierung einer `kwrite`-Funktion

Der gegebene ARM-Assembler-Code implementiert die Funktion `kwrite`, die Daten an verschiedene Ausgabestellen schreibt, wie zum Beispiel an das Display, eine Datei oder über UART für Fehlerausgaben. Diese Funktion wird beispielsweise von `kprintf` verwendet, um formatierte Ausgaben zu ermöglichen.

### Datenabschnitt

Zu Beginn wird im Datenabschnitt eine Konstante definiert:

```assembly
.section .data
newline: .byte  0x0d, 0x0a, 0x00
```

Hier wird die Zeichenkette `newline` definiert, die aus einem Carriage Return (`0x0d`), einem Line Feed (`0x0a`) und einer Nullterminierung (`0x00`) besteht. Diese Sequenz wird verwendet, um Zeilenumbrüche zu handhaben.

### Funktion `kwrite`

Die Hauptfunktion `kwrite` wird deklariert und beginnt mit der Sicherung wichtiger Register:

```assembly
.global kwrite
.section .text

kwrite:
    push    {lr}
    push    {r11}
    mov     r11, sp
```

Hier werden das Link-Register (`lr`) und der Frame-Pointer (`r11`) auf dem Stack gesichert. Anschließend wird `r11` auf den aktuellen Stack-Pointer gesetzt, um einen festen Bezugspunkt für lokale Variablen zu haben.

Die Funktion erwartet folgende Eingaben:

- `r0`: OUTPUT-Stream ([0] DISPLAY, [1] File, [2/3] ERROR)
- `r1`: Adresse des auszugebenden Strings
- `r2`: Anzahl der auszugebenden Bytes

#### Auswahl des Ausgabehandlers

Der nächste Abschnitt verwendet eine Sprungtabelle, um je nach Wert von `r0` zum entsprechenden Ausgabehandler zu springen:

```assembly
    and     r0, r0, #0x3
    adr     r3, stdout_tbl
    ldr     pc, [r3, r0, lsl #2]
    b       .                @ Sollte nicht erreichbar sein

stdout_tbl:
    Out_0:
        .word Out_0_handler  @ Ausgabe an Display
    Out_1:
        .word Out_1_handler  @ Ausgabe an Datei
    Out_2:
        .word Out_2_handler  @ Ausgabe über UART 
    Out_3:
        .word Out_2_handler  @ Für Tabellen-Alignment
```

In diesem Abschnitt wird der Wert in `r0` auf die unteren 2 Bits beschränkt, um eine Zahl zwischen 0 und 3 zu erhalten. Anschließend wird die Adresse der Sprungtabelle `stdout_tbl` in `r3` geladen. Der Programm Counter (`pc`) wird dann mit der Adresse des entsprechenden Handlers aus der Tabelle gesetzt, basierend auf dem Wert von `r0`. Der letzte Befehl ist ein Platzhalter, der nicht erreicht werden sollte.

Die Sprungtabelle `stdout_tbl` enthält die Adressen der Handler für die verschiedenen Ausgabetypen:

- `Out_0_handler`: Handler für die Ausgabe an das Display.
- `Out_1_handler`: Handler für die Ausgabe an eine Datei (hier unvollständig).
- `Out_2_handler`: Handler für die Ausgabe über UART, verwendet für Fehlerausgaben.
- `Out_3` zeigt ebenfalls auf `Out_2_handler` für das Tabellen-Alignment.

#### Handler `Out_0_handler` (Ausgabe an Display)

Der Handler für die Ausgabe an das Display verarbeitet die zu schreibenden Bytes und gibt sie zeichenweise aus:

```assembly
Out_0_handler:
    mov     r3, #0

Out_0_loop:
    cmp     r3, r2
    bhs     Out_0_loop_end
    ldrb    r0, [r1, r3]
```

Der Handler beginnt mit der Initialisierung des Zählers `r3`, der die Anzahl der verarbeiteten Bytes verfolgt. In der Schleife wird `r3` mit der Gesamtzahl der auszugebenden Bytes verglichen. Wenn alle Bytes verarbeitet sind, wird zur Schleifenende gesprungen. Andernfalls wird das nächste Byte aus dem Eingabepuffer in `r0` geladen, um es weiter zu verarbeiten.

Der nächste Abschnitt überprüft, ob ein spezielles Zeichen (möglicherweise ein Zeilenumbruch) erkannt wurde:

```assembly
out_0_check_newline:
    cmp     r0, #0x2f
    bne     out_0_output
    push    {r0}
    add     r0, r3, #1
    ldrb    r0, [r1, r0]
    cmp     r0, #0x6e
    pop     {r0}
    bne     out_0_output
```

- Hier wird geprüft, ob das aktuelle Zeichen ein `/` (ASCII-Wert `0x2f`) ist.
- Wenn ja, wird das nächste Zeichen geladen und geprüft, ob es ein `n` (ASCII-Wert `0x6e`) ist.
- Wenn die Sequenz `/n` erkannt wird, handelt es sich um einen Zeilenumbruch.

Wenn ein Zeilenumbruch erkannt wurde, wird die Funktion `out_0_print_newline` aufgerufen:

```assembly
out_0_newline_output:
    bl      out_0_print_newline
    add     r3, r3, #2
    cmp     r3, r2
    bhs     Out_0_loop_end
    b       Out_0_loop
```

Zunächst wird die Funktion `out_0_print_newline` aufgerufen, um einen Zeilenumbruch auszugeben. Der Zähler `r3` wird um 2 erhöht, da zwei Zeichen verarbeitet wurden. Danach wird die Schleife fortgesetzt, es sei denn, das Ende der Verarbeitung ist erreicht.

Wenn kein Zeilenumbruch vorliegt, wird das Zeichen normal ausgegeben:

```assembly
out_0_output:
    push    {r0-r3}
    mov     r1, r0
    bl      text_printchar
    pop     {r0-r3}
    add     r3, r3, #1
    b       Out_0_loop
```

In dieser Schleife wird das aktuelle Zeichen zunächst auf den Stack gesichert. Danach wird die Funktion `text_printchar` aufgerufen, um ein einzelnes Zeichen auf dem Display auszugeben. Anschließend wird der Zähler in `r3` um 1 erhöht, bevor die Schleife fortgesetzt wird.

Die Schleife endet, wenn alle Bytes verarbeitet sind:

```assembly
Out_0_loop_end:
    b       write_end
```

#### Funktion `out_0_print_newline`

Diese Funktion gibt einen Zeilenumbruch auf dem Display aus:

```assembly
out_0_print_newline:
    push    {lr}
    push    {r0-r3}
    mov     r0, #2
    ldr     r1, =0xa
    bl      text_printchar
    pop     {r0-r3}
    pop     {lr}
    bx      lr
```

Diese Funktion gibt einen Zeilenumbruch auf dem Display aus. Die Funktion `text_printchar` wird aufgerufen, um den Zeilenumbruch auszugeben. Zum Abschluss werden die Register wiederhergestellt, und die Funktion kehrt zum Aufrufer zurück.

#### Handler `Out_1_handler` (Ausgabe an Datei)

Der Handler für die Ausgabe an eine Datei ist im Code angedeutet, aber nicht implementiert:

```assembly
Out_1_handler:
    // ...
    b       .
```

Dieser Abschnitt würde die Logik für das Schreiben in eine Datei enthalten, ist aber hier nicht weiter ausgeführt.

#### Handler `Out_2_handler` (Ausgabe über UART)

Der Handler für die Fehlerausgabe über UART ist dem Display-Handler ähnlich aufgebaut:

```assembly
Out_2_handler:
    mov     r3, #0

Out_2_loop:
    cmp     r3, r2
    bhs     Out_2_loop_end
    ldrb    r0, [r1, r3]
```

Die Schleife verarbeitet die zu schreibenden Bytes analog zum Display-Handler. Es wird geprüft, ob ein Zeilenumbruch erkannt wird:

```assembly
out_2_check_newline:
    cmp     r0, #0x2f
    bne     out_2_output
    push    {r0}
    add     r0, r3, #1
    ldrb    r0, [r1, r0]
    cmp     r0, #0x6e
    pop     {r0}
    bne     out_2_output
```

Wenn ein Zeilenumbruch erkannt wird, wird `out_2_print_newline` aufgerufen:

```assembly
out_2_newline_output:
    bl      out_2_print_newline
    add     r3, r3, #2
    cmp     r3, r2
    bhs     Out_2_loop_end
    b       Out_2_loop
```

Andernfalls wird das Zeichen über UART ausgegeben:

```assembly
out_2_output:
    bl      k_uart_write_char
    add     r3, r3, #1
    b       Out_2_loop
```

In dieser Schleife wird zunächst die Funktion `k_uart_write_char` aufgerufen, um ein Zeichen über UART zu senden. Anschließend wird der Zähler in `r3` um eins erhöht, bevor die Schleife fortgesetzt wird.

Die Schleife endet, wenn alle Bytes verarbeitet sind:

```assembly
Out_2_loop_end:
    b       write_end
```

#### Funktion `out_2_print_newline`

Diese Funktion gibt einen Zeilenumbruch über UART aus:

```assembly
out_2_print_newline:
    push    {lr}
    push    {r0-r3}
    mov     r0, #2
    ldr     r1, =newline
    ldr     r2, =2
    bl      kwrite
    pop     {r0-r3}
    pop     {lr}
    bx      lr
```

Diese Funktion gibt einen Zeilenumbruch über UART aus. Zunächst werden die Register gesichert. Der OUTPUT-Stream wird auf UART gesetzt, die Adresse der `newline`-Sequenz und die Anzahl der auszugebenden Bytes werden geladen. Anschließend wird `kwrite` aufgerufen, um den Zeilenumbruch zu senden. Zum Abschluss werden die Register wiederhergestellt, und die Funktion kehrt zum Aufrufer zurück.

#### Abschluss der Funktion `kwrite`

Am Ende stellt `kwrite` den Stack wieder her und kehrt zum Aufrufer zurück:

```assembly
write_end:
    mov     sp, r11
    pop     {r11}
    pop     {lr}
    bx      lr
    b       .
```

Am Ende von `kwrite` wird der Stack wiederhergestellt und zur aufrufenden Funktion zurückgekehrt. Zunächst wird der Stack-Pointer auf den Wert des Frame-Pointers gesetzt, anschließend der ursprüngliche Frame-Pointer und die Rücksprungadresse wiederhergestellt. Schließlich erfolgt der Rücksprung zur aufrufenden Funktion durch das Kommando `bx lr`.

|---------------------------|------------------------------------|----------------------------|
|   [zurück](kwrite_ue.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](kread_ue.md)    |


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