## ADD
Der ADD-Befehl wird verwendet, um zwei Werte zu addieren und das Ergebnis in ein Register zu speichern.

### Syntax
Die grundlegende Syntax des ADD-Befehls lautet wie folgt:

```
ADD <Zielregister>, <Quellregister>, <Quelle>
```
Hierbei ist `<Zielregister>` das Register, in das das Ergebnis der Addition gespeichert wird. Auf den Wert, der im `<Quellregister>` steht, wird ein anderer Wert (`<Quelle>`) addiert, der entweder in einem anderen Register steht oder ein unmittelbarer Wert sein kann.

Achtung: Es ist nicht möglich, zwei unmittelbare Werte direkt miteinander zu addieren!


### Beispiele

1. **Addition zweier Register**
```
ADD Rd, Rn, Rm
```
In diesem Beispiel wird der Wert von `Rn` mit dem Wert von `Rm` addiert, das Ergebnis wird dann in `Rd` gespeichert.

2. **Addition mit einem unmittelbaren Wert**

```
ADD Rd, Rn, #imm
```
Hier wird der Wert von `Rn` mit dem unmittelbaren Wert `#imm` addiert, das Ergebnis wird dann in `Rd` gespeichert.

### Anwendungsbeispiel

```
    MOV R1, #5       @ Lade den Wert 5 in Register R1
    MOV R2, #10      @ Lade den Wert 10 in Register R2
    ADD R0, R1, R2   @ Addiere R1 und R2, speichere das Ergebnis in R0
	ADD R3, R1, #3   @ Addiere den Wert 3 auf R1, speichere das Ergebnis in R3
```

**So sehen nach Ausführung die Register aus:**


![Screenshot of Example Program](./ADD.png)

[weiter](SUB.md)