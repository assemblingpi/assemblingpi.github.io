# A.5 Prozedurale Programmierung
## 5.2.7 Was ist eine Prozedur: Übungsaufgabe 
### Aufgabenstellung: Finde das geheime Passwort!

Du stehst vor der Herausforderung, das **geheime Passwort** eines disassemblierten Programms zu finden. Der Code wurde aus C-Code durch einen Compiler erzeugt und erwartet die Eingabe eines Schlüssels. Wird der richtige Schlüssel eingegeben, erscheint `Success`, andernfalls `Error`. Deine Aufgabe besteht darin, den Code ausschließlich durch statische Analyse – also das Untersuchen des Codes ohne dessen Ausführung – zu entschlüsseln und den korrekten Schlüssel zu finden!

### Deine Aufgaben:
- **Den Code verstehen:** Finde heraus, wie der Code funktioniert. Welche Eingaben werden verarbeitet und nach welchen Kriterien entscheidet das Programm, ob der eingegebene Schlüssel korrekt ist?
- **Das Passwort finden:** Nutze deine Analysefähigkeiten, um den Code zu durchdringen. Achte besonders auf Speicheroperationen und Vergleichsoperationen – hier versteckt sich das Passwort.

### Zusätzliche Hinweise:
#### Konstantenpool: 
Unveränderliche Daten wie Strings oder Schlüssel werden nicht direkt im Code hinterlegt, sondern im sogenannten Konstantenpool gespeichert. Sie werden durch Labels wie `.LCPIx_x` referenziert, welche auf die **Speicherorte** dieser Konstanten **zeigen**. 

#### Externe Funktionen: 
Funktionen wie `printf`, `scanf` `strcmp` und `memcpy` sind zwar nicht direkt im Assemblercode eingebunden, aber sie werden durch die Anweisung "Branch with Link" (bl) referenziert. Das Wissen um die Funktionsweise dieser externen Funktionen hilft dir, den Ablauf des Programms zu verstehen:

- **printf:** Wird verwendet, um Ausgaben auf den Bildschirm zu bringen. Dabei wird ein Zeiger auf den Formatstring in Register r0 übergeben, und mögliche weitere Argumente folgen in den Registern r1 bis r3.
- **scanf:** Wird verwendet, um Eingaben vom Benutzer zu lesen. Ein Zeiger auf den Formatstring wird in Register `r0` übergeben, und die Speicheradressen, in die die eingegebenen Werte geschrieben werden sollen, folgen in den Registern `r1` bis `r3`. Diese Speicheradressen müssen als Zeiger auf die jeweiligen Variablen übergeben werden, da `scanf` die Werte direkt an diesen Speicherorten speichert.
- **memcpy:** Kopiert Daten von einem Quellort (r1) zu einem Zielort (r0), wobei die Anzahl der Bytes in r2 übergeben wird.
- **strcmp:** Vergleicht zwei Strings, die in r0 und r1 übergeben werden. Das Ergebnis wird in r0 zurückgegeben: Null bedeutet, die Strings sind gleich, ein anderer Wert zeigt eine Abweichung.

**Nun liegt es an dir, die Funktionsweise des Codes zu analysieren und das geheime Passwort zu entschlüsseln – möge die Analyse erfolgreich sein!**
```
.extern printf
.extern scanf
.extern memcpy
.extern strcmp


.section .data
.Lstr_scrt:
        .asciz  "top_secret_password"
.L.str:
        .asciz  "Geben Sie den Schluessel ein: "
.L.str.1:
        .asciz  "%s"
.L.str.2:
        .asciz  "Success\n"
.L.str.3:
        .asciz  "Error\n"
.L.str.4:
        .asciz  "KygZc"
.L.str.5:
        .asciz  "Trivial"

.section .text
.LBB0_0:
        push {r11, lr}
        mov r11, sp
        sub sp, sp, #40
        str r0, [r11, #-4]
        ldr r1, .LCPI0_0
        add r0, sp, #16
        mov r2, #20
        str r0, [sp, #8]            
        bl  memcpy
        mov r1, #0
        str r1, [sp, #12]
        ldr r1, [r11, #-4]
        str r0, [sp, #4]            
        mov r0, r1
        ldr r1, [sp, #8]            
        bl strcmp
        cmp r0, #0
        bne .LBB0_2
        b .LBB0_1
.LBB0_1:
        mov r0, #1
        str r0, [sp, #12]
        b .LBB0_3
.LBB0_2:
        mvn r0, #0
        str r0, [sp, #12]
        b .LBB0_3
.LBB0_3:
        mov sp, r11
        pop {r11, lr}
        bx lr
.LCPI0_0:
        .long   .Lstr_scrt
.LBB0_4:
        push {r11, lr}
        mov r11, sp
        sub sp, sp, #16
        str r0, [sp, #8]
        ldr r0, [sp, #8]
        bl .LBB0_0
        mov r0, #10
        strb r0, [sp, #7]
        mov r0, #0
        str r0, [sp]
        b .LBB1_1
.LBB1_1:                                
        ldr r0, [sp]
        ldr r1, .LCPI1_0
        ldrb r0, [r1, r0]
        cmp r0, #0
        beq .LBB1_4
        b .LBB1_2
.LBB1_2:                                
        ldrb r0, [sp, #7]
        ldr r1, [sp]
        ldr r2, .LCPI1_0
        ldrb r3, [r2, r1]
        eor r0, r3, r0
        strb r0, [r2, r1]
        b .LBB1_3
.LBB1_3:                               
        ldr r0, [sp]
        add r0, r0, #1
        str r0, [sp]
        b .LBB1_1
.LBB1_4:
        ldr r0, [sp, #8]
        ldr r1, .LCPI1_0
        bl strcmp
        cmp r0, #0
        bne .LBB1_6
        b .LBB1_5
.LBB1_5:
        mov r0, #0
        str r0, [r11, #-4]
        b .LBB1_7
.LBB1_6:
        mvn r0, #0
        str r0, [r11, #-4]
        b .LBB1_7
.LBB1_7:
        ldr r0, [r11, #-4]
        mov sp, r11
        pop {r11, lr}
        bx lr
.LCPI1_0:
        .long   .L.str.4
main:
        push {r11, lr}
        mov r11, sp
        sub sp, sp, #120
        mov r0, #0
        str r0, [r11, #-4]
        ldr r0, .LCPI2_0
        bl printf
        ldr r1, .LCPI2_1
        add r2, sp, #16
        str r0, [sp, #8]            
        mov r0, r1
        mov r1, r2
        str r2, [sp, #4]            
        bl scanf
        ldr r1, [sp, #4]            
        str r0, [sp]                
        mov r0, r1
        bl .LBB0_4
        str r0, [sp, #12]
        ldr r0, [sp, #12]
        cmp r0, #0
        bne .LBB2_2
        b .LBB2_1
.LBB2_1:
        ldr r0, .LCPI2_3
        bl printf
        b .LBB2_3
.LBB2_2:
        ldr r0, .LCPI2_2
        bl printf
        b   .LBB2_3
.LBB2_3:
        mov r0, #0
        mov sp, r11
        pop {r11, lr}
        bx  lr
.LCPI2_0:
        .long   .L.str
.LCPI2_1:
        .long   .L.str.1
.LCPI2_2:
        .long   .L.str.3
.LCPI2_3:
        .long   .L.str.2
``` 


|-----------------------------|------------------------------------|----------------------------------------|
|   [zurück](param.md)        |   [Hauptmenü](../ueberblick.md)    |     [weiter](disasm_lsg.md)            |


| **5.2 Was Was ist eine Prozedur**                                             |
|-------------------------------------------------------------------------------|
| [5.2.1 Intro](wasistproz.md)                                                  |
| [5.2.2 Der Stack](wasiststack.md)                                             |
| [5.2.3 Was ist der Stackframe einer Prozedur?](wasiststackframe.md)           |
| [5.2.4 Was ist der Calltree?](wasistcalltree.md)                              |
| [5.2.5 Prozeduren, Link Register und Stack](prozlrstack.md)                   |
| [5.2.6 Parameterübergabe und Rückgabewerte](param.md)                         |
| [5.2.7 Finde das geheime Passwort!](disasm_ue.md)                             |