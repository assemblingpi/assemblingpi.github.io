## SUB
Der SUB-Befehl wird verwendet, um einen Wert von einem anderen zu subtrahieren und das Ergebnis in ein Register zu speichern.

### Syntax
Die grundlegende Syntax des SUB-Befehls lautet wie folgt:

```
SUB <Zielregister>, <Quellregister>, <Quelle>
```
Hierbei ist `<Zielregister>` das Register, in das das Ergebnis der Subtraktion gespeichert wird, <Quellregister> ist das Register, von dem subtrahiert wird, und <Quelle> ist ein Register, dessen Wert subtrahiert wird. `<Quelle>` kann aber genauso wieder ein unmittelarer Wert sein!
Bei SUB gelten die selben Regeln wie bei ADD: Man darf keine unmittelbaren Werte voneinander subtrahieren und ein Registerwert kann nicht von einem unmittelbaren Wert subtrahiert werden! 

### Beispiele
1. **Subtraktion zweier Register**
```
SUB Rd, Rn, Rm
```
In diesem Beispiel wird der Wert von `<Rm>` von dem Wert von `<Rn>` subtrahiert. Das Ergebnis wird in `<Rd>` gespeichert.

2. **Subtraktion eines unmittelbaren Wertes**
```
SUB Rd, Rn, #imm
```
Hier wird der unmittelbare Wert `<#imm>` von dem Wert in `<Rn>` subtrahiert, das Ergebnis wird dann in `<Rd>` gespeichert.

### Anwendungsbeispiele


```
.global _start
_start:
    MOV R0, #10      // Lade den Wert 10 in Register R0
    MOV R1, #4       // Lade den Wert 4 in Register R1
    SUB R2, R0, R1   // Subtrahiere R1 von R0, speichere das Ergebnis in R2
    SUB R3, R0, #5   // Subtrahiere den Wert 5 von R0, speichere das Ergebnis in R3
    
    // Folgende Beispiele zeigen, wie man SUB nicht anwenden kann:
    
    SUB R4, #8, #3   // Das Subtrahieren von zwei unmittelbaren Werten ist nicht erlaubt
    SUB R5, #9, R1   // Ein Registerwert kann nicht auf einen unmittelbaren Wert subtrahiert werden
```
[weiter](MUL.md)