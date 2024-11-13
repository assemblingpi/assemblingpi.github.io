# A.3 Verknüpfungen von Basic Blocks
## 3.1.1 Kontrollflussinstruktionen: Intro

Kontrollflussinstruktionen in ARM-Assembler sind entscheidend für die Steuerung des Programmablaufs, da sie es ermöglichen, den Programmfluss zu ändern, indem sie das Programm zu einer anderen Stelle im Code springen lassen. Solche Instruktionen werden verwendet, um Entscheidungen zu treffen und den Code dynamisch zu steuern.
In ARM heißen diese Instruktionen: **Branch-Instruktionen**. 
Der Name „Branch“ bezieht sich darauf, dass durch derartige Befehle der Programmfluss von seinem "natürlichen", sequentiellen Lauf „abzweigt“  – das heißt, man springt bei der Ausführung zu einem anderen Punkt im Programm, basierend auf bestimmten Bedingungen oder direkt.

### Es gibt zwei Haupttypen von Kontrollflussinstruktionen in ARM:

Im ARM-Assembler gibt es **unbedingte Sprünge**, die den Programmfluss unabhängig vom vorherigen Code immer ändern. Daneben gibt es **bedingte Sprünge**, die nur ausgeführt werden, wenn Status-Flags, die durch die Ausführung vorherigen Codes gesetzt wurden, bestimmte Bedingungen erfüllen.

|-------------------------------------------|------------------------------------|---------------------------------|
|   [zurück](../Instruktionen/bedinstr.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](unbedingtespr.md)    |


| **3.1 Kontrollflussinstruktionen**                                    |
|-----------------------------------------------------------------------|
| [3.1.1 Intro](ctrlflow.md)                                            |
| [3.1.2 Unbedingte Sprünge](unbedingtespr.md)                          |
| [3.1.3 Bedingte Sprünge](bedingtespr.md)                              |
| [3.1.4 Manipulation des Programmcounters (Übungsaufgabe)](ctrlue.md)	|
