## Priviligierungslevel und Prozessormodi
Moderne Prozessoren können in verschiedenen Betriebsmodi arbeiten, die unterschiedliche Privilegierungslevel haben. Die Betriebsmodi eines Prozessors bestimmen, in welchem Zustand er sich befindet und welche Art von Code er ausführt. Jeder Modus ist für eine bestimmte Art von Aufgaben ausgelegt und bietet Zugang zu unterschiedlichen Ressourcen und Registersets. Priviligierungslevel sind Sicherheitsstufen eines Prozessors, die bestimmen, welche Zugriffsrechte eine Software auf Systemressourcen hat.

### Privilegierungslevel und Prozessormodi bei ARM-Prozessoren
Auch in der ARMv7-A-Architektur gibt es mehrere Betriebsmodi, die jeweils einem spezifischen Zweck und unterschiedlichen Zugriffsrechten dienen. So läuft beispielsweise Anwendungs-Code im usr-Modus auf dem unprivilegierten Level PL0, während der svc-Modus auf dem privilegierten Level PL1 ausgeführt wird.

[Privilegierungslevel](privlev.md)

[Betriebsmodi](betrmod.md)

[Statusregister und Betriebsmodi](cpsrmod.md)

[Interrupts und Priviligierungslevel](irqpriv.md)

[Rolle der MMU bei den Priviligierungslevel des Prozessors](mmupriv.md)


### Hinweis
Im Rahmen dieses Tutorials werden wir Code (Interrupts ausgenommen) im Supervisormode ausführen, auch die MMU werden wir nicht konfigurieren, da dies den Rahmen des Tutorials sprengen würde.



