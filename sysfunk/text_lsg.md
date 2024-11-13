# B.3 Implementierung systemnaher Funktionen
## 3.1.6 Systemnahe Funktionen: Lösung Textdarstellung via Textmode

Der vorliegende Code implementiert ein grundlegendes System zur Textdarstellung im Textmodus. Er verwaltet den aktuellen Positionierungsindex in der Texttabelle, ermöglicht das Drucken und Löschen von Zeichen sowie das Handling von Zeilenumbrüchen. Durch die Verwendung von globalen und externen Symbolen lässt sich dieser Code problemlos in größere Systeme integrieren, um die Textausgabe auf dem Bildschirm zu steuern und darzustellen.

Der Code umfasst die Deklaration von globalen und externen Symbolen, die Definition von Daten sowie mehrere Funktionen zur Zeichendarstellung, zur Verwaltung des Indexes und zur Steuerung von Textoperationen. Dabei werden wichtige Funktionen wie das Drucken und Löschen von Zeichen, das Setzen von Zeilenumbrüchen und das Vorwärts- und Rückwärtsbewegen im Textpuffer realisiert.

Dank dieser Struktur bietet der Code eine effiziente und leicht anpassbare Grundlage für die Textverarbeitung und -ausgabe in textbasierten Umgebungen.

### 1. Deklarationen und Datenabschnitt

```
.global textmode_state
.global text_printchar
.global text_inc_table_index
.global text_newline
.global text_del
.global text_current_index

.extern char_base
.extern textmode_get_tabentry 
.section .data 
    table_index:    .word 0x00          @ Welches 8x8 Element soll beschrieben werden?
    textmode_state: .word 0x00          @ damit beim einlesen auch auf Bildschirm ausgegeben werden kann
```

Im Anfangsteil des Codes werden globale Symbole deklariert, die von anderen Modulen aus zugänglich sind, sowie externe Symbole, die auf Ressourcen außerhalb dieses Codes verweisen. Der Datenabschnitt definiert zwei wichtige Variablen:
- **`table_index`**: Speichert den aktuellen Index des 8x8-Elements, das beschrieben werden soll.
- **`textmode_state`**: Hält den Zustand des Textmodus fest, um sicherzustellen, dass Texteingaben auch auf dem Bildschirm ausgegeben werden können.

### 2. Funktion `text_printchar`

```
.section .text
text_printchar:                         @ r1 = ascii
    push    {lr}
    push    {r11}
    mov     r11, sp
    push    {r4 -r6}
    @ enter?
    cmp     r1, #0xa
    beq     text_pr_newline
    @ select char
    ldr     r0, =char_base
    lsl     r1, r1, #7
    add     r0, r0, r1
    push    {r0}
    ldr     r1, =table_index
    ldr     r1, [r1]
    mov     r2, #0
    bl      textmode_get_tabentry       @ r0 = 8x8-Block-Base-Adresse
    pop     {r1}                        @ r1 = Charakter_base-Adresse
    mov     r3,  #0
    mov     r4, #0
    mov     r5, #0
    mov     r6, #0

rowloop:
    add     r5, r3, r6
    ldrh    r2, [r1]
    add     r1, r1, #2
    strh    r2, [r0, r5]
    add     r3, r3, #2
    cmp     r3, #16
    bhs     rowloopend
    b       rowloop

rowloopend:
    mov     r3, #0
    cmp     r4, #7
    bhs     colend
    add     r4, r4, #1
    ldr     r5, =1280
    mul     r6, r4, r5
    b       rowloop
    
colend:
    bl      text_inc_table_index
    b       text_pr_end
    
text_pr_newline:
    bl      text_newline
    
text_pr_end:
    pop     {r4 - r6}
    mov	    sp, r11
    pop	    {r11}
    pop	    {pc}
```

Diese Funktion ist verantwortlich für das Drucken eines einzelnen Zeichens auf dem Bildschirm. Sie überprüft zunächst, ob das übergebene Zeichen ein Zeilenumbruch (`\n`) ist. Falls ja, wird die Funktion zur Behandlung eines neuen Zeilenstarts aufgerufen. Andernfalls wird das entsprechende Zeichen aus der Zeichenbasis (`char_base`) geladen und an die korrekte Position im Texttabellen-Speicher geschrieben. Dies erfolgt durch eine Schleife, die die einzelnen Pixelzeilen des 8x8-Zeichens verarbeitet und in den Speicher einfügt. Nach dem Drucken eines Zeichens wird der Tabellenindex erhöht, um die Position für das nächste Zeichen vorzubereiten.

### 3. Funktion `text_inc_table_index`

```
text_inc_table_index:
    ldr     r0, =table_index
    ldr     r1, [r0]
    add     r1, r1, #1
    str     r1, [r0]
    bx      lr 
```

Diese Funktion erhöht den aktuellen Tabellenindex um eins. Sie wird nach dem Drucken eines Zeichens aufgerufen, um den Index für das nächste zu druckende Zeichen zu aktualisieren. Dadurch wird sichergestellt, dass Zeichen nacheinander in der Texttabelle platziert werden.

### 4. Funktion `text_del`

```
text_del:
    ldr     r0, =table_index
    ldr     r1, [r0]
    sub     r1, r1, #2
    str     r1, [r0] 
    bx      lr 
```


Die Funktion `text_del` dient dazu, den Tabellenindex zu verringern, wodurch das zuletzt eingefügte Zeichen effektiv gelöscht wird. Dies ist nützlich für Rückgängig-Operationen oder das Entfernen von Zeichen bei Bedarf.

### 5. Funktion `text_newline`

```
.equ row_length, 80 
text_newline:
    ldr     r0, =table_index
    mov     r3, #row_length
    ldr     r1, [r0]    
    udiv    r2, r1, r3         @ r1 = Reihenindex-alt 
    add     r2, r2, #1
    mul     r1, r2, r3
    str     r1, [r0]
    bx      lr
```

Diese Funktion behandelt den Zeilenumbruch. Sie berechnet den neuen Tabellenindex basierend auf der definierten Zeilenlänge (`row_length`), indem sie den aktuellen Index durch die Anzahl der Zeichen pro Zeile teilt, den Reihenindex erhöht und den Tabellenindex entsprechend anpasst. Dadurch wird der Cursor an den Anfang der nächsten Zeile gesetzt, um neue Zeichen korrekt unterhalb der vorherigen Zeile zu platzieren.

### 6. Funktion `text_current_index`

```
text_current_index:
    push    {lr}
    mov     r1, #0
    bl      text_printchar      @ r1 = ascii
    ldr     r0, =table_index
    ldr     r1, [r0]
    sub     r1, r1, #1
    str     r1, [r0]
    pop	    {pc}
    bx      lr 
```

Die Funktion `text_current_index` gibt den aktuellen Tabellenindex zurück. Sie ruft die `text_printchar`-Funktion auf, um ein spezielles Zeichen zu drucken, und passt dann den Tabellenindex an, um den aktuellen Zustand widerzuspiegeln. Dies ermöglicht es anderen Teilen des Programms, den aktuellen Position im Textmodus zu ermitteln und darauf basierend weitere Operationen durchzuführen.

|---------------------------|------------------------------------|-----------------------------|
|   [zurück](text_ue.md)    |   [Hauptmenü](../ueberblick.md)    |   [weiter](kwrite_ue.md)    |


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