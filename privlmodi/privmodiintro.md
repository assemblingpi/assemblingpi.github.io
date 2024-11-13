# B.1 Einführung
## 1.6.1 Priviligierungslevel und Prozessormodi

Moderne Prozessoren können in verschiedenen Betriebsmodi arbeiten, die unterschiedliche Privilegierungslevel haben. Die Betriebsmodi eines Prozessors bestimmen, in welchem Zustand er sich befindet und welche Art von Code er ausführt. Jeder Modus ist für eine bestimmte Art von Aufgaben ausgelegt und bietet Zugang zu unterschiedlichen Ressourcen und Registersets. Priviligierungslevel sind Sicherheitsstufen eines Prozessors, die bestimmen, welche Zugriffsrechte eine Software auf Systemressourcen hat.

### Privilegierungslevel und Prozessormodi bei ARM-Prozessoren
Auch in der ARMv7-A-Architektur gibt es mehrere Betriebsmodi, die jeweils einem spezifischen Zweck und unterschiedlichen Zugriffsrechten dienen. So läuft beispielsweise Anwendungs-Code im usr-Modus auf dem unprivilegierten Level PL0, während der svc-Modus auf dem privilegierten Level PL1 ausgeführt wird.

Als Einstieg zum Thema zum Thema empfiehlt sich folgendes [Video](https://www.youtube.com/watch?v=H4SDPLiUnv4). 

### Hinweis
Im Rahmen dieses Tutorials werden wir Code (Interrupts ausgenommen) im Supervisormode ausführen, auch die MMU werden wir nicht konfigurieren, da dies den Rahmen des Tutorials sprengen würde.


|--------------------------------------|------------------------------------|---------------------------|
|   [zurück](../bcm2836/armfamily.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](privlev.md)    |


|**1.6 Priviligierungslevel**                                                       |
|-----------------------------------------------------------------------------------|
| [1.6.1 Priviligierungslevel und Prozessormodi](privmodiintro.md)                  |
| [1.6.2 Privilegierungslevel (PL0, PL1, PL2)](privlev.md)                          |
| [1.6.3 Betriebsmodi](betrmod.md)                                                  |
| [1.6.4 Statusregister und Betriebsmodi](cpsrmod.md)                               |
| [1.6.5 Interrupts und Priviligierungslevel](irqpriv.md)                           |
| [1.6.6 Rolle der MMU bei den Priviligierungslevel des Prozessors](mmupriv.md)     |
