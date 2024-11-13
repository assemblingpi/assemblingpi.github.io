# A.2 Basic Blocks implementieren
## 2.3.3 Datenverarbeitung: Logische Instruktionen
### Übungsaufgabe: Maskierung mit logischen Instruktionen

In diesen Aufgaben geht es um die Verwendung von logischen Operationen, um bestimmte Bitmuster zu manipulieren. Hierbei sollen mit den kennengelernten Instruktionen gezielt Bits gesetzt, gelöscht oder invertiert werden. 

### Aufgabenstellung: Manipulation eines Bitmusters mit logischen Operationen
Gegeben ist eine 32-Bit-Zahl `0x12345678`, die in Register R0 geladen werden soll. Es sollen verschiedene Teile dieser Zahl mit logischen Operationen manipuliert werden, um ein neues Bitmuster zu erzeugen.

Achtung: Bei logischen Verknüpfungen mit `#imm`-Werten dürfen nur Werte verwendet werden, die in 8 Bits passen. Für größere Werte müssen diese zunächst in ein Register geladen werden (z.B. mit LDR), um anschließend die logische Verknüpfung zwischen zwei Registern durchzuführen.

1. Extrahiere die oberen 16 Bits der Zahl in R0 (die ersten 4 Hex-Ziffern 0x1234), speichere das Ergebnis als 32-Bit-Zahl in R1.
2. Setze die unteren 16 Bits von R0 auf 0xFFFF, ohne die oberen 16 Bits zu verändern.
3. Invertiere die oberen 8 Bits der Zahl (die ersten beiden Hex-Ziffern 0x12), während die restlichen Bits unverändert bleiben, indem Sie eine geeignete logische Operation nutzen.
4. Erstellen Sie eine 16-Bit-Bitmaske, die durch eine exklusive Veroderung mit den unteren 16 Bits das Ergebnis 0xF750 erzielt.
5. Invertieren Sie das Ergebnis zum Schluss.

Speichern Sie zur Überprüfung die Zwischenergebnisse in verschiedene Register!

|-------------------------------|------------------------------------|----------------------------|
|   [zurück](loginstr.md)       |   [Hauptmenü](../ueberblick.md)    |   [weiter](loguelsg.md)    |


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
