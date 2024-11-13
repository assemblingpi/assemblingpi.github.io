# B.3 Implementierung systemnaher Funktionen
## 3.1.2 Systemnahe Funktionen: Lösung zur Implementierung von Speicherfunktionen in ARM-Assembly

Der vorliegende Code implementiert drei grundlegende Speicherfunktionen in ARM-Assembly: `memset`, `memcpy` und `memcmp`.

### memset

```assembly
memset: 
    cmp r2, #0
    beq memset_end
    sub r2, r2, #1
    strb r1, [r0, r2]
    b memset
memset_end:
    bx lr
```

Die Funktion `memset` füllt einen Speicherbereich mit einem konstanten Wert. Sie durchläuft den Speicher rückwärts, ausgehend von der Größe des Speicherbereichs (`r2`), und speichert den Wert aus `r1` in jedes Byte. Die Schleife läuft solange, bis `r2` (die Größe) null erreicht. Durch die rückwärtsgerichtete Iteration (`sub r2, r2, #1`) werden die einzelnen Speicherstellen nacheinander beschrieben. Sobald alle Speicherstellen gesetzt sind, kehrt die Funktion mit `bx lr` zurück.

### memcpy

```assembly
memcpy:
    cmp r1, r0
    bhi memcpy_forward
    add r3, r1, r2
    cmp r0, r3
    bhi memcpy_forward
memcpy_reverse:
    subs r2, r2, #1
    ldrb r3, [r1, r2]
    strb r3, [r0, r2]
    beq memcpy_end
    b memcpy_reverse
    b .
memcpy_forward:
    ldrb r3, [r1], #1
    strb r3, [r0], #1
    subs r2, r2, #1
    bgt memcpy_forward
memcpy_end:
    bx lr
```

Die Funktion `memcpy` kopiert einen Speicherbereich von der Quelle (`r1`) zum Ziel (`r0`). Zunächst wird geprüft, ob die Speicherbereiche sich überlappen. Wenn die Zieladresse oberhalb der Quelladresse liegt, erfolgt die Speicherkopie von hinten nach vorne, um sicherzustellen, dass sich die Datenbereiche nicht überschneiden. Liegt die Zieladresse hingegen vor der Quelladresse, wird der Speicherbereich in normaler, vorwärtsgerichteter Reihenfolge kopiert.

Im Vorwärtsmodus wird jedes Byte nacheinander von der Quelladresse gelesen (`ldrb r3, [r1], #1`) und an die Zieladresse geschrieben (`strb r3, [r0], #1`). Dies wiederholt sich, bis alle Bytes kopiert wurden. Im Rückwärtsmodus erfolgt die Kopie ebenfalls byteweise, allerdings rückwärts, um das Überschreiben des Quellbereichs zu verhindern.

### memcmp

```assembly
memcmp: 
    mov     r3, r0
    mov     r0, #0
    push    {r4, r5}
mem_cmp_loop:    
    cmp     r2, #0
    beq     mem_cmp_end
    ldrb    r4, [r3], #1
    ldrb    r5, [r1], #1
    cmp     r4, r5
    bne     mem_cmp_ne
    subs    r2, r2, #1
    b       mem_cmp_loop
mem_cmp_ne:
    mov     r0, #1
mem_cmp_end:
    pop     {r4, r5}
    bx      lr
```

Die Funktion `memcmp` vergleicht zwei Speicherbereiche (`r0` und `r1`) byteweise. In jedem Schleifendurchlauf werden je ein Byte aus dem Quell- und dem Zielbereich geladen (`ldrb r4, [r3], #1` und `ldrb r5, [r1], #1`) und verglichen. Wenn die Bytes übereinstimmen, wird zur nächsten Speicherstelle übergegangen. Sind sie jedoch unterschiedlich, wird der Rückgabewert `r0` auf **1** gesetzt (`mov r0, #1`), und die Funktion kehrt zurück. Bei vollständiger Übereinstimmung bleibt der Rückgabewert **0**.

|-----------------------|------------------------------------|-----------------------------|
|   [zurück](memue.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](format_ue.md)    |


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