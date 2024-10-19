# A.2 Basic Blocks implementieren
## 2.3.1 Datenverarbeitung: Die ALU

Die **ALU (Arithmetic Logic Unit)** ist eine zentrale Komponente eines Prozessors, die für die Ausführung von Berechnungen und logischen Operationen zuständig ist. Sie führt arithmetische Befehle wie **Addition** und **Subtraktion** durch, die mathematische Operationen auf den Daten in den Registern der ALU anwenden. Ebenso verarbeitet sie **logische Befehle**, die boolesche Operationen wie **UND**, **ODER** und **XOR** ausführen, um Daten auf Bit-Ebene zu manipulieren.

Zusätzlich führt die ALU **Vergleichsoperationen** aus, die zwei Werte gegenüberstellen, ohne diese direkt zu verändern. Dabei setzt die ALU spezielle **Flags** im Statusregister, die Informationen über das Ergebnis der Operation speichern. Diese Flags sind entscheidend für bedingte Befehle, da sie den Programmablauf beeinflussen, indem sie festlegen, ob bestimmte Aktionen basierend auf dem Ergebnis einer Berechnung oder eines Vergleichs ausgeführt werden. 

Damit übernimmt die ALU neben arithmetischen und logischen Operationen auch eine wichtige Rolle bei der Steuerung des Programmflusses.

|--------------------------------------|-------------------------------|-------------------------|
| [zurück](../Sektionen_Label/label.md)| [Hauptmenü](../ueberblick.md) | [weiter](arithinstr.md) |


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
