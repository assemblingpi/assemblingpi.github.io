## LDR Pseudo-Instruktion
Wie zuvor erwähnt,  kann man mit der Instruktion `MOV Rd, #im` nur 8-bit Werte in Register speichern. Um größere Werte in einem Register abzulegen, verwendet man die LDR-Instruktion. 
Achtung: Das ist nur ein Pseudo-Befehl. Die eigentliche Anwendung von LDR wird im späteren Kapitel unter **link** erklärt!

### Syntax
```
LDR <Zielregister>, =imm
```
Hierbei ist `<Zielregister>` das Register, in das der konstante Wert `imm` kopiert werden soll. Anders als bei MOV wird hier ein `=` anstelle von `#` verwendet, um eine Konstante zu kennzeichnen.

### Beispiel
```
        LDR R0, =0xaabb			@ Lade 16-Bit-Wert in Register R0
		LDR R1, =0xaabbccdd		@ Lade 32-Bit-Wert in Register R1
		MOV R2, #0xabc			@ Wird nicht funktionieren, weil 12-Bit-Werte nicht mit MOV gespeichert werden können
```