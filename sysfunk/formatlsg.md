## Lösung: Implementierung von Zahlendarstellungsfunktionen 

### **Daten-Sektion (`.section .data`)**

Diese Sektion definiert globale Symbole und reserviert Speicherplatz für verschiedene Datenstrukturen, die von den Funktionen im Textabschnitt verwendet werden.

```
.global num_2_dec
.global num2hexascii
.global float2ascii
.section .data

 sign:                  .byte 0x0
 						.balign 4
 num2dec_buffer:
 						.space 16, 0x0
 

 Hex_Lookup:	       .asciz "0123456789ABCDEF"
                       .balign 4
 kformat_buffer:	       
 					   .space 120, 0x0
 Hexres:               .ascii "0", "x"
 kformat_buffer_reverse:	       
 					   .space 120, 0x0
 					   .balign 4
```

Die globalen Symbole `num_2_dec`, `num2hexascii` und `float2ascii` werden für den Zugriff aus anderen Modulen verfügbar gemacht. Im Datenbereich werden Speicherbereiche für verschiedene Zwecke reserviert: `sign` dient vermutlich zur Speicherung eines Vorzeichens, während der Puffer `num2dec_buffer` für die Umwandlung von Zahlen in Dezimaldarstellungen genutzt wird. Die Zeichenkette `Hex_Lookup` unterstützt die Konvertierung von Zahlen in ihre hexadezimale Form. Zusätzliche Puffer wie `kformat_buffer` und `kformat_buffer_reverse` bieten Speicherplatz für allgemeine Formatierungen und möglicherweise umgekehrte Zeichenfolgen. `Hexres` speichert das Präfix `"0x"` für hexadezimale Ausgaben.

### **Text-Sektion (`.section .text`)**

Diese Sektion enthält die Implementierung der Funktionen zur Umwandlung von Zahlen in ASCII-Darstellungen sowie Hilfsfunktionen zur Pufferbereinigung.

#### **Funktion: `float2ascii`**

```
float2ascii: 	
	push {lr}
	push {r11}
	mov  r11, sp
	vmov s0, r1
float_asc:	
	vcvt.s32.f32 s1, s0
	vmov r0, s1
	cmp r0, #0
	bge float_positive
float_negative:
	ldr r1, =#-1
	vmov s2, r1
	vcvt.f32.s32 s2, s2
	vcvt.f32.s32 s1, s1
	vmul.f32 s1, s1, s2
	vmul.f32 s0, s0, s2
	vsub.f32 s0, s0, s1
	mov r1, #0
	sub r1, r1, r0
	mov r2, #1
	mov r3, #0
	bl num_2_dec
	b fraction
float_positive:
	vcvt.f32.s32 s1, s1
	vsub.f32 s0, s0, s1
	mov r1, r0
	mov r2, #0
	mov r3, #0
	bl num_2_dec
fraction:
	add r1, r1, #1
	mov r2, #0x2e
	strb r2, [r0, r1]
	add r1, r1, #1
	mov r2, #10
	vmov s2, r2
	vcvt.f32.s32 s2, s2
	vmov.f32 s3, #1   
	vdiv.f32 s3, s3, s2	
	vmov s15, s2
	vmul.f32 s14, s15, s2
	vmul.f32 s13, s14, s2
	vmul.f32 s12, s13, s2
	vmul.f32 q3, q3, d0[0]
	vmul.f32 q2, q3, d1[1]
	vcvt.s32.f32 q2, q2
	vcvt.f32.s32 q2, q2
	vmul.f32 q2, q2, d1[0]
	vsub.f32 q1, q3, q2
	vcvt.s32.f32 q2, q1
float_fr_save:	
	mov r2, #0x30
	vdup.s32 q3, r2
	vadd.s32 q1, q2, q3 
	vmov r2, s7
	strb r2, [r0, r1]
	add r1, r1, #1
	vmov r2, s6
	strb r2, [r0, r1]
	add r1, r1, #1
	vmov r2, s5
	strb r2, [r0, r1]
	add r1, r1, #1
	vmov r2, s4
	strb r2, [r0, r1]
	add r1, r1, #1
endfl2asc:
	mov		sp, r11
	pop     {r11}	
	ldr r0, =kformat_buffer
	pop {lr}
	bx  lr
```

**Erklärung:**

Die Funktion `float2ascii` konvertiert eine gegebene Gleitkommazahl in ihre ASCII-Darstellung und speichert sie in einem internen Puffer. Der Rückgabewert `r0` zeigt auf den Puffer, und `r1` gibt die Länge des erzeugten Strings an.

**Schritt-für-Schritt-Erklärung:**

1. **Prolog:**
   ```
   push {lr}
   push {r11}
   mov  r11, sp
   vmov s0, r1
   ```
Im Prolog werden die Rücksprungadresse (`lr`) und das Register `r11` auf den Stack gesichert. Der aktuelle Stackzeiger wird in `r11` gespeichert, um ihn später verwenden zu können. Anschließend wird der Float-Wert aus `r1` in das FPU-Register `s0` übertragen.

2. **Konvertierung des Float-Werts in eine Ganzzahl:**
   ```
   float_asc:	
   vcvt.s32.f32 s1, s0
   vmov r0, s1
   cmp r0, #0
   bge float_positive
   ```
Die Funktion konvertiert den Float-Wert im Register `s0` in eine 32-Bit-Ganzzahl und speichert das Ergebnis in `s1`. Diese Ganzzahl wird anschließend in das Register `r0` übertragen. Schließlich wird `r0` mit 0 verglichen, um festzustellen, ob die Zahl positiv oder negativ ist.

3. **Behandlung negativer Werte:**
   ```
   float_negative:
   ldr r1, =#-1
   vmov s2, r1
   vcvt.f32.s32 s2, s2
   vcvt.f32.s32 s1, s1
   vmul.f32 s1, s1, s2
   vmul.f32 s0, s0, s2
   vsub.f32 s0, s0, s1
   mov r1, #0
   sub r1, r1, r0
   mov r2, #1
   mov r3, #0
   bl num_2_dec
   b fraction
   ```
Zunächst wird der Wert `-1` in `r1` geladen und in das FPU-Register `s2` übertragen. Dann werden `s2` und `s1` in Fließkommazahlen umgewandelt. Die Werte in `s1` und `s0` werden mit `-1` multipliziert, um den Betrag der negativen Zahl zu ermitteln. Anschließend wird der negative Rest berechnet, und die Register werden für den Aufruf der Funktion `num_2_dec` vorbereitet. Schließlich wird zur Verarbeitung des Dezimalteils in den Abschnitt `fraction` gesprungen.

4. **Behandlung positiver Werte:**
   ```
   float_positive:
   vcvt.f32.s32 s1, s1
   vsub.f32 s0, s0, s1
   mov r1, r0
   mov r2, #0
   mov r3, #0
   bl num_2_dec
   ```
In diesem Abschnitt wird `s1` zurück in eine Fließkommazahl konvertiert, und der ganzzahlige Teil wird von `s0` subtrahiert, um den Dezimalteil zu isolieren. Anschließend werden die Register vorbereitet, um die Funktion `num_2_dec` für die Umwandlung des Ganzzahlteils aufzurufen.

5. **Verarbeitung des Dezimalteils:**
   ```assembly
   fraction:
   add r1, r1, #1
   mov r2, #0x2e
   strb r2, [r0, r1]
   add r1, r1, #1
   mov r2, #10
   vmov s2, r2
   vcvt.f32.s32 s2, s2
   vmov.f32 s3, #1   
   vdiv.f32 s3, s3, s2	
   vmov s15, s2
   vmul.f32 s14, s15, s2
   vmul.f32 s13, s14, s2
   vmul.f32 s12, s13, s2
   vmul.f32 q3, q3, d0[0]
   vmul.f32 q2, q3, d1[1]
   vcvt.s32.f32 q2, q2
   vcvt.f32.s32 q2, q2
   vmul.f32 q2, q2, d1[0]
   vsub.f32 q1, q3, q2
   vcvt.s32.f32 q2, q1
   ```

In diesem Abschnitt wird zunächst ein Dezimalpunkt (`0x2e`) in den Puffer eingefügt, gefolgt von der Inkrementierung des Index. Der Wert `10` wird in `r2` gesetzt und in eine Fließkommazahl `s2` umgewandelt. Es folgen eine Reihe von Multiplikationen und Divisionen, um die Dezimalstellen zu berechnen und genau darzustellen. Schließlich werden die berechneten Werte konvertiert und für die Ausgabe vorbereitet.

6. **Speichern der Dezimalstellen:**
   ```assembly
   float_fr_save:	
   mov r2, #0x30
   vdup.s32 q3, r2
   vadd.s32 q1, q2, q3 
   vmov r2, s7
   strb r2, [r0, r1]
   add r1, r1, #1
   vmov r2, s6
   strb r2, [r0, r1]
   add r1, r1, #1
   vmov r2, s5
   strb r2, [r0, r1]
   add r1, r1, #1
   vmov r2, s4
   strb r2, [r0, r1]
   add r1, r1, #1
   ```

In dieser Funktion wird `0x30` zu den berechneten Werten addiert, um die ASCII-Darstellung der Dezimalstellen zu erhalten. Die berechneten ASCII-Zeichen werden dann nacheinander in den Puffer gespeichert, wobei der Index nach jedem gespeicherten Zeichen erhöht wird.

7. **Epilog:**
   ```assembly
   endfl2asc:
   mov		sp, r11
   pop     {r11}	
   ldr r0, =kformat_buffer
   pop {lr}
   bx  lr
   ```

Im Epilog wird der ursprüngliche Stackzeiger wiederhergestellt, indem `r11` in den Stackpointer (`sp`) zurückübertragen wird. Anschließend wird die Adresse des Puffers `kformat_buffer` in `r0` geladen. Schließlich wird die Rücksprungadresse (`lr`) wiederhergestellt, und die Funktion kehrt zum Aufrufer zurück.

#### **Funktion: `num_2_dec`**

```assembly
num_2_dec:		
	push    {lr}
	push 	{r11}
	mov 	r11, sp
	and     r2, r2, #0x3f 
	push 	{r1, r2}
	bl      clear_buff
buff_cleared_dec:				
	pop 	{r1, r2}
	push    {r4,r5, r6, r7}
	cmp     r2, #1
	mov     r6, #0
	bne     dec_signed_processed
	mov     r2, #0x2d
	ldr     r3, =sign
	strb    r2, [r3]

dec_signed_processed:
	ldr     r4, =kformat_buffer
	mov     r0, #0 
	mov     r2, #10
	mov     r3, #0
	cmp     r1, #0
	bne     num_2_dec_loop
	add     r0, r1, #0x30
	push    {r0}
	add     r6, #1
	b 		num_2_dec_conv_end

num_2_dec_loop:
	mov     r3, r1
	cmp     r3, #0
	beq     num_2_dec_conv_end
	udiv    r1, r1, r2
	umull 	r0, r5, r1, r2		 
	sub     r0, r3, r0
	add     r0, #0x30
	push    {r0}
	add     r6, #1
	b		num_2_dec_loop

num_2_dec_conv_end:		
	cmp     r6, #0
	beq     print_is_d
	mov     r1, r6 
	mov     r5, r3	

print_is_d:	
	mov     r7, #0
	ldr     r3, =sign
	ldrb    r2, [r3]
	cmp     r2, #0x2d
	bne    	print_is_d_loop
	strb    r2, [r4], #1

print_is_d_loop:
	pop     {r0}
	strb    r0, [r4, r7]
	cmp     r7, r6
	add     r7, #1
	bls     print_is_d_loop

num_2_dec_end:	
	add     r1, r5, r1
	ldr     r3, =sign
	ldrb    r2, [r3]
	cmp     r2, #0x2d
	subne     r1, #1
	mov     r2, #0
	strb    r2, [r3]
	pop		{r4, r5, r6, r7}
	mov		sp, r11
	pop     {r11}
	ldr     r0, =kformat_buffer
	pop     {lr}
	bx      lr
```
**Erklärung:**

Die Funktion `num_2_dec` konvertiert eine gegebene Ganzzahl in ihre dezimale ASCII-Darstellung und speichert diese in einem internen Puffer. Der Rückgabewert `r0` zeigt auf den Puffer, und `r1` gibt die Länge des erzeugten Strings an. Die Funktion verarbeitet auch negative Zahlen und ermöglicht die Angabe einer Feldbreite (`fieldwidth`).

**Schritt-für-Schritt-Erklärung:**

1. **Prolog:**
   ```
   push    {lr}
   push 	{r11}
   mov 	r11, sp
   and     r2, r2, #0x3f 
   push 	{r1, r2}
   bl      clear_buff
   ```
Im Prolog werden die Rücksprungadresse (`lr`) und das Register `r11` auf den Stack gesichert, und der aktuelle Stackzeiger wird in `r11` übertragen. Anschließend werden die unteren 6 Bits von `r2` maskiert, und `r1` sowie `r2` werden auf dem Stack gespeichert. Schließlich wird die Funktion `clear_buff` aufgerufen, um den Puffer zu leeren.

2. **Nach dem Aufruf von `clear_buff`:**
   ```
   buff_cleared_dec:				
   pop 	{r1, r2}
   push    {r4,r5, r6, r7}
   cmp     r2, #1
   mov     r6, #0
   bne     dec_signed_processed
   mov     r2, #0x2d
   ldr     r3, =sign
   strb    r2, [r3]
   ```
Die Funktion holt zunächst die Register `r1` und `r2` vom Stack zurück und speichert dann die Register `r4`, `r5`, `r6` und `r7` erneut auf dem Stack. Anschließend wird `r2` mit `1` verglichen. Ist `r2` ungleich `1`, wird zu `dec_signed_processed` gesprungen. Ist `r2` jedoch gleich `1`, wird ein Minuszeichen (`0x2d`) in den `sign`-Puffer geschrieben.

3. **Verarbeitung der Zahl:**
   ```
   dec_signed_processed:
   ldr     r4, =kformat_buffer
   mov     r0, #0 
   mov     r2, #10
   mov     r3, #0
   cmp     r1, #0
   bne     num_2_dec_loop
   add     r0, r1, #0x30
   push    {r0}
   add     r6, #1
   b 		num_2_dec_conv_end
   ```
In diesem Abschnitt wird die Adresse des `kformat_buffer` in `r4` geladen, während `r0` auf `0` und `r2` auf `10` für die Division gesetzt werden. Anschließend wird `r1` mit `0` verglichen. Ist `r1` nicht gleich `0`, springt der Code zur Schleife `num_2_dec_loop`. Wenn `r1` jedoch gleich `0` ist, wird `0x30` (ASCII-Wert für `'0'`) zu `r1` addiert und auf dem Stack gespeichert.

4. **Schleife zur Ziffernermittlung:**
   ```
   num_2_dec_loop:
   mov     r3, r1
   cmp     r3, #0
   beq     num_2_dec_conv_end
   udiv    r1, r1, r2
   umull 	r0, r5, r1, r2		 
   sub     r0, r3, r0
   add     r0, #0x30
   push    {r0}
   add     r6, #1
   b		num_2_dec_loop
   ```
In der Schleife wird der aktuelle Wert von `r1` nach `r3` kopiert und anschließend mit `0` verglichen. Ist `r3` gleich `0`, endet die Schleife. Andernfalls wird `r1` durch `10` geteilt, und das Ergebnis wird wieder in `r1` gespeichert. Danach wird `r1` mit `10` multipliziert, um das Produkt in `r0` zu speichern, und dieses wird von `r3` subtrahiert, um den Rest (die aktuelle Ziffer) zu ermitteln. Der ASCII-Wert der Ziffer wird durch Hinzufügen von `0x30` erzeugt und auf dem Stack gespeichert. Schließlich wird der Zähler `r6` um `1` erhöht, bevor die Schleife wiederholt wird.

5. **Konvertierung und Speichern der Ziffern:**
   ```
   num_2_dec_conv_end:		
   cmp     r6, #0
   beq     print_is_d
   mov     r1, r6 
   mov     r5, r3	

   print_is_d:	
   mov     r7, #0
   ldr     r3, =sign
   ldrb    r2, [r3]
   cmp     r2, #0x2d
   bne    	print_is_d_loop
   strb    r2, [r4], #1

   print_is_d_loop:
   pop     {r0}
   strb    r0, [r4, r7]
   cmp     r7, r6
   add     r7, #1
   bls     print_is_d_loop
   ```
Die Konvertierung beginnt mit dem Vergleich von `r6` (Anzahl der Ziffern) mit `0`. Ist `r6` gleich `0`, wird direkt zu `print_is_d` gesprungen. Andernfalls wird `r1` auf `r6` gesetzt und `r5` auf `r3`. Anschließend wird das Vorzeichen aus dem `sign`-Puffer geladen, und falls es ein Minuszeichen (`0x2d`) ist, wird es in den Puffer geschrieben. In der Schleife `print_is_d_loop` werden die Ziffern vom Stack zurückgeholt und nacheinander in den `kformat_buffer` gespeichert. Der Zähler `r7` wird inkrementiert, bis alle Ziffern gespeichert sind.

6. **Epilog:**
   ```
   num_2_dec_end:	
   add     r1, r5, r1
   ldr     r3, =sign
   ldrb    r2, [r3]
   cmp     r2, #0x2d
   subne     r1, #1
   mov     r2, #0
   strb    r2, [r3]
   pop		{r4, r5, r6, r7}
   mov		sp, r11
   pop     {r11}
   ldr     r0, =kformat_buffer
   pop     {lr}
   bx      lr
   ```
Im Epilog wird `r5` zu `r1` addiert, um die Gesamtlänge des erstellten Strings zu berechnen. Danach wird das Vorzeichen überprüft, und falls es ein Minuszeichen ist, wird die Länge entsprechend angepasst. Das Vorzeichen im `sign`-Puffer wird zurückgesetzt. Anschließend werden die gespeicherten Register wiederhergestellt und die Adresse des Puffers `kformat_buffer` in `r0` geladen. Schließlich wird die Rücksprungadresse wiederhergestellt, und die Funktion kehrt zurück.

#### **Funktion: `num2hexascii`**

```assembly
num2hexascii:   
	push 	{lr}
	push 	{r11}
	mov 	r11, sp
	push    {r4, r5,r6}
	push 	{r1, r2}
	bl      clear_buff
	bl      clear_buff_rev
buff_cleared_hex:				
	pop 	{r1, r2}
	mov     r5, #0
	mov     r3, #0			      
num_2_ascii_Hex_loop:
	and     r2, r1, #0xf
	ldr 	r4, =Hex_Lookup
	ldrb	r4, [r4, r2]
	push    {r4}
	add		r3, r3, #1
	lsr		r1, r1, #4	
	cmp		r1, #0				
	bne		num_2_ascii_Hex_loop
	mov     r5, r3
	ldr 	r4, =kformat_buffer_reverse
	bge     hex_save

hex_save:	
	mov		r1, r3
	mov     r3, #0 
	sub     r1, #1
hex_save_loop:
	pop     {r0}
	strb     r0, [r4 ,r3]
	add     r3, #1
	cmp     r3, r1
	ble     hex_save_loop
	ldr     r0, =kformat_buffer_reverse
	ldr     r2, =kformat_buffer
	sub     r5, r5, #1
	mov     r1, r5
hex_reverse:
	push    {r1}
	ldrb    r3, [r0], #1
	strb    r3, [r2, r1]
	subs    r1, r1, #1
	bge     hex_reverse
	pop     {r1}
	mov     r1, r5
	add     r1, r1, #2
	pop     {r4,r5, r6}
	mov		sp, r11
	pop     {r11}
	ldr		r0, =Hexres
	pop     {lr}
	bx      lr
```

**Erklärung:**

Die Funktion `num2hexascii` konvertiert eine gegebene Ganzzahl in ihre hexadezimale ASCII-Darstellung und speichert sie in einem internen Puffer. Der Rückgabewert `r0` zeigt auf den Puffer, und `r1` gibt die Länge des erzeugten Strings an. Die Funktion ermöglicht auch die Angabe einer Feldbreite (`fieldwidth`) für die Ausgabe.

**Schritt-für-Schritt-Erklärung:**

1. **Prolog:**
   ```assembly
   push 	{lr}
   push 	{r11}
   mov 	r11, sp
   push    {r4, r5,r6}
   push 	{r1, r2}
   bl      clear_buff
   bl      clear_buff_rev
   ```

Im Prolog werden zunächst die Rücksprungadresse (`lr`) und das Register `r11` auf den Stack gesichert, und der aktuelle Stackzeiger wird in `r11` übertragen. Anschließend werden die Register `r4`, `r5`, `r6`, `r1` und `r2` auf dem Stack gespeichert. Danach werden die Funktionen `clear_buff` und `clear_buff_rev` aufgerufen, um die Puffer zu leeren.

2. **Nach dem Aufruf von `clear_buff` und `clear_buff_rev`:**
   ```assembly
   buff_cleared_hex:				
   pop 	{r1, r2}
   mov     r5, #0
   mov     r3, #0			      
   ```

Nach dem Aufruf von `clear_buff` und `clear_buff_rev` werden die Register `r1` und `r2` vom Stack geladen. Anschließend werden `r5` und `r3` auf `0` gesetzt, wobei `r5` als Zähler für die Hex-Ziffern dient und `r3` als Index fungiert.

3. **Schleife zur Ziffernermittlung:**
   ```assembly
   num_2_ascii_Hex_loop:
   and     r2, r1, #0xf
   ldr 	r4, =Hex_Lookup
   ldrb	r4, [r4, r2]
   push    {r4}
   add		r3, r3, #1
   lsr		r1, r1, #4	
   cmp		r1, #0				
   bne		num_2_ascii_Hex_loop
   mov     r5, r3
   ldr 	r4, =kformat_buffer_reverse
   bge     hex_save
   ```

In der Schleife `num_2_ascii_Hex_loop` werden die unteren 4 Bits von `r1` maskiert, um eine Hexadezimalziffer zu extrahieren. Anschließend wird das entsprechende Zeichen aus der Tabelle `Hex_Lookup` geladen und auf dem Stack gespeichert. Der Zähler `r3` wird erhöht, und `r1` wird um 4 Bits nach rechts verschoben, um die nächste Hex-Ziffer zu verarbeiten. Dieser Vorgang wird wiederholt, bis `r1` null ist. Schließlich wird `r5` auf die Anzahl der extrahierten Ziffern gesetzt, und die Adresse von `kformat_buffer_reverse` wird in `r4` geladen.

4. **Speichern der hexadezimalen Zeichen:**
   ```
   hex_save:	
   mov		r1, r3
   mov     r3, #0 
   sub     r1, #1
   hex_save_loop:
   pop     {r0}
   strb     r0, [r4 ,r3]
   add     r3, #1
   cmp     r3, r1
   ble     hex_save_loop
   ```

In dieser Funktion wird die Anzahl der Hex-Ziffern in `r1` gesetzt und `r3` auf `0` initialisiert. Nach einer Anpassung von `r1` beginnt die Schleife `hex_save_loop`, in der die Hex-Ziffern vom Stack zurückgeholt und in den Puffer `kformat_buffer_reverse` geschrieben werden. Der Zähler `r3` wird dabei inkrementiert, bis alle Ziffern gespeichert sind.

5. **Umkehrung der Zeichenfolge:**
```
   ldr     r0, =kformat_buffer_reverse
   ldr     r2, =kformat_buffer
   sub     r5, r5, #1
   mov     r1, r5
hex_reverse:
   push    {r1}
   ldrb    r3, [r0], #1
   strb    r3, [r2, r1]
   subs    r1, r1, #1
   bge     hex_reverse
   pop     {r1}
   mov     r1, r5
   add     r1, r1, #2
```

Die Funktion lädt die Adressen von `kformat_buffer_reverse` und `kformat_buffer` in `r0` bzw. `r2`. `r5` wird um `1` reduziert und `r1` auf diesen Wert gesetzt. In der Schleife `hex_reverse` wird ein Zeichen aus `kformat_buffer_reverse` gelesen und an der entsprechenden Position in `kformat_buffer` gespeichert. Nach jedem Durchlauf wird `r1` dekrementiert, bis alle Zeichen umgekehrt sind. Am Ende wird `r1` vom Stack zurückgeholt und um `2` erhöht.

6. **Epilog:**
   ```
   pop     {r4,r5, r6}
   mov		sp, r11
   pop     {r11}
   ldr		r0, =Hexres
   pop     {lr}
   bx      lr
   ```
Die Register `r4`, `r5` und `r6` werden wiederhergestellt, der ursprüngliche Stackzeiger wird zurückgesetzt, und die Adresse des Hex-Präfixes `"0x"` wird in `r0` geladen. Schließlich wird die Rücksprungadresse wiederhergestellt, und die Funktion kehrt zurück.

#### **Funktion: `clear_buff`**

```
clear_buff:
	push    {lr}
	ldr 	r0, =kformat_buffer
	mov     r1, #0x00
	mov		r2, #120
	bl		memset
	pop     {lr}
	bx      lr
```

Die Funktion `clear_buff` setzt den Inhalt des Puffers `kformat_buffer` auf null. Zunächst wird die Rücksprungadresse gesichert, dann die Adresse des Puffers in `r0` geladen, `r1` auf `0x00` gesetzt und `r2` mit der Puffergröße von 120 Bytes initialisiert. Anschließend wird die `memset`-Funktion aufgerufen, um alle Bytes auf null zu setzen. Zum Abschluss wird die Rücksprungadresse wiederhergestellt, und die Funktion kehrt zurück.

---

#### **Funktion: `clear_buff_rev`**

```
clear_buff_rev:
	push    {lr}
	ldr 	r0, =kformat_buffer_reverse
	mov     r1, #0x00
	mov		r2, #120
	bl		memset
	pop     {lr}
	bx      lr
```

Die Funktion `clear_buff_rev` setzt den gesamten Inhalt des Puffers `kformat_buffer_reverse` auf null. Zunächst wird die Rücksprungadresse gesichert, danach wird die Adresse des Puffers in `r0` geladen, `r1` auf den Wert `0x00` gesetzt und `r2` mit der Puffergröße von 120 Bytes initialisiert. Anschließend wird die `memset`-Funktion aufgerufen, um alle 120 Bytes auf null zu setzen. Abschließend wird die Rücksprungadresse wiederhergestellt, und die Funktion kehrt zurück.

|----------------------------|------------------------------------|-----------------------------|
|   [zurück](format_ue.md)   |   [Hauptmenü](../ueberblick.md)    |   [weiter](canvas_ue.md)    |







