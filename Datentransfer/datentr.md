## Datentransfer in Assembler

Datentransfer in Assembler umfasst mehrere Arten der Datenbewegung innerhalb eines Computersystems. Eine Möglichkeit ist der Transfer von Daten zwischen CPU-Registern, bei dem Informationen direkt innerhalb der CPU von einem Register in ein anderes verschoben werden. 
Eine weitere Form des Datentransfers erfolgt zwischen Registern und dem Hauptspeicher, wobei Daten in ein Register geladen oder aus einem Register in den Speicher zurückgeschrieben werden. 
Zusätzlich gibt es den Datentransfer von direkten Werten, die im Maschinencode eingebettet sind, zu Registern. Diese eingebetteten Werte, die als Konstanten dienen, werden in Register geladen, um in Berechnungen verwendet zu werden. 

Diese verschiedenen Arten von Datentransfers sind grundlegende Vorgänge in der Assemblerprogrammierung, die den Austausch von Daten innerhalb des Systems ermöglichen und dadurch sicherstellen, dass die notwendigen Daten bei Bedarf verfügbar sind.



### Mov: Datentransfer zwischen Registern und Transfer von direkten Werten:
Der `MOV`-Befehl in ARMv7 Assembler wird verwendet, um Daten von einer Quelle in ein Zielregister zu kopieren. Die Quelle kann dabei entweder ein anderes Register oder aber ein Wert sein, der direkt in den Maschinenbefehl eingebettet ist. 
Der `MOV`-Befehl beeinflusst nicht den Speicher, sondern arbeitet nur mit den Registern der CPU.

Die grundlegende Syntax des `MOV`-Befehls lautet wie folgt:
```asm
MOV <Zielregister>, <Quelle>
```
Hierbei ist `<Zielregister>` das Register, in das die Daten kopiert werden sollen, und `<Quelle>` kann ein anderes Register oder ein sofortiger (konstanter) Wert sein.

[weitere Infos zu unmittelbaren Werten](../immediates/immediates.md)


#### Laden eines unmittelbaren Wertes in ein Register

```asm
MOV Rd, #im
```

Hierbei wird der unmittelbare Wert #im in das Register Rd geladen. Das #-Symbol kennzeichnet, dass es sich um einen unmittelbaren (konstanten) Wert handelt. Rd steht stellvertretend für eines der General Purpose Register von R0 bis R10.


#### Kopieren eines Wertes von einem Register in ein anderes Register

```asm
MOV Rd, Rn
```

Hierbei wird der Wert, der sich in `Rn` befindet, nach `Rd` kopiert. 
Sowohl Rd, als auch Rn stehen hier stellvertretend für die General Purpose Register.
Nach der Ausführung dieses Befehls enthalten sowohl `Rd` als auch `Rn` denselben Wert.

#### Anwendungsbeispiel
Betrachten wir nun ein vollständiges Beispiel, das veranschaulicht, wie der `MOV`-Befehl verwendet wird, um Werte zwischen Registern zu kopieren und unmittelbare Werte in Register zu laden:
```asm
.section .text 
.global _start _start: 
MOV R0, #5        @ Lade den Wert 5 in Register R0 
MOV R1, R0        @ Kopiere den Wert von R0 nach R1 
MOV R2, #10       @ Lade den Wert 10 in Register R2
```

### LDR/STR: Datentransfer zwischen Register und Speicher:
Neben dem Datentransfer zwischen Registern kann es notwendig sein, Daten zwischen einem Register und dem Hauptspeicher auszutauschen. Für diese Art des Datentransfers gibt es in ARM-Assembler Load und Store-Befehle. Bei diesen Befehlen (LDR/STR) gibt es unterschiedliche Adressierungsarten

LDR: Load
LDR steht für "Load Register" und wird verwendet, um Daten von einer Adresse im Speicher in ein Register zu laden. Hierbei unterscheidet man zwischen unterschiedlichen Adressierungsarten.

...

STR: Store

...
```

[weiter](../arithlog/arithlogintro.md)