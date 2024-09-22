## Übungsaufgabe

Betrachten sie das folgende Codebeispiel. Wie sie sehen können, ist es nicht nur möglich den Kontrollfluss über Sprünge zu verändern, der Programmcounter lässt sich auch direkt manipulieren. 

```asm
.global start
.section .text

start:
some_func:
	mov r2, #7 		   
	sub r0, pc, #12 	
loop:
	ldr r1, [r0]    	
	str r1, [r0, #28] 	
	add r0, r0, #4 		
	subs r2, r2, #1 	
	subne pc, pc, #24
	
	.space 20000
```	
### Hinweis
Um das Verhalten des Codes während der Ausführung zu analysieren, ist es notwendig im Fenster **Settings** alle Häckchen bei **Debugging Checks** zu entfernen.

|-----------------------------|------------------------------------|------------------------------------------------|
|   [zurück](bedingtespr.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](../ctrlstrukturen/ctrlstrcts.md)    |