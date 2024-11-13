# A.4 Datentypen 
## 4.3.4 Komplexe Datentypen: Zugriffsberechnung bei einem eindimensionalen Array

### Lösung zur Übungsaufgabe: Eindimensionale Arrays

### 1. Bubble Sort

```
.data
	.align 4
array: .byte 0x5, 0x12, 0x4, 0xa, 0x2, 0x1, 0x09, 0x00

	.align 4
.text
.global _start
_start:
	
	ldr r0, =array
	bl bubblesort
end: 
	b end

```
Der Start des Programms lädt die Adresse des Arrays in Register r0 und ruft die Funktion bubblesort auf. Nach der Rückkehr aus dieser Funktion wird eine Dauerschleife ausgeführt.

```
bubblesort:
	push {lr,r6}
	mov r6, #0
```

Schleifenzähler wird mit `0` initialisiert.

```
loop_aussen:
	cmp r6, #8
	beq endbubble 
	add r6, r6, #1
	bl sort
	b loop_aussen
endbubble:
	pop {pc, r6}
```	  
Der Zähler wird inkrementiert und der Tauschvorgang (wenn nötig) durchgeführt und die Schleife wiederholt, bis das Ende des Arrays erreicht wurde.	  

```
sort:	   
	mov r1, #0
```
Eine Variable für den Zugriff auf den Array wird initialisiert.

```
loop_innen: 
	push {lr}
	cmp r1, #8
	bhi quit
	ldrb r2, [r0, r1]
	add r1, r1, #1
	ldrb r3, [r0, r1]
	sub r1, r1, #1
	cmp r2, r3
	bls sorted
	bl unsorted      
sorted:	  
	add r1, r1, #1
	bl loop_innen	
quit:
	pop {pc}
```

Wenn die Elemente bereits richtig angeordnet sind , wird der Zähler erhöht und die Schleife wiederholt, andernfalls wird zum Tausch gesprungen. Nach dem Vergleich aller Elemente wird die Schleife beendet.

```
unsorted:
	push {r4, r5} 
	ldrb r4, [r0, r1]
	add r1, r1, #1
	ldrb r5, [r0, r1]
	sub r1, r1, #1	  
	strb r5, [r0, r1]
	add r1, r1, #1
	strb r4, [r0, r1]
	sub r1, r1, #1  
	pop {r4, r5}
	bx lr
```
Umsortieren der falsch angeordneten Elemente.


### 2: Insertion Sort

```
.data
    .align 4
    .word 0x0000
array: .byte 0x5, 0x12, 0x4, 0xa, 0x2, 0x1, 0x09, 0x00
length:
    .byte 0x5
    .align 4
.text
.global _start
_start:

    ldr r1, =array
    ldr r0, =length
    bl insertsort

end: 
    b end
```

Aufruf der `insertsort`-Funktion.

```
insertsort:
	push {r4,r5}
	mov r3, #1
	ldr r1, =array
```
Es wird ein Zähler auf `1` gesetzt, der den Fortschritt durch das Array verfolgt. Der Algorithmus beginnt dadurch mit dem zweiten Element, da das erste Element bereits als sortiert betrachtet wird.
```
loop_o:
	cmp r3, r0
	beq end_loop_o
	ldrb r2, [r1 , r3] 
	sub r4, r3, #1  
```
Die äußere Schleife läuft so lange, bis das Ende des Arrays erreicht ist. Dabei wird für jedes Element die richtige Position im bereits sortierten Teil des Arrays gesucht.

```
loop_i:
	cmp r4, #0
	blt loop_i_end
	ldrb r5, [r1, r4] 
	cmp r5, r2
	ble loop_i_end
	add r4, #1
	strb r5, [r1,r4]
	sub r4, r4, #2
loop_i_end:
	add r4, #1
	strb r2, [r1, r4]
	add r3, #1
	b loop_o
```
Das aktuelle Element wird mit den vorhergehenden verglichen. Wenn ein größeres Element gefunden wird, wird es nach rechts verschoben. Sobald ein kleineres oder gleiches Element gefunden wird, wird das aktuelle Element in die entstandene Lücke eingefügt. Diese innere Schleife wird so lange wiederholt, bis entweder kein Element mehr links vom aktuellen Element übrig ist (d. h. der Zähler kleiner als 0 wird) oder ein kleineres oder gleiches Element gefunden wird, das bereits korrekt einsortiert ist.

```
end_loop_o:
	pop {r4, r5}
	bx lr
```
Wenn die äußere Schleife alle Elemente des Arrays durchlaufen hat, ist das Array sortiert, und die Sortierfunktion wird beendet.

|----------------------------|------------------------------------|-------------------------------|
|   [zurück](array1dueb.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](lookuptable.md)    |


| **4.3 Komplexe Datentypen**                                                   |
|-------------------------------------------------------------------------------|
| [4.3.1 Intro](komplexedtypen.md)                                              |
| [4.3.2 Structs (Strukturen)](structs.md)                                      |
| [4.3.3 Arrays in Assembler](arrays.md)                                        |
| [4.3.4 Zugriffsberechnung bei einem eindimensionalen Array](array1dim.md)     |
| [4.3.5 Lookup-Tables](lookuptable.md)                           				|
| [4.3.6 Mehrdimensionalen Arrays](arraysmultidim.md)                           |

