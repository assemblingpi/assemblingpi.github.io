# B.3 Implementierung systemnaher Funktionen
## 3.1.4 Systemnahe Funktionen: Grundlegende Grafikbibliothek Lösung

### 1. **Deklarationen und Datenabschnitt**

```assembly
.global canvas_init
.global put_pixel
.global fillscreen
.global drawline
.global put_pixel
.global canvas_base

.extern InitializeFrameBuffer

.section .data
.align 4

canvas_base: .word 0x0
canvas_error_string: .asciz "canvas_error could not init framebuff /n ________________________ /n"
```

Dieser Abschnitt deklariert globale Symbole und externe Funktionen, die im gesamten Programm verwendet werden. Im Datenabschnitt werden die Basisadresse des Framebuffers (`canvas_base`) und eine Fehlermeldung (`canvas_error_string`) definiert. Diese dienen zur Verwaltung des Framebuffers und zur Fehlerbehandlung bei der Initialisierung.

---

### 2. **Konstanten-Definitionen**

```assembly
.section .text

.equ SCREEN_WIDTH,  640 
.equ SCREEN_HEIGHT, 480 
.equ BIT_DEPTH,     16

.equ CWh,           640 
.equ CHh,           240 

.equ Xwidth,        640 * 2

.equ SCREEN_MAX,    640 * 480
```

Hier werden verschiedene Konstanten definiert, die die Bildschirmauflösung (`SCREEN_WIDTH`, `SCREEN_HEIGHT`), Farbtiefe (`BIT_DEPTH`) und weitere Parameter für die grafische Darstellung festlegen. Diese Konstanten werden in den nachfolgenden Funktionen zur Berechnung von Speicheradressen und zur Steuerung der Grafikoperationen verwendet.

---

### 3. **Funktion `canvas_init`**

```
canvas_init:
	push        {lr}
	mov         r0, #SCREEN_WIDTH
	mov         r1, #SCREEN_HEIGHT
	mov         r2, #BIT_DEPTH
	bl          InitializeFrameBuffer       
	teq         r0, #0                      
	beq         canvas_error

store_canv_base_adr:
	ldr         r3, [r0, #32]			     
	and         r3, #0x3FFFFFFF              
	ldr         r1, =canvas_base
	str         r3, [r1]
	pop         {lr}
	bx          lr
```

Die Funktion `canvas_init` initialisiert den Framebuffer für die Bildschirmausgabe. Sie übergibt die Bildschirmbreite, -höhe und Farbtiefe an die externe Funktion `InitializeFrameBuffer`. Nach der Initialisierung überprüft sie, ob die Rückgabeadresse gültig ist. Bei Erfolg wird die physische Basisadresse des Framebuffers gespeichert. Tritt ein Fehler auf, wird die Fehlerbehandlungsroutine `canvas_error` aufgerufen.

---

### 4. **Fehlerbehandlung `canvas_error`**

```	
canvas_error:
	ldr     r1, =canvas_error_string
	mov     r2, #2
	bl      kprintf        
	b       canvas_error
```

Die Funktion `canvas_error` gibt eine vordefinierte Fehlermeldung aus, wenn die Initialisierung des Framebuffers fehlschlägt. Anschließend wird eine Endlosschleife gestartet, um die weitere Ausführung des Programms zu verhindern.

---

### 5. **Funktion `fillscreen`**

```
fillscreen: 
	mov     r0, r1
	mov     r1, #SCREEN_HEIGHT
	ldr     r3, =canvas_base
	ldr     r3, [r3]

drawRow:
	mov     r2, #SCREEN_WIDTH

drawPixel:
	strh    r0, [r3]             
	add     r3, #2               
	sub     r2, #1               
	teq     r2, #0               
	bne     drawPixel            
	sub     r1, #1               
	teq     r1, #0               
	bne     drawRow
	bx      lr
```

Die Funktion `fillscreen` füllt den gesamten Bildschirm mit einer angegebenen Hintergrundfarbe. Sie durchläuft jede Zeile und jeden Pixel des Bildschirms und setzt den jeweiligen Pixel auf die Farbe, die im Register `r1` übergeben wurde. Nach dem Füllen des gesamten Bildschirms kehrt die Funktion zum Aufrufer zurück.

---

### 6. **Funktion `get_canv_x`**

```	
get_canv_x:
	push   {r1}
	lsl    r1, r1, #1
	ldr    r0, =#CWh
	add    r0, r0, r1
	pop    {r1}
	bx     lr
```

Die Funktion `get_canv_x` konvertiert eine Benutzer-x-Koordinate in das Canvas-Koordinatensystem. Sie berechnet die entsprechende x-Position relativ zum Ursprung des Canvas und gibt das Ergebnis zurück.
(Da ein Pixel 2-Byte groß ist, muss der Benutzerwert mit 2 multipliziert werden)


### 7. **Funktion `get_canv_y`**

```
get_canv_y: 	
	push   {r1-r2}
	ldr    r0, =#CHh
	sub    r1, r0, r1
	ldr    r0, =#Xwidth
	mul    r0, r1, r0
	pop    {r1-r2}
	bx     lr
```

Die Funktion `get_canv_y` konvertiert eine Benutzer-y-Koordinate in das Canvas-Koordinatensystem. Sie berechnet die entsprechende y-Position relativ zum Ursprung des Canvas und gibt das Ergebnis zurück.

---

### 8. **Funktion `put_pixel`**

```
put_pixel: 		
	push   {lr}

check_lims:
	ldr    r0, =#CWh
	cmp    r1, r0
	bge    put_pixel_end_debug
	ldr    r0, =#CHh
	cmp    r2, r0
	bge    put_pixel_end_debug

conv_to_canv_coord:
	bl     get_canv_x
	push   {r0}
	mov    r1, r2
	bl     get_canv_y
	mov    r2, r0
	pop    {r1}
	cmp    r1, #0
	blt    put_pixel_end      
	cmp    r2, #0
	blt    put_pixel_end      
	bl     canvas_put_pixel
	b      put_pixel_end
	
put_pixel_end_debug:
	nop
	nop
	nop

put_pixel_end:	
	pop    {lr}
	bx     lr
```

**Erklärung:**
Die Funktion `put_pixel` setzt einen einzelnen Pixel auf dem Bildschirm an den angegebenen Benutzerkoordinaten mit einer bestimmten Farbe. Zunächst überprüft sie, ob die Koordinaten innerhalb der Bildschirmgrenzen liegen. Anschließend konvertiert sie die Benutzerkoordinaten in das Canvas-Koordinatensystem und setzt den Pixel über die Funktion `canvas_put_pixel`. Bei ungültigen Koordinaten beendet sie die Funktion ohne Aktion.

---

### 9. **Funktion `canvas_put_pixel`**

```	
canvas_put_pixel: 
	ldr   r0, =canvas_base
	ldr   r0, [r0]
	add   r1, r1, r2
	strh  r3, [r0, r1]
	bx    lr
```

Die Funktion `canvas_put_pixel` setzt einen Pixel an den angegebenen Canvas-Koordinaten mit der angegebenen Farbe. Sie greift auf die Basisadresse des Framebuffers zu, berechnet die Speicheradresse des Pixels und speichert die Farbinformation dort ab.

|----------------------------|------------------------------------|-------------------------------|
|   [zurück](canvas_ue.md)   |   [Hauptmenü](../ueberblick.md)    |   [weiter](textmode_ue.md)    |


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