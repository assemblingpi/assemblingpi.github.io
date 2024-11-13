# B.1 Einführung
## 1.9.1 Interrupts: Was sind Interrupts?

Interrupts sind Signale, die den Prozessor darauf hinweisen, dass ein bestimmtes Ereignis eingetreten ist, das sofortige Aufmerksamkeit erfordert.
Dieser Mechanismus ermöglicht, auf externe oder interne Ereignisse zu reagieren, ohne dass der Prozessor ständig aktiv prüfen muss, ob neue Ereignisse aufgetreten sind. 
Wenn der Prozessor jedoch aktiv prüfen muss, ob ein bestimmtes Event (z. B. eine Datenübertragung oder ein Signal) aufgetreten ist, spricht man von Polling. Polling kostet die CPU wertvolle Zeit, da sie aktiv immer und immer wieder abfrägt, ob ein Signal aufgetreten ist.
Bei Interupts hingegen unterbricht der Prozessor nach Auftreten eines Interrupt-Events automatisch seine gegenwärtige Tätigkeit, um eine spezielle Routine auszuführen, die als Interrupt-Service-Routine (ISR) oder Interrupt Handler bezeichnet wird. 

|------------------------------------|------------------------------------|-----------------------------|
|   [zurück](../UART/k_uart_lsg.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](ivektable.md)    |


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
