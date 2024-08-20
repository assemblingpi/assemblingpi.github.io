## Anweisungen und Operanden

Ein Assemblerbefehl folgt einem spezifischen Format, das als Mnemonics bezeichnet wird. Mnemonics sind leicht zu merkende Abkürzungen, die für Maschinenbefehle stehen und es dem Programmierer ermöglichen, die Operationen des Prozessors auf verständliche Weise zu beschreiben. Statt numerische Maschinenbefehle zu verwenden, die schwer zu merken und zu interpretieren wären, nutzt der Assembler Mnemonics wie etwa MOV für "Move", ADD für "Addieren" oder SUB für "Subtrahieren". Diese Mnemonics repräsentieren die grundlegenden Operationen, die ein Prozessor ausführen kann.

Ein typischer Befehl in ARMv7 hat folgenden Aufbau:
```asm
opcode operand1, operand2, operand3
```

Hierbei steht opcode für den spezifischen Befehl, den der Prozessor ausführen soll. Die operands sind die Daten oder Register, auf die der Befehl angewendet wird. 

#### Zum Beispiel:
```asm
ADD R1, R2, #5
```
In diesem Beispiel steht das Mnemonic ADD für den OPCODE, der den Prozessor anweist, die beiden Operanden R2 und #5 (wobei #5 ein unmittelbarer Wert ist) zu addieren und das Ergebnis im Register R1 zu speichern.

Durch die Verwendung von Mnemonics ist Assembler verständlicher als Maschinensprache, da der Programmierer in der Lage ist, komplexe Maschinenoperationen auf einer lesbaren und relativ intuitiven Ebene auszudrücken. Anstatt direkt mit den Binärcodes zu arbeiten, können Entwickler so auf einer symbolischen Ebene arbeiten, die dennoch sehr nah an der Hardware ist und präzise Steuerung über die CPU ermöglicht. 

[Operanden und Adressierungsarten](adrmodi.md)
