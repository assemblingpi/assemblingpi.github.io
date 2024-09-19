## MLA (Multiply-Accumulate)

Der MLA-Befehl wird verwendet, um zwei Werte zu multiplizieren und das Ergebnis mit einem dritten Wert zu Addieren. Das resultierende Ergebnis wird in einem Register gespeichert.

### Syntax
```
MLA <Zielregister>, <Quellregister1>, <Quellregister2>, <Additionsregister>
```

Zuerst werden die Werte, die in `<Quellregister1>` und `<Quellregister2>` gespeichert sind, miteinander multipliziert. Anschließend wird das Ergebnis der Multiplikation mit dem Wert aus dem `<Additionsregister>` addiert. Das Endergebnis wird in das `<Zielregister>` gespeichert.
Achtung: Auch bei `MLA` können keine unmittelbaren Werte verwendet werden!


### Anwendungsbeispiele

```
.global _start
_start:
	
    MOV R0, #2   			@ Lade den Wert 2 in Register R0        
    MOV R1, #7 				@ Lade den Wert 7 in Register R1 
	MOV R2, #1				@ Lade den Wert 1 in Register R2 
	MLA R3, R0, R1, R2		@ Multipliziere R0 und R1, addiere R2, speichere das Ergebnis in R3
    						@ R3 = (R0 * R1) + R2 = (2 * 7) + 1 = 15
    
    // Folgende Beispiele zeigen, wie man MUL nicht anwenden kann
    MLA R4, #3, R1, R2      @ Fehler: Unmittelbare Werte dürfen bei MLA nicht verwendet werden!
```

[weiter](UDIV_SDIV.md)