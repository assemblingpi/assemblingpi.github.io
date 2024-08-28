## MOV
Der `MOV`-Befehl in ARMv7 Assembler wird verwendet, um Daten von einer Quelle in ein Zielregister zu kopieren. Es ist einer der grundlegendsten und am häufigsten verwendeten Befehle. Der `MOV`-Befehl beeinflusst nicht den Speicher, sondern arbeitet nur mit den Registern der CPU.

### Syntax

Die grundlegende Syntax des `MOV`-Befehls lautet wie folgt: 
```
MOV <Zielregister>, <Quelle>
```
Hierbei ist `<Zielregister>` das Register, in das die Daten kopiert werden sollen, und `<Quelle>` kann ein anderes Register oder ein sofortiger (konstanter) Wert sein.

### Beispiele
1. **Laden eines unmittelbaren Wertes in ein Register**
```
MOV Rd, #imm
```
Hier wird der unmittelbare Wert `#imm` in das Register `Rd` geladen. Das `#`-Symbol zeigt an, dass es sich um einen unmittelbaren (konstanten) Wert handelt.

2.  **Kopieren eines Wertes von einem Register in ein anderes Register**
```
MOV Rd, Rn
```
In diesem Beispiel wird der Wert, der sich im Register `Rn` befindet, in das Register `Rd` kopiert. Nach der Ausführung dieses Befehls enthalten sowohl `Rd` als auch `Rn` denselben Wert.

### Anwendungsbeispiele

Schauen wir uns ein komplettes Beispiel an, das zeigt, wie der `MOV`-Befehl verwendet wird, um Werte zwischen Registern zu kopieren und unmittelbare Werte zu laden:
```
.section .text 
.global _start _start: 

MOV R0, #5 // Lade den Wert 5 in Register R0 
MOV R1, R0 // Kopiere den Wert von R0 nach R1 
MOV R2, #10 // Lade den Wert 10 in Register R2
```
In diesem Beispiel werden mehrere `MOV`-Befehle verwendet:

-   Der Wert `5` wird in `R0` geladen.
-   Der Wert von `R0` wird in `R1` kopiert.
-   Der Wert `10` wird in `R2` geladen.

Diese Befehle demonstrieren, wie der `MOV`-Befehl genutzt wird, um Daten zwischen Registern zu übertragen und unmittelbare Werte in Register zu laden.