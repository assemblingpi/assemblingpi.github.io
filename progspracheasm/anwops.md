# A.1 Einführung
## 1.4.2 Die Programmiersprache Assembler: Anweisungen und Operanden

Ein Assemblerbefehl folgt einem spezifischen Format, das durch sogenannte Mnemonics (leicht zu merkende Kürzel) dargestellt wird. Mnemonics ermöglichen dem Programmierer, die Operationen des Prozessors auf verständliche Weise zu beschreiben. Statt numerische Maschinenbefehle zu verwenden, die schwer zu merken und zu interpretieren wären, nutzt der Assembler Mnemonics wie etwa MOV für "Move", ADD für "Addieren" oder SUB für "Subtrahieren". Diese Mnemonics repräsentieren die grundlegenden Operationen, die ein Prozessor ausführen kann.

Ein typischer Befehl in ARMv7 hat folgenden Aufbau:
```asm
opcode operand1, operand2, operand3
```

Hierbei steht opcode für den spezifischen Befehl, den der Prozessor ausführen soll. Die operands sind die Daten oder Register, auf die der Befehl angewendet wird. (Nicht alle ARMv7-Befehle benötigen drei Operanden. Manche Befehle verwenden nur einen oder zwei Operanden!)

#### Zum Beispiel:
```asm
ADD R1, R2, #5
```
In diesem Beispiel steht das Mnemonic ADD für den OPCODE, der den Prozessor anweist, die beiden Operanden R2 und #5 (wobei #5 ein unmittelbarer Wert ist) zu addieren und das Ergebnis im Register R1 zu speichern.

Der Maschinencode, der diesem Assemblerbefehl entspricht ist:
```
e2821005
```
Alle ARMv7 Maschinenbefehle sind 32-Bit lang und da ihr Format weniger komplex ist, als das von x86-Prozessoren, kann man mit etwas Übung diesen Maschinencode lesen und interpretieren. Durch die Verwendung von Mnemonics ist Assembler jedoch viel verständlicher als Maschinensprache, da der Programmierer in der Lage ist, komplexe Maschinenoperationen auf einer lesbaren und relativ intuitiven Ebene auszudrücken. Anstatt direkt mit den Binärcodes zu arbeiten, können Entwickler so auf einer symbolischen Ebene arbeiten, die dennoch sehr nah an der Hardware ist und präzise Steuerung über die CPU ermöglicht. 

|--------------------------|-------------------------------|---------------------|
| [zurück](progasmintro.md)| [Hauptmenü](../ueberblick.md) | [weiter](adrmodi.md)| 


| **1.4 Die Programmiersprache Assembler**  	                                            |
|-------------------------------------------------------------------------------------------|
| [1.4.1 Was ist Assembler?](../progspracheasm/progasmintro.md)                             |
| [1.4.2 Anweisungen und Operanden](../progspracheasm/anwops.md)                            |
| [1.4.3 Operanden und Adressierungsarten](../progspracheasm/adrmodi.md)                    |
| [1.4.4 Assembler-Direktiven](../progspracheasm/asmdirekt.md)                              |