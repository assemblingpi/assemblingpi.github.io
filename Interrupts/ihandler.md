# B.1 Einführung
## 1.9.3 Interrupts: Die Interrupt Service Routine/ der Interrupt-Handler

Der Interrupt-Handler ist das Programm, das ausgeführt wird, um das Interrupt-Ereignis zu bearbeiten. Sobald ein Interrupt auftritt, springt der Prozessor zur entsprechenden ISR, um das notwendige Ereignis zu verarbeiten, z.B. das Lesen von Daten oder das Zurücksetzen eines Timers.

|---------------------------|------------------------------------|-------------------------|
|   [zurück](ivektable.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](ictrl.md)    |


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