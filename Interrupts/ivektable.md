# B.1 Einführung
## 1.9.2 Interrupts: Die Interruptvektortabelle

Die Interruptvektortabelle ist eine spezielle Tabelle im Speicher, die eine Sammlung von Speicheradressen, sogenannten Interruptvektoren, enthält. Diese Vektoren verweisen auf den Beginn von Interrupt-Service-Routinen (ISR), die ausgeführt werden, wenn ein bestimmter Interrupt auftritt. 

Standardmäßig beginnt die Interruptvektortabelle bei ARM-Prozessoren an der Speicheradresse 0, kann jedoch auch an eine andere Position verschoben werden. Die Interruptvektoren in der Tabelle sind für verschiedene Arten von Interrupts verantwortlich, wobei einige der Verarbeitung von softwarebasierten- und andere von hardwarebasierten Interrupts dienen. Wenn ein Interrupt auftritt, springt der Prozessor zur entsprechenden Adresse in der Tabelle, um die zugehörige ISR zu finden, auszuführen und damit den Interrupt zu verarbeiten.

|--------------------------|------------------------------------|----------------------------|
|   [zurück](intintro.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](ihandler.md)    |