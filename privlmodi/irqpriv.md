# B.1 Einführung
## 1.6.5 Priviligierungslevel: Interrupts und Priviligierungslevel

Interrupts werden in modernen Prozessoren wie auch der ARMv7-Architektur in einem privilegierten Modus ausgeführt, um die Sicherheit und Integrität des Systems zu schützen. So wird verhindert, dass unautorisierte oder fehlerhafte Software den Interrupt-Prozess stören oder missbrauchen kann, was bei Software- und Hardware-Interrupts gleichermaßen wichtig ist.

### Ablauf 
Der Prozessor wechselt bei einem Interrupt automatisch in den jeweiligen Interruptmodus, um sicherzustellen, dass der Interrupt sicher und korrekt verarbeitet wird. Nachdem er den Interrupt Handler ausgeführt hat, kehrt er wieder in den ursprünglichen Modus zurück, um den normalen Betrieb fortzusetzen.

|-------------------------|------------------------------------|---------------------------|
|   [zurück](cpsrmod.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](mmupriv.md)    |


|**1.6 Priviligierungslevel**                                                       |
|-----------------------------------------------------------------------------------|
| [1.6.1 Priviligierungslevel und Prozessormodi](privmodiintro.md)                  |
| [1.6.2 Privilegierungslevel (PL0, PL1, PL2)](privlev.md)                          |
| [1.6.3 Betriebsmodi](betrmod.md)                                                  |
| [1.6.4 Statusregister und Betriebsmodi](cpsrmod.md)                               |
| [1.6.5 Interrupts und Priviligierungslevel](irqpriv.md)                           |
| [1.6.6 Rolle der MMU bei den Priviligierungslevel des Prozessors](mmupriv.md)     |