# B.1 Einführung
## 1.6.6 Priviligierungslevel: Rolle der MMU bei den Priviligierungslevel des Prozessors

Die Memory Management Unit (MMU) ist entscheidend für die Umsetzung von Privilegierungsleveln. Sie verwaltet die Speicherzugriffe, indem sie virtuelle Adressen in physische Adressen übersetzt und Zugriffsrechte für verschiedene Privilegierungsmodi durchsetzt. Dadurch ermöglicht die MMU die Isolation von Prozessen und sorgt dafür, dass Software auf niedrigeren Privilegierungsleveln keinen unautorisierten Zugriff auf Speicherbereiche hat, die für höhere Level reserviert sind. Dies ist essenziell für die Sicherheit und Stabilität des Systems und die Grundlage von modernen Betriebsystemen, die in Kernel und Userspace getrennt sind.

|-------------------------|------------------------------------|--------------------------------|
|   [zurück](irqpriv.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](../Boot/boot.md)    |


|**1.6 Priviligierungslevel**                                                       |
|-----------------------------------------------------------------------------------|
| [1.6.1 Priviligierungslevel und Prozessormodi](privmodiintro.md)                  |
| [1.6.2 Privilegierungslevel (PL0, PL1, PL2)](privlev.md)                          |
| [1.6.3 Betriebsmodi](betrmod.md)                                                  |
| [1.6.4 Statusregister und Betriebsmodi](cpsrmod.md)                               |
| [1.6.5 Interrupts und Priviligierungslevel](irqpriv.md)                           |
| [1.6.6 Rolle der MMU bei den Priviligierungslevel des Prozessors](mmupriv.md)     |