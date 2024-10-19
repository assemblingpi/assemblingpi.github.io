# A.2 Basic Blocks implementieren
## 2.1.1 Datentransfer in Assembler

Datentransfer in Assembler umfasst mehrere Arten der Datenbewegung innerhalb eines Computersystems. Eine Möglichkeit ist der Transfer von Daten zwischen CPU-Registern, bei dem Informationen direkt innerhalb der CPU von einem Register in ein anderes verschoben werden. 
Eine weitere Form des Datentransfers erfolgt zwischen Registern und dem Hauptspeicher, wobei Daten in ein Register geladen oder aus einem Register in den Speicher zurückgeschrieben werden. 
Zusätzlich gibt es den Datentransfer von direkten Werten, die im Maschinencode eingebettet sind, zu Registern. Diese eingebetteten Werte, die als Konstanten dienen, werden in Register geladen, um in Berechnungen verwendet zu werden. 

Diese verschiedenen Arten von Datentransfers sind grundlegende Vorgänge in der Assemblerprogrammierung, die den Austausch von Daten innerhalb des Systems ermöglichen und dadurch sicherstellen, dass die notwendigen Daten bei Bedarf verfügbar sind.

|----------------------------------|-------------------------------|------------------|
| [zurück](../CPUlator/cpulator.md)| [Hauptmenü](../ueberblick.md) | [weiter](MOV.md) | 


| **2.1 Datentransfer**                                                                     |
|-------------------------------------------------------------------------------------------|
| [2.1.1 Datentransfer in Assembler](datentr.md)                                            |
| [2.1.2 Datentransfer zwischen Registern und Transfer von direkten Werten](MOV.md)         |
| [2.1.3 Immediate-Werte in ARM-Assembler](Immediates.md)                                   |
| [2.1.4 Datentransfer zwischen Register und Speicher](LDR_STR.md)                          |
