# B.1 Einführung
## 1.6.5 Priviligierungslevel: Interrupts und Priviligierungslevel

Interrupts werden in modernen Prozessoren wie auch der ARMv7-Architektur in einem privilegierten Modus ausgeführt, um die Sicherheit und Integrität des Systems zu schützen. So wird verhindert, dass unautorisierte oder fehlerhafte Software den Interrupt-Prozess stören oder missbrauchen kann, was bei Software- und Hardware-Interrupts gleichermaßen wichtig ist.

### Ablauf 
Der Prozessor wechselt bei einem Interrupt automatisch in den jeweiligen Interruptmodus, um sicherzustellen, dass der Interrupt sicher und korrekt verarbeitet wird. Nachdem er den Interrupt Handler ausgeführt hat, kehrt er wieder in den ursprünglichen Modus zurück, um den normalen Betrieb fortzusetzen.

|-------------------------|------------------------------------|---------------------------|
|   [zurück](cpsrmod.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](mmupriv.md)    |