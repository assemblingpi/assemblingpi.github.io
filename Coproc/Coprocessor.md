## Was ist ein Coprozessor?

Ein Coprocessor ist ein Spezialprozessor, der bestimmte Aufgaben übernimmt, um den Hauptprozessor (CPU) zu entlasten. Coprozessoren können Aufgaben wie Gleitkommaoperationen, Grafikberechnungen, Signalverarbeitung oder kryptographische Operationen übernehmen. Dadurch kann der Hauptprozessor effizienter arbeiten, da er sich auf andere Aufgaben konzentrieren kann. 

### Systemcontrol-Coprozessor

In der ARM-Architektur gibt es mehrere Coprozessoren, die nummeriert sind. 
Der p15 Coprozessor**, auch als CP15 bekannt, ist der Systemkontroll-Coprozessor in der ARM-Architektur. Er verwaltet wichtige Systemfunktionen wie die Konfiguration von Caches, die Steuerung der Memory Management Unit (MMU) und Sicherheitsfunktionen. 
CP15 ermöglicht es dem Betriebssystem und privilegierten Programmen, die Hardware auf einer niedrigen Ebene zu steuern und zu überwachen. Als integraler Bestandteil des ARM-Prozessors ist er unerlässlich für die Systemsteuerung.

Der Zugriff auf CP15 ist auf privilegierte Modi beschränkt, wie den Kernel- oder Supervisor-Modus. Dies ist eine Sicherheitsmaßnahme, um sicherzustellen, dass nur vertrauenswürdige Software, die im privilegierten Modus läuft, kritische Systemoperationen durchführen kann, die über CP15 gesteuert werden​

### MCR- und MRC-Instruktionen

In der ARM-Architektur ermöglichen die MCR- und MRC-Instruktionen den Datenaustausch zwischen ARM-Registern und Coprozessor-Registern. Die MCR-Instruktion dient dazu, den Zustand oder die Konfiguration eines Coprozessors zu ändern, indem sie Daten von einem ARM-Register in ein Coprozessor-Register überträgt. Die MRC-Instruktion wird verwendet, um den aktuellen Zustand oder Konfigurationsdaten eines Coprozessors auszulesen und in ein ARM-Register zu laden, was beispielsweise bei der Überprüfung oder Anpassung von Systemfunktionen wichtig ist.

Bedeutung und Verwendung
MCR (Move to Coprocessor Register): Lädt den Inhalt eines ARM-Registers in ein Coprozessor-Register.
MRC (Move to ARM Register from Coprocessor): Liest den Inhalt eines Coprozessor-Registers und speichert ihn in einem ARM-Register.
Syntax: 
```
  MCR: `mcr <coprocessor>, <opcode1>, <Rt>, <CRn>, <CRm>, <opcode2>`
  MRC: `mrc <coprocessor>, <opcode1>, <Rt>, <CRn>, <CRm>, <opcode2>`

<coprocessor>: Bestimmt den Coprozessor, z.B. `p15` für die Systemsteuerungseinheit.
<opcode1>: Zusätzlicher Opcode zur Spezifizierung der Operation.
<Rt>: Bei MCR das ARM-Register, dessen Wert übertragen wird; bei MRC das ARM-Register, in das der Wert gelesen wird.
<CRn>: Zielregister im Coprozessor (bei MCR) oder Quellregister im Coprozessor (bei MRC).
<CRm>: Optionales zweites Coprozessor-Register.
<opcode2>: Weiterer Opcode für Steuerinformationen.
```

