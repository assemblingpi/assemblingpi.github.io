# B.2 Erweiterungen der CPU-Funktionalität
## 2.1 Koprozessoren: Erweiterung der CPU-Funktionalität

Neben der **CPU (Central Processing Unit)** existieren in modernen Computersystemen spezialisierte Prozessoren, sogenannte **Koprozessoren**, die bestimmte Aufgaben übernehmen, um die Hauptprozessor zu entlasten. 

### Rolle und Funktion von Koprozessoren

**Koprozessoren** sind spezialisierte Einheiten, die für bestimmte Arten von Operationen optimiert sind. Typische Aufgaben, die von Koprozessoren übernommen werden, umfassen:

- **Gleitkommaoperationen:** Für präzisere mathematische Berechnungen, die über einfache Ganzzahloperationen hinausgehen.
- **Grafikberechnungen:** Für die Verarbeitung und Darstellung von grafischen Daten.
- **Signalverarbeitung:** Für die Bearbeitung von Audiosignalen, Bilddaten oder anderen komplexen Datenströmen.
- **Kryptographische Operationen:** Für die sichere Verarbeitung von Daten durch Verschlüsselung und Entschlüsselung.

Durch die Auslagerung dieser spezialisierten Aufgaben an Koprozessoren kann die **CPU** effizienter arbeiten, da sie sich auf allgemeine Aufgaben konzentrieren kann.

### Integration von Koprozessoren in den Programmfluss

Koprozessoren werden durch spezielle Maschinenbefehle angesprochen. Diese Anweisungen ermöglichen es dem Programm, die spezialisierten Funktionen des Koprozessors zu nutzen, um komplexe Operationen effizient auszuführen, ohne die Haupt-CPU zu belasten.

So könnte ein Programm, das umfangreiche mathematische Berechnungen durchführt, Anweisungen enthalten, die diese Berechnungen an den **Gleitkomma-Koprozessor** delegieren. Dadurch kann die **CPU** weiterhin andere Aufgaben übernehmen, während der Koprozessor die rechenintensiven Operationen effizient bearbeitet.

### Systemsteuerung durch Koprozessoren

In der ARM-Architektur beispielsweise gibt es mehrere Koprozessoren, die jeweils nummeriert sind und spezifische Systemfunktionen verwalten. Der **CP15 Coprocessor** ist der Systemsteuerungs-Koprozessor in der ARM-Architektur. Er ist verantwortlich für:

- **Konfiguration von Caches:** Verwaltung und Optimierung der Cache-Speicher, um den Zugriff auf häufig genutzte Daten zu beschleunigen.
- **Steuerung der Memory Management Unit (MMU):** Verwaltung der Speicherzuordnung und des Zugriffs auf verschiedene Speicherbereiche.
- **Sicherheitsfunktionen:** Implementierung von Sicherheitsmechanismen, die den Zugriff auf kritische Systemressourcen kontrollieren.

Der **CP15** ermöglicht es dem Betriebssystem und privilegierten Programmen, die Hardware auf einer niedrigen Ebene zu steuern und zu überwachen. Diese Steuerung ist entscheidend, um die Effizienz und Sicherheit des Systems zu gewährleisten.

#### Zugriff und Sicherheit

Der Zugriff auf den **CP15 Coprocessor** ist auf privilegierte Betriebsmodi beschränkt, wie den **Kernel- oder Supervisor-Modus**. Diese Einschränkung stellt sicher, dass nur vertrauenswürdige Software, die im privilegierten Modus läuft, kritische Systemoperationen durchführen kann, die über den Koprozessor gesteuert werden. Diese Sicherheitsmaßnahme verhindert, dass unautorisierte Programme auf wichtige Systemfunktionen zugreifen und sorgt für eine stabile und sichere Systemumgebung.


#### MCR- und MRC-Instruktionen
In der ARM-Architektur ermöglichen die MCR- und MRC-Instruktionen den Datenaustausch zwischen ARM-Registern und Koprozessor-Registern. Die MCR-Instruktion dient dazu, den Zustand oder die Konfiguration eines Koprozessors zu ändern, indem sie Daten von einem ARM-Register in ein Koprozessor-Register überträgt. Die MRC-Instruktion wird verwendet, um den aktuellen Zustand oder Konfigurationsdaten eines Koprozessors auszulesen und in ein ARM-Register zu laden, was beispielsweise bei der Überprüfung oder Anpassung von Systemfunktionen wichtig ist.

#### Bedeutung und Verwendung

**MCR (Move to Koprozessor Register):** Lädt den Inhalt eines ARM-Registers in ein Koprozessor-Register.

**MRC (Move to ARM Register from Koprozessor):** Liest den Inhalt eines Koprozessor-Registers und speichert ihn in einem ARM-Register.

#### Syntax: 
```
  MCR: `mcr <Koprozessor>, <opcode1>, <Rt>, <CRn>, <CRm>, <opcode2>`
  MRC: `mrc <Koprozessor>, <opcode1>, <Rt>, <CRn>, <CRm>, <opcode2>`
```
**Koprozessor:** Bestimmt den Koprozessor, z.B. `p15` für die Systemsteuerungseinheit.

**opcode1:** Zusätzlicher Opcode zur Spezifizierung der Operation.

**Rt:** Bei MCR das ARM-Register, dessen Wert übertragen wird; bei MRC das ARM-Register, in das der Wert gelesen wird.

**CRn:** Platzhalter für Zielregister im Koprozessor (bei MCR) oder Quellregister im Koprozessor (bei MRC).

**CRm:** Platzhalter für optionales zweites Koprozessor-Register.

**opcode2:** Weiterer Opcode für Steuerinformationen.

|---------------------------------------|------------------------------------|---------------------------------------|
|   [zurück](../Interrupts/implirq.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](../Timer/timerintro.md)    |