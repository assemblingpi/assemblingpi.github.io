# B.1 Einführung
## 1.2 Wichtige Konzepte der Systemprogrammierung

### Memory-Mapped I/O
Memory-Mapped I/O bedeutet, dass Kontroll- und Statusregister über Speicheradressen angesprochen werden. Durch das Schreiben in oder Lesen aus diesen Adressen kann Software direkt mit Hardwaremodulen interagieren, indem sie beispielsweise einen Timer startet oder den Status eines Interrupts überprüft. Dieser Mechanismus ermöglicht eine direkte und effiziente Steuerung der Hardware durch die Software.

### Kontrollregister
Kontrollregister sind spezielle Bereiche im Speicher, die zur Steuerung von Hardware-Komponenten dienen. Sie ermöglichen es, bestimmte Funktionen zu aktivieren oder zu deaktivieren, etwa das Starten eines Timers oder das Konfigurieren von Interrupts. Diese Register werden von der Software gesetzt, um die Hardware entsprechend zu steuern.

### Statusregister 
Statusregister zeigen den aktuellen Zustand von Hardware-Komponenten an. Sie informieren die Software darüber, was gerade in einer Hardwarekomponente passiert, beispielsweise ob ein Interrupt aufgetreten ist oder ob ein Timer abgelaufen ist. Diese Register werden von der Hardware aktualisiert und können von der Software gelesen werden.

### Pending-Register
Pending-Register werden im Zusammenhang mit Interrupt-Controllern verwendet und zeigen an, welche Ereignisse oder Interrupts aktuell auf Bearbeitung warten. Wenn ein Interrupt auftritt, wird ein Bit in diesem Register gesetzt und bleibt aktiv, bis der Interrupt bearbeitet wurde. Diese Register helfen der Software, schnell zu erkennen, welche Ereignisse noch nicht verarbeitet sind.

### Coprozessoren
Coprozessoren sind spezialisierte Prozessoren, die Aufgaben wie mathematische Berechnungen oder Speicherverwaltung übernehmen. Sie werden über spezielle Befehle gesteuert und übernehmen bestimmte Funktionen für die Haupt-CPU.

|--------------------------------------|------------------------------------|------------------------------------------------------|
|   [zurück](../Advanced/advanced.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](../assemblerlinker/wasistasmlinker.md)    |
