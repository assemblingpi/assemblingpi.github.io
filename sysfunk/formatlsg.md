# B.3 Implementierung systemnaher Funktionen
## 3.1.3 Systemnahe Funktionen: Lösung zur Implementierung von Zahlendarstellungsfunktionen 

### **Daten-Sektion (`.section .data`)**

Diese Sektion definiert globale Symbole und reserviert Speicherplatz für verschiedene Datenstrukturen, die von den Funktionen im Textabschnitt verwendet werden.

```
.global num_2_dec
.global num2hexascii
.global float2ascii
.section .data

 sign:   
         .byte 0x0
         .balign 4

 num2dec_buffer:
         .space 16, 0x0

 Hex_Lookup:
         .asciz "0123456789ABCDEF"
         .balign 4

 kformat_buffer:	       
         .space 120, 0x0

 Hexres: 
         .ascii "0", "x"

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
   push  {lr}
   push  {r11}
   mov   r11, sp
   vpush {d0-d15}
   vmov  s0, r1
   mov   r2, #0

float_asc:	
   vcmpe.f32      s0, #0
   vmrs           APSR_nzcv, FPSCR
   movmi          r2, #1
   vabs.f32       s0, s0
   vcvt.s32.f32   s1, s0
   vmov           r0, s1
   vcvt.f32.s32   s1, s1
   vsub.f32       s0, s0, s1
   mov            r1, r0
   mov            r3, #0
   bl             num_2_dec
   
fraction:
   add            r1, r1, #1
   mov            r2, #0x2e
   strb           r2, [r0, r1]
   add            r1, r1, #1
   mov            r2, #10
   vmov           s2, r2
   vcvt.f32.s32   s2, s2
   vmov.f32       s3, #1   
   vdiv.f32       s3, s3, s2	
   vmov           s15, s2
   vmul.f32       s14, s15, s2
   vmul.f32       s13, s14, s2
   vmul.f32       s12, s13, s2
   vmul.f32       q3, q3, d0[0]
   vmul.f32       q2, q3, d1[1]
   vcvt.s32.f32   q2, q2
   vcvt.f32.s32   q2, q2
   vmul.f32       q2, q2, d1[0]
   vsub.f32       q1, q3, q2
   vcvt.s32.f32   q2, q1

float_fr_save:	
   mov            r2, #0x30
   vdup.s32       q3, r2
   vadd.s32       q1, q2, q3 
   vmov           r2, s7
   strb           r2, [r0, r1]
   add            r1, r1, #1
   vmov           r2, s6
   strb           r2, [r0, r1]
   add            r1, r1, #1
   vmov           r2, s5
   strb           r2, [r0, r1]
   add            r1, r1, #1
   vmov           r2, s4
   strb           r2, [r0, r1]

endfl2asc:
   vpop  {d0-d15}
   mov   sp, r11
   pop   {r11}
   ldr   r0, =kformat_buffer
   pop   {lr}
   bx    lr

```

**1. Initialisierung und Vorbereitung**

```assembly
   push  {lr}
   push  {r11}
   mov   r11, sp
   vpush {d0-d15}
   vmov  s0, r1
   mov   r2, #0
```

Ja, hier ist die Erklärung in kürzerer und wesentlicherer Form:

**1. Initialisierung und Vorbereitung**

```assembly
   push  {lr}
   push  {r11}
   mov   r11, sp
   vpush {d0-d15}
   vmov  s0, r1
   mov   r2, #0
```

Die Funktion sichert die Rücksprungadresse und den Framepointer auf dem Stack, speichert die Floating-Point-Register und lädt den Eingabewert (`r1`) in das Register `s0`. Das Register `r2` wird auf `0` gesetzt, um das Vorzeichen des Vorkommateils der Zahl für den späteren Aufruf von `num_2_dec` als positiv zu initialisieren.

**2. Bestimmen des Vorzeichens und Absolutwertbildung**

```assembly
float_asc:	
   vcmpe.f32      s0, #0
   vmrs           APSR_nzcv, FPSCR
   movmi          r2, #1
   vabs.f32       s0, s0
```

Die Fließkommazahl in `s0` wird mit Null verglichen, um das Vorzeichen zu ermitteln. Wenn die Zahl negativ ist, wird die Negativ-Flag gesetzt, und `r2` wird auf `1` gesetzt. Anschließend wird der Absolutwert der Zahl berechnet und wieder in `s0` gespeichert, um die weitere Verarbeitung zu vereinfachen.

**3. Trennung in Ganzzahl- und Bruchteil**

```assembly
   vcvt.s32.f32   s1, s0
   vmov           r0, s1
   vcvt.f32.s32   s1, s1
   vsub.f32       s0, s0, s1
```

Der Absolutwert in `s0` wird in eine Ganzzahl konvertiert und in `s1` gespeichert. Dieser ganzzahlige Wert wird in `r0` übertragen. Durch Rückkonvertierung von `s1` in eine Fließkommazahl und anschließende Subtraktion von `s0` wird der Bruchteil ermittelt und in `s0` belassen.

**4. Konvertierung des Ganzzahlteils in ASCII**

```assembly
   mov   r1, r0
   mov   r3, #0
   bl    num_2_dec
```

Der ganzzahlige Wert wird in `r1` kopiert, und `r3` wird auf `0` gesetzt. Die Funktion `num_2_dec` wird aufgerufen, um den Ganzzahlteil in eine ASCII-Zeichenkette zu konvertieren. Das Vorzeichenflag in `r2` wird dabei berücksichtigt, sodass `num_2_dec` ein Minuszeichen voranstellen kann, wenn die Zahl negativ ist.

**5. Einfügen des Dezimalpunkts**

```assembly
fraction:
   add   r1, r1, #1
   mov   r2, #0x2e
   strb  r2, [r0, r1]
   add   r1, r1, #1
```

Nach der Konvertierung des Ganzzahlteils wird der Index `r1` um 1 erhöht, und der ASCII-Wert für den Dezimalpunkt (`.`) wird in den Puffer an der aktuellen Position gespeichert. Der Index wird erneut erhöht, um Platz für die Nachkommastellen zu schaffen.

**6. Vorbereitung zur Berechnung der Nachkommastellen**

```assembly
   mov            r2, #10
   vmov           s2, r2
   vcvt.f32.s32   s2, s2
   vmov.f32       s3, #1   
   vdiv.f32       s3, s3, s2	
```

Der Wert `10` wird in `s2` als Fließkommazahl gespeichert. `s3` wird auf `1.0` gesetzt und durch `s2` geteilt, um `0.1` zu erhalten. Dieser Wert wird als Skalierungsfaktor für die Nachkommastellen verwendet.

**7. Berechnung der Potenzen von 10**

```assembly
   vmov     s15, s2
   vmul.f32 s14, s15, s2
   vmul.f32 s13, s14, s2
   vmul.f32 s12, s13, s2
```

Es werden die Potenzen von 10 berechnet und in den Registern `s12` bis `s15` gespeichert. Diese Werte (10, 100, 1000, 10000) werden genutzt, um die Nachkommastellen entsprechend ihrer Stellenwerte zu skalieren.

**8. Skalierung und Extraktion der Nachkommastellen**

```assembly
   vmul.f32       q3, q3, d0[0]
   vmul.f32       q2, q3, d1[1]
   vcvt.s32.f32   q2, q2
   vcvt.f32.s32   q2, q2
   vmul.f32       q2, q2, d1[0]
   vsub.f32       q1, q3, q2
   vcvt.s32.f32   q2, q1
```

Der Bruchteil in `s0` wird mit den berechneten Potenzen multipliziert, um die Nachkommastellen zu isolieren. Die Ergebnisse werden zwischen Fließkomma- und Ganzzahlformaten konvertiert, um die Ziffern der Nachkommastellen zu erhalten.

**9. Konvertierung der Nachkommastellen in ASCII**

```assembly
float_fr_save:	
   mov      r2, #0x30
   vdup.s32 q3, r2
   vadd.s32 q1, q2, q3 
```

Der ASCII-Wert für '0' (`0x30`) wird in alle Elemente des Vektors `q3` dupliziert. Durch Addition der extrahierten Nachkommastellen in `q2` zu `q3` entstehen die ASCII-Codes der entsprechenden Ziffern.

**10. Speichern der Nachkommastellen im Puffer**

```assembly
   vmov  r2, s7
   strb  r2, [r0, r1]
   add   r1, r1, #1
   vmov  r2, s6
   strb  r2, [r0, r1]
   add   r1, r1, #1
   vmov  r2, s5
   strb  r2, [r0, r1]
   add   r1, r1, #1
   vmov  r2, s4
   strb  r2, [r0, r1]
```

Die ASCII-Zeichen der Nachkommastellen werden nacheinander in den Puffer geschrieben. Nach jedem Schreibvorgang wird der Index `r1` erhöht, um die nächste Position zu adressieren.

**11. Abschluss und Rückgabe**

```assembly
endfl2asc:
   vpop  {d0-d15}
   mov   sp, r11
   pop   {r11}	
   ldr   r0, =kformat_buffer
   pop   {lr}
   bx    lr
```

Am Ende werden die Register vom Stack geladen, `sp` auf `r11` zurückgesetzt, `r11` vom Stack geholt und die Adresse von `kformat_buffer` in `r0` geladen. Schließlich wird `lr` wiederhergestellt und zur Rückkehr gesprungen.

#### **Funktion: `num_2_dec`**

```assembly
num_2_dec:		
   push  {lr}
   push  {r11}
   mov   r11, sp
   and   r2, r2, #0x3f 
   push  {r1, r2}
   bl    clear_buff
     
buff_cleared_dec:				
   pop   {r1, r2}
   push  {r4,r5, r6, r7}
   cmp   r2, #1
   mov   r6, #0
   bne   dec_signed_processed
   mov   r2, #0x2d
   ldr   r3, =sign
   strb  r2, [r3]

dec_signed_processed:
   ldr   r4, =kformat_buffer
   mov   r0, #0 
   mov   r2, #10
   mov   r3, #0
   cmp   r1, #0
   bne   num_2_dec_loop
   add   r0, r1, #0x30
   push  {r0}
   add   r6, #1
   b     num_2_dec_conv_end

num_2_dec_loop:
   mov   r3, r1
   cmp   r3, #0
   beq   num_2_dec_conv_end
   udiv  r1, r1, r2
   umull r0, r5, r1, r2		 
   sub   r0, r3, r0
   add   r0, #0x30
   push  {r0}
   add   r6, #1
   b     num_2_dec_loop

num_2_dec_conv_end:		
   cmp   r6, #0
   beq   print_is_d
   mov   r1, r6 
   mov   r5, r3	

print_is_d:	
   mov   r7, #0
   ldr   r3, =sign
   ldrb  r2, [r3]
   cmp   r2, #0x2d
   bne   print_is_d_loop
   strb  r2, [r4], #1

print_is_d_loop:
   pop   {r0}
   strb  r0, [r4, r7]
   cmp   r7, r6
   add   r7, #1
   bls   print_is_d_loop

num_2_dec_end:	
   add   r1, r5, r1
   ldr   r3, =sign
   ldrb  r2, [r3]
   cmp   r2, #0x2d
   subne r1, #1
   mov   r2, #0
   strb  r2, [r3]
   pop   {r4, r5, r6, r7}
   mov   sp, r11
   pop   {r11}
   ldr   r0, =kformat_buffer
   pop   {lr}
   bx    lr
```

1. **Prolog:**
   ```
   push  {lr}
   push  {r11}
   mov   r11, sp
   and   r2, r2, #0x3f 
   push  {r1, r2}
   bl    clear_buff
   ```
Im Prolog werden die Rücksprungadresse (`lr`) und das Register `r11` auf den Stack gesichert, danach wird der Stackzeiger in `r11` gespeichert. `r2` wird so maskiert, dass nur die unteren 6 Bits erhalten bleiben und dient als Übergabeparameter dazu, der Dezimalzahl gegebenenfalls später ein ASCII-Minuszeichen voranzustellen. Anschließend werden `r1` und `r2` auf den Stack gelegt, bevor die Funktion `clear_buff` zum Leeren des Puffers aufgerufen wird.

2. **Nach dem Aufruf von `clear_buff`:**
   ```
   buff_cleared_dec:				
      pop   {r1, r2}
      push  {r4,r5, r6, r7}
      cmp   r2, #1
      mov   r6, #0
      bne   dec_signed_processed
      mov   r2, #0x2d
      ldr   r3, =sign
      strb  r2, [r3]
   ```
Die Funktion holt zunächst die Register `r1` und `r2` vom Stack zurück und speichert dann die Register `r4`, `r5`, `r6` und `r7` erneut auf dem Stack. Anschließend wird `r2` mit `1` verglichen. Ist `r2` ungleich `1`, wird zu `dec_signed_processed` gesprungen. Ist `r2` jedoch gleich `1`, wird ein Minuszeichen (`0x2d`) in den `sign`-Puffer geschrieben.

3. **Verarbeitung der Zahl:**
   ```
   dec_signed_processed:
      ldr   r4, =kformat_buffer
      mov   r0, #0 
      mov   r2, #10
      mov   r3, #0
      cmp   r1, #0
      bne   num_2_dec_loop
      add   r0, r1, #0x30
      push  {r0}
      add   r6, #1
      b     num_2_dec_conv_end
   ```
In diesem Abschnitt wird die Adresse des `kformat_buffer` in `r4` geladen, während `r0` auf `0` und `r2` auf `10` für die Division gesetzt werden. Anschließend wird `r1` mit `0` verglichen. Ist `r1` nicht gleich `0`, springt der Code zur Schleife `num_2_dec_loop`. Wenn `r1` jedoch gleich `0` ist, wird `0x30` (ASCII-Wert für `'0'`) zu `r1` addiert und auf dem Stack gespeichert.

4. **Schleife zur Ziffernermittlung:**
   ```
   num_2_dec_loop:
      mov      r3, r1
      cmp      r3, #0
      beq      num_2_dec_conv_end
      udiv     r1, r1, r2
      umull    r0, r5, r1, r2		 
      sub      r0, r3, r0
      add      r0, #0x30
      push     {r0}
      add      r6, #1
      b        num_2_dec_loop
   ```
In der Schleife wird der aktuelle Wert von `r1` nach `r3` kopiert und anschließend mit `0` verglichen. Ist `r3` gleich `0`, endet die Schleife. Andernfalls wird `r1` durch `10` geteilt, und das Ergebnis wird wieder in `r1` gespeichert. Danach wird `r1` mit `10` multipliziert, um das Produkt in `r0` zu speichern, und dieses wird von `r3` subtrahiert, um den Rest (die aktuelle Ziffer) zu ermitteln. Der ASCII-Wert der Ziffer wird durch Hinzufügen von `0x30` erzeugt und auf dem Stack gespeichert. Schließlich wird der Zähler `r6` um `1` erhöht, bevor die Schleife wiederholt wird.

5. **Konvertierung und Speichern der Ziffern:**
   ```
   num_2_dec_conv_end:		
      cmp      r6, #0
      beq      print_is_d
      mov      r1, r6 
      mov      r5, r3	

   print_is_d:	
      mov      r7, #0
      ldr      r3, =sign
      ldrb     r2, [r3]
      cmp      r2, #0x2d
      bne      print_is_d_loop
      strb     r2, [r4], #1

   print_is_d_loop:
      pop      {r0}
      strb     r0, [r4, r7]
      cmp      r7, r6
      add      r7, #1
      bls      print_is_d_loop
   ```
Die Konvertierung beginnt mit dem Vergleich von `r6` (Anzahl der Ziffern) mit `0`. Ist `r6` gleich `0`, wird direkt zu `print_is_d` gesprungen. Andernfalls wird `r1` auf `r6` gesetzt und `r5` auf `r3`. Anschließend wird das Vorzeichen aus dem `sign`-Puffer geladen, und falls es ein Minuszeichen (`0x2d`) ist, wird es in den Puffer geschrieben. In der Schleife `print_is_d_loop` werden die Ziffern vom Stack zurückgeholt und nacheinander in den `kformat_buffer` gespeichert. Der Zähler `r7` wird inkrementiert, bis alle Ziffern gespeichert sind.

6. **Epilog:**
   ```
   num_2_dec_end:	
      add      r1, r5, r1
      ldr      r3, =sign
      ldrb     r2, [r3]
      cmp      r2, #0x2d
      subne    r1, #1
      mov      r2, #0
      strb     r2, [r3]
      pop      {r4, r5, r6, r7}
      mov      sp, r11
      pop      {r11}
      ldr      r0, =kformat_buffer
      pop      {lr}
      bx       lr
   ```
Im Epilog wird `r5` zu `r1` addiert, um die Gesamtlänge des erstellten Strings zu berechnen. Danach wird das Vorzeichen überprüft, und falls es ein Minuszeichen ist, wird die Länge entsprechend angepasst. Das Vorzeichen im `sign`-Puffer wird zurückgesetzt. Anschließend werden die gespeicherten Register wiederhergestellt und die Adresse des Puffers `kformat_buffer` in `r0` geladen. Schließlich wird die Rücksprungadresse wiederhergestellt, und die Funktion kehrt zurück.

#### **Funktion: `num2hexascii`**

```assembly
num2hexascii:   
   push     {lr}
   push     {r11}
   mov      r11, sp
   push     {r4, r5,r6}
   push     {r1, r2}
   bl       clear_buff
   bl       clear_buff_rev

buff_cleared_hex:				
   pop      {r1, r2}
   mov      r5, #0
   mov      r3, #0

num_2_ascii_Hex_loop:
   and      r2, r1, #0xf
   ldr      r4, =Hex_Lookup
   ldrb     r4, [r4, r2]
   push     {r4}
   add      r3, r3, #1
   lsr      r1, r1, #4	
   cmp      r1, #0				
   bne      num_2_ascii_Hex_loop
   mov      r5, r3
   ldr      r4, =kformat_buffer_reverse
   bge      hex_save

hex_save:	
   mov      r1, r3
   mov      r3, #0 
   sub      r1, #1

hex_save_loop:
   pop      {r0}
   strb     r0, [r4 ,r3]
   add      r3, #1
   cmp      r3, r1
   ble      hex_save_loop
   ldr      r0, =kformat_buffer_reverse
   ldr      r2, =kformat_buffer
   sub      r5, r5, #1
   mov      r1, r5

hex_reverse:
   push     {r1}
   ldrb     r3, [r0], #1
   strb     r3, [r2, r1]
   subs     r1, r1, #1
   bge      hex_reverse
   pop      {r1}
   mov      r1, r5
   add      r1, r1, #2
   pop      {r4,r5, r6}
   mov      sp, r11
   pop      {r11}
   ldr      r0, =Hexres
   pop      {lr}
   bx       lr
```

**Erklärung:**

Die Funktion `num2hexascii` konvertiert eine gegebene Ganzzahl in ihre hexadezimale ASCII-Darstellung und speichert sie in einem internen Puffer. Der Rückgabewert `r0` zeigt auf den Puffer, und `r1` gibt die Länge des erzeugten Strings an. Die Funktion ermöglicht auch die Angabe einer Feldbreite (`fieldwidth`) für die Ausgabe.

**Schritt-für-Schritt-Erklärung:**

1. **Prolog:**
   ```assembly
   push     {lr}
   push     {r11}
   mov      r11, sp
   push     {r4, r5,r6}
   push     {r1, r2}
   bl       clear_buff
   bl       clear_buff_rev
   ```

Im Prolog werden zunächst die Rücksprungadresse (`lr`) und das Register `r11` auf den Stack gesichert, und der aktuelle Stackzeiger wird in `r11` übertragen. Anschließend werden die Register `r4`, `r5`, `r6`, `r1` und `r2` auf dem Stack gespeichert. Danach werden die Funktionen `clear_buff` und `clear_buff_rev` aufgerufen, um die Puffer zu leeren.

2. **Nach dem Aufruf von `clear_buff` und `clear_buff_rev`:**
   ```assembly
   buff_cleared_hex:				
      pop      {r1, r2}
      mov      r5, #0
      mov      r3, #0			      
   ```

Nach dem Aufruf von `clear_buff` und `clear_buff_rev` werden die Register `r1` und `r2` vom Stack geladen. Anschließend werden `r5` und `r3` auf `0` gesetzt, wobei `r5` als Zähler für die Hex-Ziffern dient und `r3` als Index fungiert.

3. **Schleife zur Ziffernermittlung:**
   ```assembly
      num_2_ascii_Hex_loop:
      and      r2, r1, #0xf
      ldr      r4, =Hex_Lookup
      ldrb     r4, [r4, r2]
      push     {r4}
      add      r3, r3, #1
      lsr      r1, r1, #4	
      cmp      r1, #0				
      bne      num_2_ascii_Hex_loop
      mov      r5, r3
      ldr      r4, =kformat_buffer_reverse
      bge      hex_save
   ```

In der Schleife `num_2_ascii_Hex_loop` werden die unteren 4 Bits von `r1` maskiert, um eine Hexadezimalziffer zu extrahieren. Anschließend wird das entsprechende Zeichen aus der Tabelle `Hex_Lookup` geladen und auf dem Stack gespeichert. Der Zähler `r3` wird erhöht, und `r1` wird um 4 Bits nach rechts verschoben, um die nächste Hex-Ziffer zu verarbeiten. Dieser Vorgang wird wiederholt, bis `r1` null ist. Schließlich wird `r5` auf die Anzahl der extrahierten Ziffern gesetzt, und die Adresse von `kformat_buffer_reverse` wird in `r4` geladen.

4. **Speichern der hexadezimalen Zeichen:**
   ```
   hex_save:	
      mov      r1, r3
      mov      r3, #0 
      sub      r1, #1
   
   hex_save_loop:
      pop      {r0}
      strb     r0, [r4 ,r3]
      add      r3, #1
      cmp      r3, r1
      ble      hex_save_loop
   ```

In dieser Funktion wird die Anzahl der Hex-Ziffern in `r1` gesetzt und `r3` auf `0` initialisiert. Nach einer Anpassung von `r1` beginnt die Schleife `hex_save_loop`, in der die Hex-Ziffern vom Stack zurückgeholt und in den Puffer `kformat_buffer_reverse` geschrieben werden. Der Zähler `r3` wird dabei inkrementiert, bis alle Ziffern gespeichert sind.

5. **Umkehrung der Zeichenfolge:**

```
   ldr      r0, =kformat_buffer_reverse
   ldr      r2, =kformat_buffer
   sub      r5, r5, #1
   mov      r1, r5

hex_reverse:
   push     {r1}
   ldrb     r3, [r0], #1
   strb     r3, [r2, r1]
   subs     r1, r1, #1
   bge      hex_reverse
   pop      {r1}
   mov      r1, r5
   add      r1, r1, #2
```

Die Funktion lädt die Adressen von `kformat_buffer_reverse` und `kformat_buffer` in `r0` bzw. `r2`. `r5` wird um `1` reduziert und `r1` auf diesen Wert gesetzt. In der Schleife `hex_reverse` wird ein Zeichen aus `kformat_buffer_reverse` gelesen und an der entsprechenden Position in `kformat_buffer` gespeichert. Nach jedem Durchlauf wird `r1` dekrementiert, bis alle Zeichen umgekehrt sind. Am Ende wird `r1` vom Stack zurückgeholt und um `2` erhöht.

6. **Epilog:**
   
```
   pop      {r4,r5, r6}
   mov      sp, r11
   pop      {r11}
   ldr      r0, =Hexres
   pop      {lr}
   bx       lr
```

Die Register `r4`, `r5` und `r6` werden wiederhergestellt, der ursprüngliche Stackzeiger wird zurückgesetzt, und die Adresse des Hex-Präfixes `"0x"` wird in `r0` geladen. Schließlich wird die Rücksprungadresse wiederhergestellt, und die Funktion kehrt zurück.

#### **Funktion: `clear_buff`**

```
clear_buff:
   push     {lr}
   ldr      r0, =kformat_buffer
   mov      r1, #0x00
   mov      r2, #120
   bl       memset
   pop      {lr}
   bx       lr
```

Die Funktion `clear_buff` setzt den Inhalt des Puffers `kformat_buffer` auf null. Zunächst wird die Rücksprungadresse gesichert, dann die Adresse des Puffers in `r0` geladen, `r1` auf `0x00` gesetzt und `r2` mit der Puffergröße von 120 Bytes initialisiert. Anschließend wird die `memset`-Funktion aufgerufen, um alle Bytes auf null zu setzen. Zum Abschluss wird die Rücksprungadresse wiederhergestellt, und die Funktion kehrt zurück.

#### **Funktion: `clear_buff_rev`**

```
clear_buff_rev:
   push     {lr}
   ldr      r0, =kformat_buffer_reverse
   mov      r1, #0x00
   mov      r2, #120
   bl       memset
   pop      {lr}
   bx       lr
```

Die Funktion `clear_buff_rev` setzt den gesamten Inhalt des Puffers `kformat_buffer_reverse` auf null. Zunächst wird die Rücksprungadresse gesichert, danach wird die Adresse des Puffers in `r0` geladen, `r1` auf den Wert `0x00` gesetzt und `r2` mit der Puffergröße von 120 Bytes initialisiert. Anschließend wird die `memset`-Funktion aufgerufen, um alle 120 Bytes auf null zu setzen. Abschließend wird die Rücksprungadresse wiederhergestellt, und die Funktion kehrt zurück.

|----------------------------|------------------------------------|-----------------------------|
|   [zurück](format_ue.md)   |   [Hauptmenü](../ueberblick.md)    |   [weiter](canvas_ue.md)    |


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