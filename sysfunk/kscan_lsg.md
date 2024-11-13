# B.3 Implementierung systemnaher Funktionen
## 3.1.10 Systemnahe Funktionen: Lösung zur Implementiere `kscan` für formatiertes Einlesen
### 1. **Deklarationen und Datenabschnitt**

Der Anfang des Codes deklariert globale Symbole, externe Funktionen und definiert Konstanten sowie Daten für Fehlermeldungen und einen internen Puffer:

```
.global kscan
.extern kprintf
.extern memset
.extern kread

.equ BASE,              0x00
.equ STR_ADR,           BASE -  0x04  
.equ PARAM_CNT,         BASE -  0x08  
.equ STACKMAX,          BASE -  PARAM_CNT
// --------------------------------------
.equ ARGS,              BASE +  0x04     
// --------------------------------------
.section .data
kscanf_buffer:
                        .space 1024, 0x0
```

Eine globale Funktion `kscan` und externe Funktionen (`kprintf`, `memset`, `kread`) werden definiert. Es werden einige Konstanten zur Speicherverwaltung und Parameterübergabe gesetzt, wie etwa **`BASE`** und **`ARGS`**, die Offsets für verschiedene Speicheradressen festlegen.

Im **Datenabschnitt** wird ein interner Puffer (`kscanf_buffer`) von 1024 Bytes reserviert, der für Eingaben durch `kread` genutzt wird.

### 2. **Funktion `kscan`**

Die Hauptfunktion `kscan` implementiert eine Scanner-Funktion ähnlich der Standard `scanf`-Funktion. Sie verarbeitet einen Formatstring und liest entsprechende Eingaben ein.

```
.section .text
kscan:  			
```

Der **`.text`-Abschnitt** enthält den ausführbaren Code, einschließlich der Funktion `kscan`. Diese Funktion erwartet, dass der **Formatstring** (z.B. ein String mit Platzhaltern wie `%s` oder `%d`) im Register `r1` übergeben wird. Die zugehörigen **Parameter** für die Platzhalter im Formatstring werden über den **Stack** übergeben, wobei die zuerst verwendeten Argumente zuletzt auf den Stack gelegt werden müssen.

### 3. **Initialisierung und Stack-Setup**

Der Anfang der Funktion `kscan` richtet den Stack-Frame ein und speichert den Formatstring sowie den Parameterzähler.

```
	push	{lr}
	push	{r11}
	mov 	r11, sp
	sub 	sp, sp, #STACKMAX
	str 	r1, [r11, #STR_ADR]
	mov 	r2, #0
	str 	r2, [r11, #PARAM_CNT]
	push	{r6} 
```

Zu Beginn der Funktion werden das Link-Register und der Frame-Pointer gesichert. Anschließend wird der Stack-Frame eingerichtet, indem `r11` auf den aktuellen Stack-Pointer gesetzt und Speicherplatz für lokale Variablen reserviert wird. Die Adresse des Formatstrings wird im Stack gespeichert, und der Parameterzähler wird auf 0 initialisiert. Zudem wird das Register `r6` gesichert, da es im späteren Verlauf noch benötigt wird.

### 4. **Hauptschleife zur Verarbeitung des Formatstrings**

Die Funktion durchläuft den Formatstring Zeichen für Zeichen und verarbeitet diese entsprechend.

```
scan_srcstr_loop:
	ldr 	r0, [r11, #STR_ADR]
	ldrb	r1, [r0], #1
	str 	r0, [r11, #STR_ADR]
	cmp 	r1, #0
	beq 	scanf_end_scan
	cmp 	r1, #'%'                 @ Umwandlungszeichen?
	beq 	format_id
	b   	scan_srcstr_loop
```

Die Funktion durchläuft den Formatstring, indem sie jedes Zeichen nacheinander lädt und überprüft. Dabei wird bei jedem Schritt die Adresse des nächsten Zeichens aktualisiert. Wenn das Ende des Strings erreicht wird, wird die Schleife beendet. Wenn ein `%` gefunden wird, was auf einen Formatbezeichner hinweist, wird zu dessen Verarbeitung verzweigt. Ansonsten wird die Schleife fortgesetzt, um das nächste Zeichen zu prüfen.

### 5. **Verarbeitung von Formatbezeichnern**

Wenn ein `%` erkannt wird, wird das nachfolgende Zeichen als Formatbezeichner verarbeitet.

```
format_id:
	ldr 	r1, [r11, #PARAM_CNT]    @ aktueller param index in r2
	add 	r1, r1, #4 
	str 	r1, [r11, #PARAM_CNT]   
	ldr 	r0, [r11, #STR_ADR]
	ldrb	r1, [r0]
	add 	r0, r0, #1
	str 	r0, [r11, #STR_ADR]
	cmp 	r1, #126                 
	bhi 	checkerror
	cmp 	r1, #65                  
	bhi 	checkasc				 
```

Die Funktion verwaltet den aktuellen Parameterindex, indem sie diesen um 4 Bytes erhöht, da jedes Argument im Stack 4 Bytes groß ist. Anschließend wird das nächste Zeichen des Formatstrings geladen, das als Formatbezeichner dient, und die Adresse im Stack wird aktualisiert. Es folgt eine Überprüfung des Bezeichners: Wenn dieser einen ungültigen ASCII-Wert (größer als 126) hat, wird zur Fehlerbehandlung gesprungen. Ist der Bezeichner ein gültiger Buchstabe (ASCII-Wert >= 65), wird zur weiteren Überprüfung verzweigt.

### 6. **Fehlerbehandlung bei ungültigen Formatbezeichnern**

Falls ein unbekannter Formatbezeichner erkannt wird, wird die Funktion mit dem Rückgabewert `-1` beendet:

```
checkerror:
	mvn 	r0, #0 
	b   	scanf_end	
```

### 7. **Ende des Scanvorgangs**

Wenn das Ende des Formatstrings erreicht ist, wird geprüft, ob alle Parameter verarbeitet wurden.

```
scanf_end_scan:
	ldr 	r1, [r11, #PARAM_CNT]
	cmp 	r1, #0     
	bne 	scanf_end
```

Am Ende des Scans wird der Parameterzähler überprüft. Wenn der Zähler auf null steht, bedeutet dies, dass kein Argument verarbeitet wurde, was auf einen Fehler hindeutet. Falls der Zähler nicht null ist, wird zum regulären Ende der Funktion gesprungen.

### 8. **Fehlerbehandlung bei fehlenden Argumenten**

Wenn der Parameterzähler nicht null ist, wird eine entsprechende Fehlerbehandlung eingeleitet:

```
scan_error:
	mvn 	r0, #1 
```

Der Wert `-2` wird in das Register `r0` geladen, und der Kontrollfluss setzt sich mit `scanf_end` fort, wo die Funktion schließlich mit dem Rückgabewert in `r0` beendet wird.

### 9. **Ende der `kscan`-Funktion**

Die Funktion stellt den Stack wieder her und kehrt zum Aufrufer zurück.

```
scanf_end:
	pop 	{r6} 
	mov 	sp, r11
	pop 	{r11}
	pop 	{lr}
	bx  	lr
```

Am Ende der Funktion wird das Register `r6` wiederhergestellt, und das Stack-Frame aufgelöst, indem der Stack-Pointer auf den Wert von `r11` zurückgesetzt wird. Anschließend werden der Frame-Pointer und das Link-Register wiederhergestellt. Schließlich erfolgt die Rückkehr zur aufrufenden Funktion durch einen Sprung zur Adresse im Link-Register (`lr`).


### 10. **ASCII Sprungtabelle und Verarbeitungsfunktionen**

Die Sprungtabelle ordnet jedem unterstützten Formatbezeichner eine entsprechende Funktion zu.

```
checkasc:
	orr 	r1, #32
	sub 	r1, r1, #0x61
	adr 	r0, ascii_jmp_tbl
	ldr 	pc, [r0, r1, lsl #2]
	b   	.

ascii_jmp_tbl:
	a:  	.word checkerror
	b:  	.word checkerror
	c:  	.word checkerror
	d:  	.word sc_is_d
	e:  	.word checkerror
	f:  	.word checkerror
	g:  	.word checkerror
	h:  	.word checkerror
	i:  	.word sc_is_d
	j:  	.word checkerror
	k:  	.word checkerror
	l:  	.word checkerror
	m:  	.word checkerror
	n:  	.word checkerror
	o:  	.word checkerror
	p:  	.word checkerror
	q:  	.word checkerror
	r:  	.word checkerror
	s:  	.word sc_is_s
	t:  	.word checkerror
	u:  	.word checkerror
	v:  	.word checkerror
	w:  	.word checkerror
	x:  	.word sc_is_x
	y:  	.word checkerror
	z:  	.word checkerror

```

Die Funktion `checkasc` verarbeitet Formatbezeichner aus dem Formatstring. Zunächst wird der Buchstabe, falls nötig, von Groß- in Kleinbuchstaben umgewandelt. Anschließend wird der Buchstabe in einen Index für die Sprungtabelle umgerechnet, und die Adresse der entsprechenden Funktion wird geladen. Die Sprungtabelle enthält Zuordnungen für die Buchstaben `d`, `i`, `s`, und `x`, die zu spezifischen Verarbeitungsfunktionen führen, während alle anderen Bezeichner zu einer Fehlerbehandlung (`checkerror`) verzweigen. Der Befehl `b .` ist ein Fallback, der nicht erreicht werden sollte.

### 11. **Verarbeitung von Dezimalzahlen (`sc_is_d`)**

Die folgende Funktion verarbeitet das Einlesen und Konvertieren von Dezimalzahlen (`%d`).

```
sc_is_d:         
	ldr 	r0, =kscanf_buffer
	mov 	r1, #0
	mov 	r2, #11
	bl  	memset                    @ clear buffer

sc_get_val:
	mov 	r0, #1                    
	ldr 	r1, =kscanf_buffer
	mov 	r2, #11
	bl  	kread
	ldr 	r1, =0xffffffff           
	cmp 	r0, r1
	beq 	error_dez
	cmp 	r0, #10                   
	beq 	error_dez
	mov 	r3, r0                    
	mov 	r2, #0
	ldr 	r1, =kscanf_buffer        

sc_dez_check_minus:
	ldrb	r0, [r1, r2]
	cmp 	r0, #0x2d                 
	moveq	r6, #1
	movne	r6, #0
	addeq	r2, r2, #1

sc_dez_check_buff_all:
	ldrb	r0, [r1, r2]              @ lade Byte von Buffer zwecks überprüfung ob Ziffer
	cmp 	r0, #0x30                 @ value < Ascii "0" ?
	blo 	error_wrong_input
	cmp 	r0, #0x39                 @ value > Ascii "9"?
	bhi 	error_wrong_input
	cmp 	r2, r3
	beq 	dez_buff_process
	add 	r2, r2, #1                
	b   	sc_dez_check_buff_all

dez_buff_process:	
	mov 	r0, #0                    
	mov 	r2, #1
	push	{r4,r5}
	add 	r5, r3, #1
	mov 	r3, #0                    
	mov 	r4, #10
	add 	r3, r6, #0

dez2reg_loop:
	ldr 	r1, =kscanf_buffer
	ldrb	r1, [r1, r3]
	sub 	r1, r1, #0x30             @ ASCI -> Value
	mov 	r2, #10
	mla 	r0, r0, r2, r1            @ r0 = (r0 * 10 ^ x ) + r1
	add 	r3, r3, #1
	cmp 	r3, r5
	bne 	dez2reg_loop

dez_end:
	pop 	{r4,r5}                   
	mov 	r1, #0
	cmp 	r6, r1
	subne	r0, r1, r0                @ Resultat ist negativ (zweierkomplement)  			
	ldr 	r1, [r11, #PARAM_CNT] 
	add 	r1, #ARGS
	ldr 	r1, [r11, r1]
	str 	r0, [r1]  
	b   	format_id_end
```

Die Funktion `sc_is_d` verarbeitet die Eingabe eines Dezimalwerts und wandelt ihn in eine Ganzzahl um.

Zunächst wird der Puffer geleert, indem `memset` aufgerufen wird. Danach liest die Funktion mithilfe von `kread` bis zu 11 Zeichen in den Puffer ein und prüft die Eingabe auf Fehler oder Überlänge. Wenn ein Minuszeichen erkannt wird, wird dies in `r6` markiert. Anschließend überprüft die Funktion jedes Zeichen im Puffer darauf, ob es eine gültige Ziffer ist. 

Die Konvertierung der Ziffern erfolgt in der Schleife `dez2reg_loop`, die die ASCII-Zeichen in numerische Werte umwandelt und diese zu einer Ganzzahl zusammensetzt. Am Ende wird die Zahl, falls ein Minuszeichen vorhanden war, negativ gemacht und das Ergebnis im Speicher abgelegt, bevor die Funktion zur weiteren Verarbeitung des Formatstrings zurückkehrt.

### 12. **Verarbeitung von Zeichenketten (`sc_is_s`)**

Diese Funktion verarbeitet das Einlesen und Speichern von Zeichenketten (`%s`).

```
sc_is_s:
	ldr 	r0, =kscanf_buffer
	mov 	r1, #0
	ldr 	r2, =#1024
	bl  	memset                    @ clear buffer

get_string:	
	mov 	r0, #1
	ldr 	r1, =kscanf_buffer
	mov 	r2, #1024
	bl  	kread
	ldr 	r1, =0xffffffff
	cmp 	r0, r1
	beq 	error_s
	mov 	r3, r0
	mov 	r2, #0
				
check_strbuff_prep:
	mov 	r2, #0
	ldr 	r6, =kscanf_buffer
	ldr 	r1, [r11, #PARAM_CNT] 
	add 	r1, #ARGS
	ldr 	r1, [r11, r1]
                                           @ speichere das Ergebnis über pointer im Ziel      
check_strbuff_loop:
	ldrb	r0, [r6], #1               @ postincrement
	cmp 	r0, #0x20                  @ 0x20 = Leerzeichen, niedrigere Werte sind andere Steuerzeichen
	blo 	error_wrong_input_str
	strb	r0, [r1, r2] 
	cmp 	r3, r2			 
	beq 	str_proc_end
	add 	r2, r2, #1
	b   	check_strbuff_loop

str_proc_end:
	b   	format_id_end
```

Die Funktion `sc_is_s` verarbeitet eine Zeichenkette und speichert sie in den zugehörigen Speicherbereich. Zunächst wird der Puffer geleert, um Platz für die neue Eingabe zu schaffen. Danach liest die Funktion bis zu 1024 Zeichen mithilfe von `kread` in den Puffer ein. Wird ein Fehler erkannt, wird zur Fehlerbehandlung gesprungen.

Anschließend werden die eingelesenen Zeichen auf Leer- oder Steuerzeichen überprüft und schrittweise in den Zielspeicher übertragen. Sobald alle Zeichen verarbeitet wurden, wird die Verarbeitung beendet und die Funktion kehrt zur Hauptschleife zurück.

### 14. **Formatbezeichner Verarbeitung Ende**

Nach der Verarbeitung eines Formatbezeichners springt der Code zurück zur Hauptschleife.

```
format_id_end:
	b   	scan_srcstr_loop
```

Am Ende der Verarbeitung eines Formatbezeichners springt die Funktion mit `b scan_srcstr_loop` zurück zur Hauptschleife, um das nächste Zeichen im Formatstring zu prüfen und zu verarbeiten.

### 15. **Fehlerbehandlungen**

Mehrere Fehlerbehandlungsroutinen geben spezifische Fehlercodes als Rückgabewert zurück:

```
error_dez:
	mvn 	r0, #2
	b   	scanf_end

error_wrong_input:
	mvn 	r0, #3 
	b   	scanf_end

error_s:
	mvn     r0, #4 
	b       scanf_end

error_wrong_input_str:
	mvn     r0, #5 
	b       scanf_end
```


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