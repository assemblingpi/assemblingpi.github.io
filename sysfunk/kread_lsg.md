# B.3 Implementierung systemnaher Funktionen
## 3.1.8 Systemnahe Funktionen: Lösung Implementierung einer Eingabefunktion
### `kread`-Funktion

Die Funktion `kread` liest Daten von verschiedenen Eingabequellen und speichert sie in einem bereitgestellten Puffer. Sie gibt die Anzahl der gelesenen Bytes zurück. Die Funktion hat folgende Parameter:

- **`r0`**: Eingabestream ([0] für Datei, [1] für UART)
- **`r1`**: Zieladresse für den eingelesenen String
- **`r2`**: Anzahl der zu lesenden Bytes

#### Deklarationen und Daten

```
.global kread
.extern k_uart_read_char
.extern k_uart_write_char 
.extern textmode_state
.extern text_newline
.extern text_current_index
.section .data 

  newline:    .asciz "/n" 
```

Globale Deklarationen werden vorgenommen, um die Funktion `kread` und externe Funktionen wie `k_uart_read_char`, `k_uart_write_char`, sowie einige Zustandsvariablen (`textmode_state`, `text_newline`, `text_current_index`) bekannt zu machen. Im Datenabschnitt wird die Zeichenkette `newline` definiert, die das Zeichen `/n` enthält.

#### Beginn der Funktion `kread`

```
.section .text

kread:
    push   {lr}
    push   {r11}
    mov    r11, sp
    and    r0, r0, #0x1
    adr    r3, input_tbl
    ldr    pc, [r3, r0, lsl #2]
    b      . 
```

Die Funktion `kread` beginnt mit dem Sichern des Link-Registers (`lr`) und des Frame-Registers (`r11`) auf dem Stack. Anschließend wird `r11` auf den aktuellen Stack-Pointer (`sp`) gesetzt, um den Frame-Pointer zu initialisieren. Mit einer AND-Operation wird sichergestellt, dass der Wert in `r0` entweder 0 oder 1 ist, um den Eingabestream festzulegen. Danach wird die Adresse der Sprungtabelle `input_tbl` in `r3` geladen, und es erfolgt ein Sprung zur entsprechenden Handler-Funktion basierend auf dem Wert von `r0`. Der Befehl `b .` dient als Platzhalter für einen nicht erreichbaren Codebereich.

#### Sprungtabelle für Eingaberoutinen

```
input_tbl:
   in_0:  .word in_0_handler    @ read from File
   in_1:  .word in_1_handler    @ read from uart
   b      .
```

Die Sprungtabelle `input_tbl` enthält die Adressen der Handler-Funktionen für die Eingabe aus einer Datei (`in_0_handler`) und für die UART-Eingabe (`in_1_handler`). Der Befehl `b .` dient als Platzhalter und Endemarkierung, sollte jedoch nicht erreicht werden.

#### Handler für Eingabestream 0 (Datei)

```
in_0_handler:                   @ read from File
    b     .
```

**Nicht implementiert**: Enthält nur eine Endlosschleife, da das Lesen von Dateien nicht implementiert ist.

#### Handler für Eingabestream 1 (UART)

```
in_1_handler:                   @ read from uart
    sub   r2, r2, #1
    mov   r3, #0                @ count
    push  {r0-r3}
    ldr   r0, =textmode_state
    ldr   r1, [r0]
    cmp   r1, #0
    pop   {r0-r3}
    bne   scan_loop_text_out
```

Der Handler für Eingabestream 1 (UART) beginnt damit, `r2` um 1 zu verringern, um Platz für das Nullterminierungszeichen zu schaffen, und initialisiert `r3` als Zähler für gelesene Bytes. Anschließend werden die Register `r0` bis `r3` auf dem Stack gesichert. Der Wert von `textmode_state` wird in `r1` geladen und mit 0 verglichen. Ist `r1` ungleich 0, wird zu `scan_loop_text_out` gesprungen. Danach werden die Register `r0` bis `r3` wiederhergestellt.

#### UART-Eingabeschleife (Nicht-Textmodus)

```
scan_loop_uart_out:
    push  {r1,r2,r3}
    bl    k_uart_read_char
    pop   {r1,r2,r3}
    cmp   r0, #0xD              @ CR?
    bne   scan_not_enter_uart_out
```

In der UART-Eingabeschleife (Nicht-Textmodus) wird zunächst der aktuelle Zustand der Register `r1`, `r2` und `r3` auf dem Stack gesichert. Dann wird die Funktion `k_uart_read_char` aufgerufen, um ein Zeichen von der UART-Schnittstelle zu lesen, das in `r0` gespeichert wird. Nach dem Wiederherstellen der Register wird `r0` mit `0xD` (Carriage Return) verglichen. Falls das gelesene Zeichen kein Carriage Return ist, wird zu `scan_not_enter_uart_out` gesprungen.

#### Verarbeitung von Carriage Return (Nicht-Textmodus)

```
scan_enter_uart_out: 
    mov   r0, #2
    ldr   r1, =newline
    mov   r2, #2
    push  {r3}
    bl    kwrite
    pop   {r3}
    cmp   r3, #0
    movne r0, #0
    bne   scan_end_correct
    b     scanstr_error
```

In diesem Abschnitt wird ein Zeilenvorschub ausgegeben, indem `r0`, `r1` und `r2` mit den entsprechenden Werten für die Funktion `kwrite` vorbereitet werden, um die Zeichenkette `newline` auszugeben. Danach wird der Zustand von `r3` auf dem Stack gesichert und die Funktion aufgerufen. Nach der Rückkehr wird `r3` mit 0 verglichen: Bei einem ungleichen Wert (Erfolg) wird zu `scan_end_correct` gesprungen, andernfalls erfolgt ein Sprung zu `scanstr_error` im Fehlerfall.

#### Verarbeitung anderer Zeichen (Nicht-Textmodus)

```
scan_not_enter_uart_out:
    cmp   r0, #0x8
    beq   read_del
    cmp   r0, #0x20 
    blo   scan_loop_uart_out
```

Hier wird zunächst geprüft, ob das gelesene Zeichen ein Backspace (`0x8`) ist. Falls ja, wird zu `read_del` gesprungen. Anschließend wird geprüft, ob das Zeichen ein Steuerzeichen ist (unterhalb von `0x20`). Solche Steuerzeichen werden ignoriert, indem die Schleife erneut zu `scan_loop_uart_out` springt.

#### Tastaturanpassung (Nicht-Textmodus)

```
ami_ger_tastatur:
    cmp   r0, #0x59     @ Y -> Z
    addeq r0, r0, #1 
    beq   ami_ger_skip
    cmp   r0, #0x5a     @ Z -> Y
    subeq r0, r0, #1
    beq   ami_ger_skip
    cmp   r0, #0x79     @ Y -> Z
    addeq r0, r0, #1
    beq   ami_ger_skip
    cmp   r0, #0x7a     @ z -> y
    subeq r0, r0, #1

ami_ger_skip:
    bl    k_uart_write_char 
    strb  r0, [r1, r3]
    cmp   r3, r2
    add   r3, r3, #1
    bne   scan_loop_uart_out
```

Nun wird eine Anpassung für unterschiedliche Tastaturbelegungen vorgenommen, indem die Tasten 'Y' und 'Z' ausgetauscht werden. Bei einer Übereinstimmung mit 'Y' oder 'Z' wird das Zeichen entsprechend angepasst. Nach der Anpassung wird das Zeichen über `k_uart_write_char` ausgegeben und anschließend im Puffer an der Adresse `[r1, r3]` gespeichert. Der Zähler `r3` wird erhöht, und falls das Ende des Puffers nicht erreicht ist, springt die Schleife zurück zu `scan_loop_uart_out`, um weitere Zeichen zu verarbeiten.

#### Fehlerbehandlung

```
scanstr_error:
    ldr   r0, =0xffffffff
    b     scan_end
```

**Fehlercode setzen**: Lädt `0xffffffff` in `r0`, um einen Fehler anzuzeigen.

#### Erfolgreicher Abschluss der Eingabe

```
scan_end_correct:
    sub   r0, r3, #1    @ stringlänge in r0
```

**Stringlänge berechnen**: Subtrahiert 1 von `r3` und speichert das Ergebnis in `r0`.

#### Funktionsende

```
scan_end:
    mov   sp, r11
    pop   {r11}
    pop   {lr}
    bx    lr
```

Am Funktionsende wird das Stack-Frame aufgelöst, indem der Stack-Pointer (`sp`), das Frame-Register (`r11`), und das Link-Register (`lr`) wiederhergestellt werden. Anschließend kehrt die Funktion mit dem Befehl `bx lr` zum Aufrufer zurück.

#### Verarbeitung von Backspace (Nicht-Textmodus)

```
read_del:
    cmp   r3, #0
    beq   scan_loop_uart_out
    bl    k_uart_write_char
    sub   r3, r3, #1
    b     scan_loop_uart_out
```

Es wird geprüft, ob es Zeichen gibt, die gelöscht werden können, indem `r3` mit 0 verglichen wird. Ist `r3` gleich 0, gibt es nichts zu löschen, und die Schleife kehrt zu `scan_loop_uart_out` zurück. Andernfalls wird ein Backspace-Zeichen ausgegeben, und `r3` wird um 1 verringert, bevor die Schleife erneut zu `scan_loop_uart_out` springt.

#### Überspringen des Speicherns (Nicht-Textmodus)

```
skip_save:
    bl    k_uart_write_char 
    b     scan_loop_uart_out
```

Das Zeichen wird direkt mit `k_uart_write_char` ausgegeben, ohne es im Puffer zu speichern. Anschließend wird die Schleife fortgesetzt, indem zurück zu `scan_loop_uart_out` gesprungen wird.

#### UART-Eingabeschleife (Textmodus)

```
scan_loop_text_out:
    push  {r0-r3}
    bl    text_current_index
    pop   {r0-r3}
    push  {r1,r2,r3}
    bl    k_uart_read_char
    pop   {r1,r2,r3}
    cmp   r0, #0xD // CR?
    bne   scan_not_enter_text_out
```

In dieser Schleife wird zunächst der aktuelle Textindex über den Aufruf von `text_current_index` abgerufen. Anschließend wird ein Zeichen von der UART-Schnittstelle eingelesen. Wird dabei kein Carriage Return (`0xD`) erkannt, erfolgt ein Sprung zu `scan_not_enter_text_out`.

#### Verarbeitung von Carriage Return (Textmodus)

```
scan_enter_text_out: 
    push  {r0-r3}
    mov   r1, #1
    bl    text_printchar
    bl    text_newline
    mov   r0, #2
    ldr   r1, =newline
    mov   r2, #2
    push  {r3}
    bl    kwrite
    pop   {r3}
    pop   {r0-r3}
    cmp   r3, #0
    movne r0, #0
    bne   scan_end_correct
    b     scanstr_error	
```

In diesem Abschnitt wird das Zeichen zunächst mit `text_printchar` ausgegeben, gefolgt von einem Zeilenvorschub durch den Aufruf von `text_newline`. Danach wird die Zeichenkette `newline` über `kwrite` ausgegeben. Nach der Ausgabe wird der Erfolg geprüft: Bei erfolgreicher Verarbeitung erfolgt ein Sprung zu `scan_end_correct`, andernfalls zu `scanstr_error`, falls ein Fehler auftritt.

#### Verarbeitung anderer Zeichen (Textmodus)

```
scan_not_enter_text_out:
    cmp   r0, #0x8
    beq   read_del_text
    cmp   r0, #0x20 
    blo   scan_loop_text_out
```

Hier wird geprüft, ob das gelesene Zeichen ein Backspace (`0x8`) ist. Wenn ja, wird zu `read_del_text` gesprungen. Zeichen, die unterhalb von `0x20` liegen (Steuerzeichen), werden ignoriert, und die Schleife springt zurück zu `scan_loop_text_out`, um weitere Eingaben zu verarbeiten.

#### Tastaturanpassung (Textmodus)

```
ami_ger_tastatur_text:
    cmp   r0, #0x59    @ Y -> Z
    addeq r0, r0, #1 
    beq   ami_ger_skip_text
    cmp   r0, #0x5a    @ Z -> Y
    subeq r0, r0, #1
    beq   ami_ger_skip_text
    cmp   r0, #0x79    @ Y -> Z
    addeq r0, r0, #1
    beq   ami_ger_skip_text
    cmp   r0, #0x7a    @ z -> y
    subeq r0, r0, #1
    
ami_ger_skip_text:
    bl    k_uart_write_char 
    push  {r0-r3}
    mov   r1, r0
    bl    text_printchar 
    pop   {r0-r3}
    strb  r0, [r1, r3]
    cmp   r3, r2
    add   r3, r3, #1
    bne   scan_loop_text_out
    b     scanstr_error
```

In dieser Routine wird eine Anpassung für unterschiedliche Tastaturbelegungen vorgenommen, indem die Tasten 'Y' und 'Z' je nach Eingabe vertauscht werden. Nach der Anpassung wird das Zeichen mit `k_uart_write_char` ausgegeben und anschließend mit `text_printchar` im Textmodus dargestellt. Das Zeichen wird außerdem im Puffer an der Adresse `[r1, r3]` gespeichert. Der Zähler `r3` wird erhöht. Solange das Ende des Puffers nicht erreicht ist, wird zurück zu `scan_loop_text_out` gesprungen, andernfalls erfolgt ein Sprung zu `scanstr_error`, falls ein Fehler auftritt.

#### Verarbeitung von Backspace (Textmodus)

```
read_del_text:
    cmp   r3, #0
    beq   scan_loop_text_out
    bl    k_uart_write_char 
    push  {r0 - r3}
    mov   r1, #1
    bl    text_printchar
    bl    text_del
    pop   {r0 -r3}
    sub   r3, r3, #1
    b     scan_loop_text_out
```

Zunächst wird geprüft, ob es Zeichen zum Löschen gibt, indem `r3` mit 0 verglichen wird. Wenn `r3` gleich 0 ist, gibt es nichts zu löschen, und die Schleife springt zurück zu `scan_loop_text_out`. Andernfalls wird ein Backspace-Zeichen ausgegeben, und die Funktionen `text_printchar` sowie `text_del` werden aufgerufen, um das Zeichen im Textmodus zu entfernen. Danach wird `r3` um 1 verringert, und die Schleife setzt sich fort, indem erneut zu `scan_loop_text_out` gesprungen wird.

#### Überspringen des Speicherns (Textmodus)

```
skip_save_text:
    bl    k_uart_write_char 
    push  {r0 - r3}
    mov   r1, r0
    bl    text_printchar 
    pop   {r0 -r3}
    b     scan_loop_text_out
```

In diesem Abschnitt wird das Zeichen zunächst mit `k_uart_write_char` ausgegeben. Anschließend wird das Zeichen im Textmodus durch den Aufruf von `text_printchar` dargestellt. Nach dem Wiederherstellen der Register wird die Schleife fortgesetzt, indem zurück zu `scan_loop_text_out` gesprungen wird.

|--------------------------|------------------------------------|------------------------------|
|   [zurück](kread_ue.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](kprintf_ue.md)    |


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