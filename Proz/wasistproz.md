## Was ist eine Prozedur?

In der Assemblerprogrammierung sind Prozeduren grundlegende Bauelemente, die die Struktur und Organisation von Programmen erheblich verbessern. Sie ermöglichen die Wiederverwendbarkeit von Code und die effiziente Verwaltung von Daten und Kontrollfluss, wodurch komplexe Aufgaben übersichtlich und modular umgesetzt werden können.

### Prozeduren: Wiederverwendbare Codeblöcke
Eine Prozedur ist eine abgegrenzte Sammlung von Anweisungen, die eine bestimmte Aufgabe oder Funktion erfüllen. Durch das Zusammenfassen von Befehlen unter einem Namen können Prozeduren mehrfach im Programm aufgerufen werden, ohne den Code an mehreren Stellen duplizieren zu müssen. Dies fördert nicht nur die Modularität und Wiederverwendbarkeit des Codes, sondern vereinfacht auch die Wartung und Fehlerbehebung, da Änderungen nur einmal in der Prozedur vorgenommen werden müssen.

Prozeduren mit Rückgabewerten werden oft als Funktionen bezeichnet und ermöglichen es, Ergebnisse von Berechnungen oder Operationen zurückzugeben, die im aufrufenden Kontext weiterverwendet werden können.

### Struktur von Prozeduren: Integration von Basic Blocks
Jede Prozedur besteht aus einem oder mehreren Basic Blöcken, die durch Verzweigungen miteinander verbunden sind. 


### Aufruf und Rückkehr: Steuerung des Programmflusses
Der Programmfluss wird durch Aufruf- und Rückkehrbefehle gesteuert. Wenn eine Prozedur aufgerufen wird, springt der Programmfluss zu dem entsprechenden (ersten) Basic Block der Prozedur. Dies erfolgt durch eine spezielle Anweisung, die den Sprung zu der Prozedur ausführt und gleichzeitig die Rücksprungadresse speichert, sodass der Programmfluss nach Abschluss der Prozedur an der richtigen Stelle fortgesetzt werden kann.

Nach der Ausführung der Prozedur sorgt eine Rückkehranweisung dafür, dass der Programmfluss zur aufrufenden Stelle zurückkehrt. Dies wird durch das Entfernen des aktuellen Stackframes und das Wiederherstellen der vorherigen Rücksprungadresse erreicht.

[weiter](wasiststack.md)