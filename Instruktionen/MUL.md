## MUL
Der MUL-Befehl wird verwendet, um zwei Werte zu multiplizieren und das Ergebnis in ein Register zu speichern.

### Syntax
Die grundlegende Syntax des MUL-Befehls lautet wie folgt:


```
MUL <Zielregister>, <Quellregister1>, <Quellregister2>
```

Hierbei ist ``<Zielregister>`` das Register, in das das Ergebnis der Multiplikation gespeichert wird, ``<Quellregister1>`` und ``<Quellregister2>`` sind die Register, deren Werte multipliziert werden.
Achtung: Bei `MUL` können keine unmittelbaren Werte verwendet werden!

### Anwendungsbeispiele

```
.global _start
_start:
    MOV R0, #3       // Lade den Wert 3 in Register R0
    MOV R1, #7       // Lade den Wert 7 in Register R1
    MUL R2, R0, R1   // Multipliziere R0 und R1, speichere das Ergebnis in R2
    
    // Folgende Beispiele zeigen, wie man MUL nicht anwenden kann
    MUL R3, #3, #4   // Unmittelbare Werte dürfen bei MUL nicht angewendet werden!
    MUL R4, R0, #3
    MUL R5, #2, R1
```

[weiter](MLA.md)