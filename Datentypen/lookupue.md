# A.4 Datentypen
## 4.3.5 Komplexe Datentypen: Lookup-Tables 
### Übungsaufgaben

Neben der Berechnung der Fakultät (`x!`) gibt es zahlreiche weitere Anwendungsfälle für Lookup-Tabellen in der Programmierung. Im Folgenden werden zwei weitere Aufgabenstellungen präsentiert, bei denen Lookup-Tabellen als effiziente Lösung dienen. Jede Aufgabe wird durch einen entsprechenden ARM-Assembler-Code gelöst und ausführlich erläutert.

#### Aufgabe 1: Berechnung von y = x² + 2x + 3 mittels Lookup-Tabelle

Entwickeln Sie ein Programm, das für einen gegebenen Wert `x` die quadratische Funktion `y = x² + 2x + 3` berechnet. Nutzen Sie dazu eine Lookup-Tabelle, um die Berechnungen zu beschleunigen, indem vorab die Ergebnisse für verschiedene Werte von `x` gespeichert werden.

#### Aufgabe 2: Berechnung von y = 10^x mittels Lookup-Tabelle

Erstellen Sie ein Programm, das für einen gegebenen Wert `x` die Potenz `y = 10^x` berechnet. Verwenden Sie hierfür eine Lookup-Tabelle, um die Potenzen von 10 schnell und effizient abzurufen, ohne die Berechnung zur Laufzeit durchführen zu müssen.

|-----------------------------|------------------------------------|-----------------------------|
|   [zurück](lookuptable.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](lookuplsg.md)    |


| **4.3 Komplexe Datentypen**                                                   |
|-------------------------------------------------------------------------------|
| [4.3.1 Intro](komplexedtypen.md)                                              |
| [4.3.2 Structs (Strukturen)](structs.md)                                      |
| [4.3.3 Arrays in Assembler](arrays.md)                                        |
| [4.3.4 Zugriffsberechnung bei einem eindimensionalen Array](array1dim.md)     |
| [4.3.5 Lookup-Tables](lookuptable.md)                           		        |
| [4.3.6 Mehrdimensionalen Arrays](arraysmultidim.md)                           |