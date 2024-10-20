# B.1 Einführung
## 1.9.2 Interrupts: Die Interruptvektortabelle

Die Interruptvektortabelle ist eine spezielle Tabelle im Speicher, die eine Sammlung von Speicheradressen, sogenannten Interruptvektoren, enthält. Diese Vektoren verweisen auf den Beginn von Interrupt-Service-Routinen (ISR), die ausgeführt werden, wenn ein bestimmter Interrupt auftritt. 

Standardmäßig beginnt die Interruptvektortabelle bei ARM-Prozessoren an der Speicheradresse 0, kann jedoch auch an eine andere Position verschoben werden. Die Interruptvektoren in der Tabelle sind für verschiedene Arten von Interrupts verantwortlich, wobei einige der Verarbeitung von softwarebasierten- und andere von hardwarebasierten Interrupts dienen. Wenn ein Interrupt auftritt, springt der Prozessor zur entsprechenden Adresse in der Tabelle, um die zugehörige ISR zu finden, auszuführen und damit den Interrupt zu verarbeiten.

|--------------------------|------------------------------------|----------------------------|
|   [zurück](intintro.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](ihandler.md)    |


|**1.9 Interrupts**                                                             |
|-------------------------------------------------------------------------------|
| [1.9.1 Was sind Interrupts?](intintro.md)                                     |
| [1.9.2 Die Interruptvektortabelle](ivektable.md)                              |
| [1.9.3 Die Interrupt Service Routine/ der Interrupt-Handler](ihandler.md)     |
| [1.9.4 Der Interruptcontroller](ictrl.md)                                     |
| [1.9.5 Interrupts im Raspberry Pi 2B](raspiints.md)                           |
| [1.9.6 Aufbau und Funktion der Vector Table](armvekt.md)                      |
| [1.9.7 Privilegierungslevel und ihre Rolle bei Interrupts](privints.md)       |
| [1.9.8 Implementierung eines IRQ-Handlers](implirq.md)                        |