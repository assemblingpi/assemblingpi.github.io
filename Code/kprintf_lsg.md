## kprintf - Lösungsvorschlag

Die vorliegende `kprintf`-Funktion ist eine Implementierung eines formatierenden Ausgabemechanismus in ARM-Assembly, der ähnlich wie die `printf`-Funktion in der C-Standardbibliothek arbeitet. 

Das zentrale Prinzip der Funktion besteht darin, einen sogenannten Formatstring zu verarbeiten und die darin enthaltenen Platzhalter durch entsprechende Werte zu ersetzen.

### Ablauf der Funktion:
Die Funktion beginnt damit, den übergebenen **Formatstring** Zeichen für Zeichen zu durchlaufen. Normale Zeichen, die keine speziellen Anweisungen enthalten, werden direkt in einen Ausgabepuffer geschrieben, der dazu dient, die endgültige Zeichenkette aufzubauen. Sobald die Funktion auf ein `%`-Zeichen trifft, erkennt sie einen **Formatierungsspezifikator**. Dieser zeigt an, dass ein Platzhalter für ein Argument vorhanden ist. Die Zeichen nach dem `%` bestimmen, welcher Datentyp erwartet wird, z. B. `%d` für eine Dezimalzahl oder `%s` für eine Zeichenkette.

Nachdem der Spezifikator identifiziert wurde, wird das entsprechende Argument, das an dieser Stelle eingefügt werden soll, von einem Speicherort (meist dem Stack) abgerufen. Dieser Prozess wird als **Argumentkonvertierung** bezeichnet, da die Daten (z. B. Zahlen oder Zeichenketten) in ein lesbares Textformat umgewandelt werden müssen. Zum Beispiel wird eine im Binärformat vorliegende Zahl in eine Zeichenkette konvertiert.

Anschließend wird die konvertierte Zeichenkette an der entsprechenden Stelle im Ausgabepuffer eingefügt, sodass der Platzhalter im Formatstring durch den tatsächlichen Wert ersetzt wird. Die Funktion durchläuft den gesamten Formatstring und wiederholt diesen Vorgang, bis alle Zeichen und Platzhalter verarbeitet sind.

Sobald der Formatstring vollständig bearbeitet ist und alle Werte eingesetzt wurden, wird der Inhalt des **Ausgabepuffers** auf das vorgesehene Ausgabemedium, wie etwa eine Konsole oder ein Display, ausgegeben.

### Globale Symbole und Externe Funktionen
Zu Beginn des Codes werden globale Symbole und externe Funktionen deklariert:

```
.global kprintf        @ r1 = formatstring / r2 = OUT_TYPE 	
.extern kwrite
.extern memset
.extern k_uart_write_char
.extern num_2_dec
.extern num2hexascii
.extern str_get_length
```

### Datensegment
Das Datensegment enthält die Definition von Puffer, Konstanten und Zeichenketten, die von kprintf verwendet werden:

```
.section .data
  kprintf_buffer:      .space 1024, 0x0
  Hex_Lookup:	       .asciz "0123456789ABCDEF"
                       .balign 4
  HexAusgabe:	       .ascii "0x"
                       .balign 1
  HexString:           .byte  0,0,0,0,0,0,0,0
  format_id_error_str: .asciz "Umwandlung übersteigt Buffergroesse /n"
  check_error_str:     .asciz "Illegales Umwandlungszeichen /n"
  error_end:           .byte  0x00
```
Zusätzlich werden einige Konstanten definiert, um Längen von Zeichenketten zu berechnen:

```
.equ fid_length,       ( check_error_str - format_id_error_str)
.equ cer_length,       ( error_end - check_error_str)	
                       .balign 4
```
Diese Konstanten (fid_length und cer_length) repräsentieren die Längen der Fehlermeldungen und werden später bei der Ausgabe verwendet.

### Textsegment und Konstantendefinitionen
Im Textsegment werden zunächst Konstanten definiert, die für die Adressierung von Variablen auf dem Stack verwendet werden:
```
.equ BASE,		   		        0x00
.equ ARGS,              BASE +  0x04 

.equ STR_ADR,           BASE -  0x04 
.equ OUT_TYPE,	        BASE -  0x08 
.equ BUFF_CNT,	   		BASE -  0x0C		
.equ PARAM_CNT,         BASE -  0x10  
.equ FIELD_W,           BASE -  0x14   
.equ STACKMAX,          BASE -  FIELD_W 
```
Diese Offsets werden genutzt, um Variablen relativ zum Frame-Pointer `r11` auf dem Stack zu speichern und zu laden.

## Implementierung der kprintf-Funktion
Die Funktion kprintf beginnt mit dem Sichern wichtiger Register und der Einrichtung des Stackrahmens:
```
	push 	{lr}                   
	push 	{r11}
	mov 	r11, sp
	sub 	sp,	sp, #STACKMAX	
```

Anschließend werden die Funktionsparameter und wichtige Variablen initialisiert:
```
	str     r1, [r11, #STR_ADR  ]
	str     r2, [r11, #OUT_TYPE ]
	mov     r0, #0
	str     r0, [r11, #BUFF_CNT ]
	str     r0, [r11, #PARAM_CNT]
	str     r0, [r11, #FIELD_W  ]   
	
```
**Formatstring-Adresse (STR_ADR):** Die Adresse des Formatstrings wird in einer Stackvariablen gespeichert.
**Ausgabetyp (OUT_TYPE):** Speichert, wohin die Ausgabe erfolgen soll (z.B. UART, Screen).
**Pufferzähler (BUFF_CNT):** Hält die aktuelle Position im Ausgabepuffer.
**Parameterzähler (PARAM_CNT):** Zählt die verarbeiteten Argumente.
**Feldbreite (FIELD_W):** Speichert die angegebene Feldbreite für formatierte Ausgaben.


Weitere Arbeitsregister werden gesichert:

```
push    {r4, r5, r6, r7}
```

#### Bereinigung des Ausgabepuffers
Bevor die Verarbeitung beginnt, wird der Ausgabepuffer geleert:

```
clear_buff:
	ldr 	r0, =kprintf_buffer     
	mov     r1, #0x00
	mov		r2, #1024
	bl		memset	
```
Dadurch wird sichergestellt, dass keine alten oder unerwünschten Daten im Puffer verbleiben, die die aktuelle Ausgabe beeinträchtigen könnten.

#### Hauptschleife zur Verarbeitung des Formatstrings
Die Funktion durchläuft den Formatstring Zeichen für Zeichen. Zunächst der Kopf der Schleife:

```
scan_srcstr_loop:				
	ldr     r0, [r11, #STR_ADR]
	ldrb	r1, [r0]   
	cmp		r1, #0                   @ Ende des nullterminierten Strings erreicht?
	beq		kprintf_buf_out
	cmp		r1, #0xD                 @ Enter?
	beq		kprintf_buf_out
	cmp 	r1, #'%'                 @ Umwandlungszeichen?
	bne		buff_str_char
	ldr		r1, [r11, #PARAM_CNT]   
	add		r1, r1, #4                      
	str		r1, [r11, #PARAM_CNT]    
	ldr		r0, [r11, #STR_ADR]
	add     r1, r0, #1
	str		r1, [r11, #STR_ADR]
	ldrb    r1, [r1]                 @ Lade das nächste Zeichen nach %
```
- **Ende des Strings prüfen**: Es wird überprüft, ob das aktuelle Zeichen ein Nullbyte (`0`) ist, was das Ende des nullterminierten Strings signalisiert. Ist dies der Fall, wird die Verarbeitung beendet und der Inhalt des Ausgabepuffers ausgegeben.
- **Zeilenumbruch prüfen**: Wenn das Zeichen ein Carriage Return (`0xD`) ist, wird ebenfalls die Verarbeitung beendet und der Puffer ausgegeben.
- **Formatierungsspezifikator erkennen**: Bei einem `%`-Zeichen beginnt die Verarbeitung eines Formatierungsspezifikators. Der Parameterzähler (`PARAM_CNT`) wird erhöht, um das nächste Argument vom Stack zu laden.
- **Normale Zeichen verarbeiten**: Ist das Zeichen weder das Ende des Strings noch ein `%`, wird es als normales Zeichen behandelt. Es wird direkt in den Ausgabepuffer kopiert, der Pufferzähler (`BUFF_CNT`) wird inkrementiert, und der Stringpointer wird auf das nächste Zeichen gesetzt.

#### Verarbeitung von Formatierungsspezifikatoren
Nach Erkennung eines %-Zeichens wird geprüft, welcher Spezifikator folgt:

```
format_id:                           
check: 
	mov    r3, #1
check_loop:                          @ prüfe ob zeichen nach % eine nummer d.h Fieldwith
	cmp 	r1, #64
	bhi 	checkasc
	cmp     r1, #0x30
	blo     checkerror
	cmp     r1, #0x40
	bhs     checkerror
	push    {r2}
	ldr     r2, [r11, #FIELD_W]
	sub     r1, r1, #0x30
	mov     r0, #10
	push    {r5}
	umull 	r2, r5, r2, r3           @ fieldwith
	umull   r3, r5, r3, r0    
	pop     {r5}
	add     r1, r1, r2
	str     r1, [r11, #FIELD_W]
	pop     {r2}
	ldr		r0, [r11, #STR_ADR]
	add     r1, r0, #1
	str		r1, [r11, #STR_ADR] 
	ldrb    r1, [r1]
	b 		check_loop
```

**Feldbreite prüfen:** Es wird geprüft, ob nach dem % eine Zahl folgt, die die Feldbreite angibt. Wenn diese aus mehreren Zeichen besteht muss die entsprechende Dezimalzahl ermittelt werden.

Eine Sprungtabelle (ascii_jmp_tbl) wird verwendet, um effizient zum entsprechenden Codeabschnitt für den Spezifikator zu springen:
```
checkasc:
	orr 	r1, #32                 @ wandle das Zeichen in `r1` in einen Kleinbuchstaben um
	cmp		r1, #0x7b
	bhs		checkerror
	sub 	r1, r1, #0x61           @ passe den Wert von `r1` so an, dass er beginnend von 0 als Index in die Sprungtabelle dient
	adr		r0, ascii_jmp_tbl
	ldr 	pc, [r0, r1, lsl #2]
	b		.     
```
Sprungtabelle:

```
ascii_jmp_tbl:
    a: .word checkerror
    b: .word checkerror
    c: .word checkerror
    d: .word is_d
    e: .word checkerror
    f: .word is_f
    g: .word checkerror
    h: .word checkerror
    i: .word is_d
    j: .word checkerror
    k: .word checkerror
    l: .word checkerror
    m: .word checkerror
    n: .word checkerror
    o: .word checkerror
    p: .word checkerror
    q: .word checkerror
    r: .word checkerror
    s: .word is_s
    t: .word checkerror
    u: .word is_u
    v: .word checkerror
    w: .word checkerror
    x: .word is_x
    y: .word checkerror
    z: .word checkerror
```

Für jeden unterstützten Spezifikator gibt es einen entsprechenden Codeabschnitt:
- **%d (vorzeichenbehaftete Dezimalzahl):** Wandelt eine Zahl in eine dezimale Zeichenkette um.
- **%u (vorzeichenlose Dezimalzahl):** Wandelt eine vorzeichenlose Zahl in eine dezimale Zeichenkette um.
- **%s (Zeichenkette):** Verarbeitet eine Zeichenkette und ermittelt deren Länge.
- **%x (hexadezimale Zahl):** Wandelt eine Zahl in eine hexadezimale Zeichenkette um.
- **%f (Fließkommazahl):** Wandelt eine Fließkommazahl in eine Zeichenkette um (Aufruf von float2ascii).


Für zulässige Umwandlungszeichen werden die entsprechenden Schritte eingeleitet um die Umwandlung durchzuführen, d.h Funktionen die dies erledigen aufgerufen, bzw bei %s muss nur die Länge des Strings ermittelt werden:

##### Fließkommazahl (%f)


```
is_f:
	ldr 	r2, [r11, #BUFF_CNT]
	mov     r0, #1024
	add		r3, r2, #10
	cmp 	r3, r0
	bhs		format_id_error	
	ldr		r0, [r11, #STR_ADR]
	add     r1, r0, #1
	str		r1, [r11, #STR_ADR] 
	ldr		r1, [r11, #PARAM_CNT]
	add     r1, #ARGS
	ldr     r1, [r11, r1]
float_conv:
	bl      float2ascii
	mov     r4, r1
	b 		print_to_buff
```
- Das Argument wird als Fließkommazahl interpretiert und vom Stack geladen.
- Die Funktion `float2ascii` wird aufgerufen, um die Fließkommazahl in eine ASCII-Zeichenkette umzuwandeln.
- Die Länge der erzeugten Zeichenkette wird in `r4` gespeichert.

##### Dezimalzahlen (%u, %d, %i)

```
@----------------------------------------
is_u:
	ldr 	r2, [r11, #BUFF_CNT]
	mov     r0, #1024
	add		r3, r2, #10
	cmp 	r3, r0
	bhs		format_id_error	
	ldr		r0, [r11, #STR_ADR]
	add     r1, r0, #1
	str		r1, [r11, #STR_ADR] 
	ldr		r1, [r11, #PARAM_CNT] 
	add     r1, #ARGS
	ldr     r1, [r11, r1]
	mov     r2, #0
	b       conv_dec_asc
@----------------------------------------	
is_d:
	ldr 	r2, [r11, #BUFF_CNT]
	mov     r0, #1024
	add		r3, r2, #10
	cmp 	r3, r0
	bhs		format_id_error	
	ldr		r0, [r11, #STR_ADR]
	add     r1, r0, #1
	str		r1, [r11, #STR_ADR] 
	ldr		r1, [r11, #PARAM_CNT] 
	add     r1, #ARGS
	ldr     r1, [r11, r1]
check_minus:		
	mov r0, #0
	ldr r2, =#0x80000000
	and r3, r1, r2
	cmp r3, #0
	moveq r2, #0
	beq conv_dec_asc
	mov r2, #1
	sub r1, r2, r1
	mov r1, #1
conv_dec_asc:
	bl      num_2_dec
	mov     r4, r1
	b 		print_to_buff
```
- Das entsprechende Argument wird vom Stack geladen, basierend auf dem Parameterzähler und einem festen Offset (`ARGS`).
- Bei `%d` wird zusätzlich geprüft, ob die Zahl negativ ist. Ist dies der Fall, wird das Vorzeichen berücksichtigt, und der Absolutwert der Zahl wird berechnet.
- Die Zahl wird dann mittels der Funktion `num_2_dec` in eine dezimale ASCII-Zeichenkette umgewandelt.
- Die Länge der erzeugten Zeichenkette wird in `r4` gespeichert, um sie später beim Schreiben in den Ausgabepuffer zu verwenden.


##### Zeichenkette (%s)

```
is_s:
	ldr		r0, [r11, #STR_ADR]
	add     r1, r0, #1
	str		r1, [r11, #STR_ADR] 
	ldr		r1, [r11, #PARAM_CNT]
	add     r1, #ARGS
	ldr     r1, [r11, r1]
is_s_get:
	bl     str_get_length
	mov    r4, r1
	b 	   print_to_buff
```
- Das Argument wird als Zeiger auf eine nullterminierte Zeichenkette interpretiert und vom Stack geladen.
- Die Länge der Zeichenkette wird mit der Funktion `str_get_length` ermittelt.
- Die Länge wird in `r4` gespeichert.

##### Hexadezimale Zahl (%x)
```
is_x:
	ldr 	r2, [r11, #BUFF_CNT]
	mov     r0, #1024
	add		r3, r2, #10
	cmp 	r3, r0
	bhs		format_id_error	
	ldr		r0, [r11, #STR_ADR]
	add     r1, r0, #1
	str		r1, [r11, #STR_ADR] 
	ldr		r1, [r11, #PARAM_CNT]
	add     r1, #ARGS
	ldr     r1, [r11, r1]
is_x_convert:
	bl      num2hexascii
	mov     r4, r1
```
- Das Argument wird vom Stack geladen.
- Die Funktion `num2hexascii` wird aufgerufen, um die Zahl in eine hexadezimale ASCII-Zeichenkette umzuwandeln.
- Die Länge der erzeugten Zeichenkette wird in `r4` gespeichert.

#### Schreiben in den Ausgabepuffer
Nachdem die konvertierte Zeichenkette vorliegt und ihre Länge bekannt ist, wird sie in den Ausgabepuffer geschrieben:
```
print_to_buff:
	push    {r0}
	ldr 	r2, [r11, #BUFF_CNT]
	mov     r0, #1024
	add		r3, r2, r1        @ r3 = stringlength + buffercount     
	cmp 	r3, r0   
	pop     {r0}
	bhs		format_id_error	
	push    {r4}
	ldr     r4, =kprintf_buffer
print:
	push   {r0-r3}
	add    r5, r4, r2
	add    r2, r1, #1
	mov    r1, r0
	mov    r0, r5
	bl     memcpy 
	pop    {r0-r3}
print_end:
	pop     {r1}	
	add     r1, r1, #1
	add     r2, r2, r1
	ldr     r5, [r11, #FIELD_W]
	subs    r5, r5, r1
	movle   r3, #0
	strle   r3, [r11, #FIELD_W]
	ldr     r0, =kprintf_buffer
print_fill:	
	ble 	print_is_filled
	mov     r1, #0x20
	strb    r1, [r0, r2]
	add     r2, r2, #1
	subs    r5, r5, #1
	b       print_fill
print_is_filled:	     
	str     r2, [r11, #BUFF_CNT]
	ldr     r2, [r11, #FIELD_W]
	mov     r2, #0
	str     r2, [r11, #FIELD_W]

```
- **Pufferüberlauf prüfen:** Bevor die Zeichenkette kopiert wird, wird geprüft, ob genügend Platz im Puffer vorhanden ist.
- **Feldbreite berücksichtigen:** Wenn eine Feldbreite angegeben wurde, wird diese berücksichtigt, indem Leerzeichen hinzugefügt werden. Nach der Verarbeitung wird die Feldbreite auf Null gesetzt, um sicherzustellen, dass sie nicht unbeabsichtigt auf folgende Ausgaben angewendet wird.
- **Aktualisierung der Pufferposition:** Der Pufferzähler BUFF_CNT wird entsprechend erhöht.

Nach dem Schreiben in den Ausgabepuffer kehrt die Funktion zur Hauptschleife zurück, um den nächsten Teil des Formatstrings zu verarbeiten. Dieser Prozess wiederholt sich, bis das Ende des Formatstrings erreicht ist oder ein Zeilenumbruch erkannt wird:
```
format_id_end:	
	b       scan_srcstr_loop
```


#### Verarbeitung von normalen Zeichen
Wenn kein Formatierungsspezifikator vorliegt, werden "normale Zeichen" direkt in den Ausgabepuffer geschrieben:
```
buff_str_char:
	ldr		r3, =kprintf_buffer
	ldr		r2, [r11, #BUFF_CNT] 
	ldr		r0, [r11, #STR_ADR ] 
	ldrb	r1, [r0]              @ lade byte aus string
	strb	r1, [r3, r2]          @ speicher in buffer
	add		r2, r2, #1            @ erhöhe BUFF_CNT
	str		r2, [r11, #BUFF_CNT]  @ bufferposition anpassen
	ldr		r0, [r11, #STR_ADR ]  @ erhöhe stringaddresse
	add		r0, r0, #1
	str		r0, [r11, #STR_ADR ]
	b 		scan_srcstr_loop
```


#### Fehlerbehandlung
Die Funktion enthält Mechanismen zur Fehlerbehandlung:
```
checkerror:
    mov     r0, #2
	ldr     r1, =check_error_str
	ldr     r2, =cer_length
	bl 		kwrite
	b       kprintf_end

format_id_error:
	mov     r0, #2
	ldr     r1, =format_id_error_str
	ldr     r2, =fid_length
	bl 		kwrite
	b       kprintf_end
```
**Ungültige Spezifikatoren:** Wenn ein nicht unterstützter Spezifikator erkannt wird, wird eine Fehlermeldung ausgegeben.
**Pufferüberlauf:** Bei Überschreiten der Puffergröße wird ebenfalls eine Fehlermeldung ausgegeben.

#### Abschluss der Funktion
Nach der Verarbeitung des gesamten Formatstrings wird der Ausgabepuffer ausgegeben:

```
kprintf_buf_out:
	ldr     r0, [r11, #OUT_TYPE]
	ldr     r1, =kprintf_buffer
	ldr     r2, [r11, #BUFF_CNT]
	bl 		kwrite
```
Sobald die gesamte Zeichenkette verarbeitet wurde oder ein Abbruchkriterium erreicht ist, wird die Funktion `kwrite` aufgerufen, um den Inhalt des Ausgabepuffers auszugeben. Die Parameter für `kwrite` sind:

- **`r0`**: Der Ausgabetyp (`OUT_TYPE`), der angibt, wohin die Ausgabe erfolgen soll (z. B. Display, UART).
- **`r1`**: Die Adresse des Ausgabepuffers (`kprintf_buffer`).
- **`r2`**: Die Anzahl der zu schreibenden Bytes (`BUFF_CNT`).

Die Funktion endet mit der Wiederherstellung der Register und dem Rücksprung zum Aufrufer:
```
kprintf_end:
	pop     {r4, r5, r6, r7}
    add     sp, sp, #STACKMAX
	mov		sp, r11
	pop 	{r11}
	pop		{lr}
	bx		lr
	b		.
```


### Das Stackmanagement bei `kprintf`

Die `kprintf`-Funktion nutzt den Stack zur Verwaltung von Parametern und lokalen Variablen. Zu Beginn werden wichtige Register wie `lr` (Rücksprungadresse) und `r11` (Frame-Pointer) auf dem Stack gesichert. Anschließend wird der Frame-Pointer auf den aktuellen Stack-Pointer gesetzt, um festen Zugriff auf lokale Variablen über definierte Offsets zu gewährleisten.

Die ersten beiden Parameter, der Formatstring und der Ausgabetyp, werden in den Registern `r1` und `r2` übergeben. Zusätzliche Argumente, die durch Formatierungsspezifikatoren wie `%d` oder `%s` eingebunden werden sollen, liegen auf dem Stack (bereitgestellt von der aufrufenden Funktion). Um diese Argumente zu verarbeiten, nutzt `kprintf` einen Zähler (`PARAM_CNT`), der die Position des nächsten Arguments auf dem Stack angibt. Mit jedem neuen Spezifikator wird dieser Zähler um 4 Bytes (die Größe eines Words) erhöht, sodass das nächste Argument korrekt geladen werden kann.

Über feste Offsets vom Frame-Pointer greift die Funktion auf lokale Variablen zu, wie etwa die aktuelle Position im Formatstring (`STR_ADR`) oder die Position im Ausgabepuffer (`BUFF_CNT`). Die geladenen Argumente, wie Zahlen oder Zeichenketten, werden bei Bedarf konvertiert und in den Ausgabepuffer geschrieben.

Am Ende der Funktion wird der Stack-Pointer zurückgesetzt, der für lokale Variablen reservierte Speicher freigegeben und gesicherte Register wie `r11` und `lr` wiederhergestellt. Die Rückkehr zum Aufrufer erfolgt mit `bx lr`. Dieses Stack-Management ermöglicht es `kprintf`, flexibel mit variablen Argumenten umzugehen, ohne den Programmfluss zu beeinträchtigen. Den durch den Aufrufer reservierten Stack muss dieser selbst wieder freigeben!