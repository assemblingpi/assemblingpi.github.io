## Was ist ein Koprozessor?

Ein Koprozessor ist ein Spezialprozessor, der bestimmte Aufgaben übernimmt, um den Hauptprozessor (CPU) zu entlasten. Koprozessoren können Aufgaben wie Gleitkommaoperationen, Grafikberechnungen, Signalverarbeitung oder kryptographische Operationen übernehmen. Dadurch kann der Hauptprozessor effizienter arbeiten, da er sich auf andere Aufgaben konzentrieren kann. 

### Systemcontrol-Koprozessor
In der ARM-Architektur gibt es mehrere Koprozessoren, die nummeriert sind. 
Der p15 Coprocessor, auch als CP15 bekannt, ist der Systemsteuerungs-Koprozessor in der ARM-Architektur. Er verwaltet wichtige Systemfunktionen wie die Konfiguration von Caches, die Steuerung der Memory Management Unit (MMU) und Sicherheitsfunktionen. 
CP15 ermöglicht es dem Betriebssystem und privilegierten Programmen, die Hardware auf einer niedrigen Ebene zu steuern und zu überwachen. Als integraler Bestandteil des ARM-Prozessors ist er unerlässlich für die Systemsteuerung.

Der Zugriff auf CP15 ist auf privilegierte Modi beschränkt, wie den Kernel- oder Supervisor-Modus. Dies ist eine Sicherheitsmaßnahme, um sicherzustellen, dass nur vertrauenswürdige Software, die im privilegierten Modus läuft, kritische Systemoperationen durchführen kann, die über CP15 gesteuert werden​

### MCR- und MRC-Instruktionen
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


