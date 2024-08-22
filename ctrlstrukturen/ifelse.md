##  If-else

#### Pseudocode:
```
if(Condition) then... else...
```
#### Beispiel in ARM-Assembler:
```asm
        MOV r0, #111
        MOV r1, #222

        @ Kontrollstruktur if...then - else...
        CMP r0, r1  @ if(r0 == r1)
        BEQ iftrue
     
else:               @ Wenn Condition == false
        MOV r0, #123
        ...
        B endif
iftrue:              @ Wenn Condition = true
        MOV r0, #0x32
        ...
endif:               @ Ende der Kontrollstruktur
        MOV r0, #00
```

#### Betrachtet man den Controlflow-graph dieser Kontrollstruktur in einem Disassembler, ergibt sich folgendes Bild:

![Ifelse](./ifelse.png)

[weiter](If-then elseif-then.md)
