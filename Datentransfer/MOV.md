## Mov: Datentransfer zwischen Registern und Transfer von direkten Werten:
Der `MOV`-Befehl in ARMv7 Assembler wird verwendet, um Daten von einer Quelle in ein Zielregister zu kopieren. Die Quelle kann dabei entweder ein anderes Register oder aber ein Wert sein, der direkt in den Maschinenbefehl eingebettet ist. 
Der `MOV`-Befehl beeinflusst nicht den Speicher, sondern arbeitet nur mit den Registern der CPU.

Die grundlegende Syntax des `MOV`-Befehls lautet wie folgt:
```asm
MOV <Zielregister>, <Quelle>
```
Hierbei ist `<Zielregister>` das Register, in das die Daten kopiert werden sollen, und `<Quelle>` kann ein anderes Register oder ein sofortiger (konstanter) Wert sein.


[weitere Infos zu unmittelbaren Werten](../immediates/immediates.md)


### Laden eines unmittelbaren Wertes in ein Register

```asm
MOV Rd, #im
```

Hierbei wird der unmittelbare Wert #imm in das Register Rd geladen. Das #-Symbol kennzeichnet, dass es sich um einen unmittelbaren (konstanten) Wert handelt. Rd steht stellvertretend für eines der General Purpose Register von R0 bis R10.

Achtung: Somit können nur 8-bit Werte in Register gespeichert werden!


### Kopieren eines Wertes von einem Register in ein anderes Register

```asm
MOV Rd, Rn
```

Hierbei wird der Wert, der sich in `Rn` befindet, nach `Rd` kopiert. 
Sowohl Rd, als auch Rn stehen hier stellvertretend für die General Purpose Register.
Nach der Ausführung dieses Befehls enthalten sowohl `Rd` als auch `Rn` denselben Wert.

### Anwendungsbeispiel
Betrachten wir nun ein vollständiges Beispiel, das veranschaulicht, wie der `MOV`-Befehl verwendet wird, um Werte zwischen Registern zu kopieren und unmittelbare Werte in Register zu laden:
```asm
.section .text 
.global _start _start: 
MOV R0, #5        @ Lade den Wert 5 in Register R0 
MOV R1, R0        @ Kopiere den Wert von R0 nach R1 
MOV R2, #10       @ Lade den Wert 10 in Register R2
```

[weiter](LDR_STR.md)