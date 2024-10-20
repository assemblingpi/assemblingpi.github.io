# B.1 Einführung
## 1.9.4 Interrupts: Der Interruptcontroller

Ein Interruptcontroller ist eine Hardware-Komponente, die sicherstellt, dass ein Computer effizient auf externe Ereignisse reagieren kann. Er sammelt Interrupt-Signale von verschiedenen Quellen, identifiziert die Ursache und priorisiert die Interrupts, sodass wichtigere Ereignisse zuerst behandelt werden. Anschließend leitet er die Interrupts an den Prozessor weiter, der die nötigen Schritte zur Behandlung des Interrupts ausführt. 

|--------------------------|------------------------------------|-----------------------------|
|   [zurück](ihandler.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](raspiints.md)    |


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