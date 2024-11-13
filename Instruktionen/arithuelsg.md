# A.2 Basic Blocks implementieren
## 2.3.2 Datenverarbeitung: Arithmetische Instruktionen
### Lösung der Übungsaufgabe zu arithmetischen Instruktionen

```
MOV R0, #10			@ Lade a = 10 in R0
MOV R1, #25			@ Lade b = 25 in R1
MOV R2, #5			@ Lade c = 5 in R2
MOV R3, #3			@ Lade d = 3 in R3
MOV R4, #7			@ Lade e = 7 in R4
MOV R5, #2			@ Lade f = 2 in R5

@ Berechne a + b - c
ADD R6, R0, R1		@ R6 = a + b = 10 + 25 = 35
SUB R6, R6, R2		@ R6 = R6 - c = 35 - 5 = 30

@ Berechne d * e
MUL R7, R3, R4		@ R7 = 3 * 7 = 21

@ Subtrahiere (d * e) von (a + b - c) 
SUBS R8, R6, R7		@ R8 = 30 - 21 = 9, setze Carry-Bit

@ Subtrahiere f mit Berücksichtigung des Carry-Flags
SBC R9, R8, R5		@ R9 = R8 - f - (1 - Carry) = 9 - 2 - (1 - C) = 7
```

|-------------------------------|------------------------------------|----------------------------|
|   [zurück](arithue.md)        |   [Hauptmenü](../ueberblick.md)    |   [weiter](loginstr.md)    |


| **2.3 Datenverarbeitung**                                             |
|-----------------------------------------------------------------------|
| [2.3.1 Die ALU](arithlogintro.md)                                     |
| [2.3.2 Arithmetische Instruktionen](arithinstr.md)                    |
| [2.3.3 Logische Instrukionen](loginstr.md)                            |
| [2.3.4 Shift Operationen](shiftinstr.md)                              |
| [2.3.5 Das Statusregister](flags.md)                                  |
| [2.3.6 Vergleichsoperatoren](comp.md)                                 |
| [2.3.7 Bedingungscodes](beding.md)                                    |
| [2.3.8 Bedingte Instruktionsausführung](bedinstr.md)                  |
