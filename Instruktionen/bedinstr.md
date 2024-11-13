# A.2 Basic Blocks implementieren
## 2.3.8 Datenverarbeitung: Bedingte Instruktionsausführung

Befehle können so gestaltet werden, dass sie nur dann ausgeführt werden, wenn bestimmte Bedingungen erfüllt sind. Dies ermöglicht, dass Code nur dann ausgeführt wird, wenn die festgelegte Bedingung zutrifft.

```asm
cmp r1, #0          @ Vergleiche r1 mit 0
moveq r2, #1        @ Setze r2 auf 1, wenn r1 gleich 0 ist
movne r2, #0        @ Setze r2 auf 0, wenn r1 ungleich 0 ist
```

|------------------------|------------------------------------|--------------------------------------------------|
|   [zurück](beding.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](../kontrollflussinstr/ctrlflow.md)    |


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