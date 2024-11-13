# B.2 Erweiterungen der CPU-Funktionalität
## 2.4.7 Grafik & GPU: Senden der Initialisierungsnachricht

Legen sie das File `kframebuff.s` an, das folgenden Inhalt hat:

```
.global FrameBufferInfo
.global FrameBufferWrite
.global InitializeFrameBuffer

.section .data
.align 16
FrameBufferInfo:
    .word 640           @ Physische Breite
    .word 480           @ Physische Höhe
    .word 640           @ Virtuelle Breite
    .word 480           @ Virtuelle Höhe
    .word 0             @ GPU-Pitch (wird von der GPU gesetzt)
    .word 16            @ Farbtiefe (Bits pro Pixel)
    .word 0             @ X-Offset
    .word 0             @ Y-Offset
    .word 0             @ GPU-Pointer (Adresse des Framebuffers, wird von der GPU gesetzt)
    .word 0             @ GPU-Size (Größe des Framebuffers, wird von der GPU gesetzt)

.equ PHY_WIDTH,   0
.equ PHY_HEIGHT,  4
.equ VIRT_WIDTH,  8
.equ VIRT_HEIGHT, 12
.equ GPU_PITCH,   16
.equ BIT_DEPTH,   20
.equ X,           24
.equ Y,           28
.equ GPU_PTR,     32
.equ GPU_SIZE,    36
```

Globale Symbole wie `FrameBufferInfo`, `FrameBufferWrite` und `InitializeFrameBuffer` werden deklariert. Besonders wichtig ist, dass `FrameBufferInfo` 16-Byte-ausgerichtet ist, da dies eine entscheidende Voraussetzung für die korrekte Mailbox-Kommunikation mit der GPU ist. Die Struktur enthält Informationen zur physischen und virtuellen Auflösung (640x480), Farbtiefe (16 Bit pro Pixel) sowie die Offsets für X und Y, die hier auf 0 gesetzt sind. Felder wie `GPU_PITCH`, `GPU_PTR` und `GPU_SIZE` werden nach der Initialisierung von der GPU gefüllt. Die `.equ`-Anweisungen definieren Offsets, um im Code später bequem auf diese Felder zugreifen zu können.

### Funktion `InitializeFrameBuffer`

```
InitializeFrameBuffer:    @ Eingabe: r0 = Breite, r1 = Höhe, r2 = Farbtiefe
    cmp   r0, #4096
    cmpls r1, #4096
    cmpls r2, #32
wrong_input_ret:
    movhi r0, #0
    movhi pc, lr
right_input_continue:
    push  {r4, lr}
    ldr   r4, =FrameBufferInfo
    str   r0, [r4, #PHY_WIDTH]
    str   r1, [r4, #PHY_HEIGHT]
    str   r0, [r4, #VIRT_WIDTH]
    str   r1, [r4, #VIRT_HEIGHT]
    str   r2, [r4, #BIT_DEPTH]
reset_frame_buff:
    mov   r1, #0
    str   r1, [r4, #GPU_PITCH]
    str   r1, [r4, #X]
    str   r1, [r4, #Y]
    str   r1, [r4, #GPU_PTR]
    str   r1, [r4, #GPU_SIZE]
    mov   r0, r4
    bl    FrameBufferWrite
    teq   r0, #0
return_failed:
    movne r0, #0
    popne {r4, pc}
return_success:
    mov   r0, r4
    pop   {r4, pc}
```

Die Funktion `InitializeFrameBuffer` prüft zunächst die Eingabeparameter für Breite, Höhe und Farbtiefe. Dabei wird sichergestellt, dass die Breite und Höhe 4096 nicht überschreiten und die Farbtiefe maximal 32 beträgt. Sind die Werte ungültig, setzt die Funktion `r0` auf 0 und beendet sich direkt.

Wenn die Eingaben gültig sind, sichert die Funktion die Register `r4` und die Rücksprungadresse (`lr`). Anschließend wird die `FrameBufferInfo`-Struktur mit den Eingabewerten gefüllt. Dabei werden die physische und virtuelle Breite sowie Höhe entsprechend den übergebenen Werten gesetzt, ebenso wie die Farbtiefe.

Die GPU-spezifischen Felder wie `GPU_PITCH`, `X`, `Y`, `GPU_PTR` und `GPU_SIZE` werden auf 0 gesetzt, da diese später durch die GPU ausgefüllt werden, nachdem die Framebuffer-Konfiguration erfolgreich an die GPU übergeben wurde.

Die Funktion ruft dann `FrameBufferWrite` auf, um die Konfiguration an die GPU zu senden. Nach dem Schreiben überprüft sie, ob die Operation erfolgreich war. Ist die Kommunikation fehlgeschlagen, wird die Funktion mit dem Fehlercode `r0 = 0` beendet.

Im Erfolgsfall wird die Adresse der `FrameBufferInfo`-Struktur in `r0` zurückgegeben, und die Funktion kehrt zum Aufrufer zurück.

### Funktion `FrameBufferWrite`

```
FrameBufferWrite:
    push {lr}
    orr  r0, #0xC0000000
    mov  r1, #1
    bl   MailboxWrite
    cmp  r0, #0
    bne  FrameBufferWriteError
    mov  r0, #1
    bl   MailboxRead
    cmp  r0, #0
    bne  FrameBufferWriteError
    pop  {pc}

FrameBufferWriteError:
    mov  r0, #1
    pop  {pc}
```

Die Funktion `FrameBufferWrite` beginnt damit, die Rücksprungadresse (`lr`) auf den Stack zu sichern, um später zur aufrufenden Funktion zurückkehren zu können. Danach wird die Adresse, die an die GPU gesendet werden soll (in `r0`), mit `0xC0000000` kombiniert. Dies passt die Speicheradresse an den Adressraum der GPU an, da CPU und GPU unterschiedliche Adressbereiche verwenden.

Um die Framebuffer-Informationen an die GPU zu senden, wird der Mailbox-Kanal vorbereitet, indem `r1` auf den Wert 1 gesetzt wird. Die Funktion `MailboxWrite` wird aufgerufen, um die Adresse der `FrameBufferInfo`-Struktur an die GPU zu übertragen.

Nach dem Senden wird das Ergebnis von `MailboxWrite` überprüft. Falls ein Fehler auftritt (Rückgabewert ungleich 0), wird zur Fehlerbehandlungsroutine `FrameBufferWriteError` gesprungen. Ist das Schreiben erfolgreich, wird `r0` erneut auf 1 gesetzt, um den Mailbox-Kanal für das Lesen vorzubereiten, und `MailboxRead` wird aufgerufen, um die Antwort der GPU zu empfangen.

Auch das Ergebnis von `MailboxRead` wird auf 0 geprüft. Tritt hier ein Fehler auf, wird ebenfalls zur Fehlerbehandlungsroutine gesprungen. Falls keine Fehler aufgetreten sind, wird die Rücksprungadresse wiederhergestellt, und die Funktion kehrt regulär zum Aufrufer zurück.

Sollte während der Kommunikation mit der GPU ein Fehler auftreten, wird in der Fehlerbehandlungsroutine `r0` auf 1 gesetzt, um den Fehler zu signalisieren, und die Funktion endet durch Rücksprung zum Aufrufer.

|--------------------------|-------------------------------|----------------------------------------|
| [zurück](framemailb.md)  | [Hauptmenü](../ueberblick.md) | [weiter](../sysfunk/sysfunkintro.md)   |


|**2.4 Grafik & GPU**                                                       |
|---------------------------------------------------------------------------|
| [2.4.1 Einführung in die Computergrafik & RGB565-Format](grafikintro.md)  |
| [2.4.2 Was ist eine GPU?](gpuintro.md)                                    |
| [2.4.3 Die GPU des BCM2836 VideoCoreIV](gpubcm2836.md)                    |
| [2.4.4 Kommunikation mittels Mailbox](kommb.md)                           |
| [2.4.5 Der Framebuffer und seine Nutzung](framebuff.md)                   |
| [2.4.6 Der Framebuffer-Mailbox-Kanal](framemailb.md)                      |
| [2.4.7 Senden der Initialisierungsnachricht](sendinit.md)                 |