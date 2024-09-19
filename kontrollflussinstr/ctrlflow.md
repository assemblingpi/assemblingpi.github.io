## Kontrollflussinstruktionen

Kontrollflussinstruktionen in ARM-Assembler sind entscheidend für die Steuerung des Programmablaufs, da sie es ermöglichen, den Programmfluss zu ändern, indem sie das Programm zu einer anderen Stelle im Code springen lassen. Solche Instruktionen werden verwendet, um Entscheidungen zu treffen und den Code dynamisch zu steuern.
In ARM heißen diese Instruktionen: **Branch-Instruktionen**. 
Der Name „Branch“ bezieht sich darauf, dass durch derartige Befehle der Programmfluss von seinem "natürlichen", sequentiellen Lauf „abzweigt“  – das heißt, man springt bei der Ausführung zu einem anderen Punkt im Programm, basierend auf bestimmten Bedingungen oder direkt.

### Es gibt zwei Haupttypen von Kontrollflussinstruktionen in ARM:

#### Unbedingte Sprünge:
Unbedingte Sprünge, wie der `B`-Befehl (Branch), ändern den Programmfluss immer zu einer angegebenen Adresse, ohne Rücksicht auf etwaige Bedingungen. Das bedeutet, dass das Programm immer zu der angegebenen Stelle im Code springt, wenn dieser Befehl ausgeführt wird.

**b bl bx pc-direkt manipulieren alle unbedingten sprünge können mit bedingungen versehen werden!**

#### Bedingte Sprünge:
Bedingte Sprünge sind etwas komplexer. Sie führen nur dann einen Sprung aus, wenn eine bestimmte Bedingung erfüllt ist. Diese Bedingung basiert auf Status-Flags, die durch vorherige Operationen von der ALU gesetzt wurden. Zum Beispiel überprüft der `BEQ`-Befehl (Branch if Equal), ob das Ergebnis einer vorherigen Operation gleich Null war, und springt nur dann zu einer neuen Adresse, wenn dies zutrifft. Weitere Beispiele sind etwa `BNE` (Branch if Not Equal) und `BGT` (Branch if Greater Than). Diese bedingten Sprünge ermöglichen es dem Programm, verschiedene Codepfade abhängig von bestimmten Bedingungen oder Ergebnissen zu verfolgen, was eine flexible und dynamische Steuerung des Programms ermöglicht.

[weiter](../ctrlstrukturen/ctrlstrcts.md)