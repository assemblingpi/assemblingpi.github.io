# A.1 Einführung
## 1.2.7 Grundlagen der Computerarchitektur: Der Befehlszyklus

Der Befehlszyklus in einem Computerprozessor beschreibt, wie Befehle vom Prozessor verarbeitet werden. Er besteht vereinfacht dargestellt aus drei grundlegenden Schritten: **Fetch (Holen)**, **Decode (Dekodieren)** und **Execute (Ausführen)**. Jeder dieser Schritte spielt eine wesentliche Rolle bei der Ausführung von Programmen.
1. **Fetch (Holen):** Der Prozessor holt den nächsten Befehl aus dem Speicher. Die Adresse des Befehls wird durch den **Program Counter (PC)** bestimmt, der nach jedem Fetch-Schritt auf den nächsten Befehl zeigt. Der Program Counter ist entscheidend, da er sicherstellt, dass die Befehle in der richtigen Reihenfolge ausgeführt werden.
2. **Decode (Dekodieren):** Der geholte Befehl wird vom Prozessor dekodiert. In dieser Phase interpretiert der Prozessor den Befehl, um festzustellen, welche Operation ausgeführt werden soll und welche Operanden (Daten) erforderlich sind.
3. **Execute (Ausführen):** Der Prozessor führt die im Befehl spezifizierte Operation aus. Dies kann eine mathematische Berechnung, ein Datenzugriff oder eine andere Operation sein, die im Befehl angegeben ist.

### Decodierung von Arm Befehlen
Nachdem der Prozessor einen Befehl aus dem Speicher geladen hat, muss er den Befehl korrekt deuten um die richtigen Signale für Recheneinheiten und Steuereinheiten zu generieren. Beim Dekodieren erkennt er anhand des Binärcodes die Art des Befehls und die beteiligten Operanden. Diese Information wird genutzt, um die nächsten Schritte zur Ausführung des Befehls festzulegen. Die Dekodierung bildet somit die Brücke zwischen dem Abrufen des Befehls und dessen Ausführung.

Eine anschauliche und vollständige Übersicht über das Format der ARM-Maschinencode-Befehlen finden Sie [hier](https://xlogicx.net/ARM_Atlas.html).


**Im folgenden das Befehlsformat einiger Instruktionstypen von ARMv7 Prozessoren:**

`Datenverarbeitungsbefehle`

|31 ... 28|27 ... 25| 24 ... 21 |  20 | 19 ... 16|15 ... 12|11  ...  0|
|---------|---------|-----------|-----|----------|---------|----------|
| Cond    | 001     | Opcode    | S   | Rn       | Rd      | Operand2 | 

`Multiply`

|31 ... 28|27 ... 22|21 |20 |19 ...16|15 ... 12|11 ... 8|7 ... 4|3 ... 0| 
|---------|---------|---|---|--------|---------|--------|-------|-------|      
| Cond    | 000000  | A | S | Rd     | Rn      | Rs     | 1001  | Rm    | 

`Load/Store Byte/ Word`

|31 ... 28|27 ... 26|25 |24 |23 |22 |21 |20 |19 ... 16|15 ... 12|11 ... 0|
|---------|---------|---|---|---|---|---|---|---------|---------|--------|
| Cond    | 01      | I | P | U | B | W | L | Rn      | Rd      | Offset |   

`Branch`

|31 ... 28|27 ... 25|24 |23 ... 0|
|---------|---------|---|--------|
| Cond    | 101     | L | Offset | 

`Branch Exchange`

|31 ... 28|27 ... 24|23 ... 20|19 ... 17|16 ... 13|12 ... 9|8 ... 5|4 ... 0|
|---------|---------|---------|---------|---------|---------|------|-------|
| Cond    | 0001    | 0010    | 1111    | 1111    | 1111    | 0001 | Rn    | 


### Single-Cycle-Architektur
Bei einer **Single-Cycle-Architektur** wird jeder Befehl in genau einem Taktzyklus (einem „Cycle“) vollständig abgearbeitet. Die Phasen **Fetch**, **Decode** und **Execute** werden Schritt für Schritt nacheinander abgearbeitet.

### Pipelined-Architektur
Die **Pipelined-Architektur** ist ein Konzept, das die Verarbeitungsgeschwindigkeit von CPUs erheblich steigert. Sie basiert auf der Idee, dass der Prozess der Befehlsausführung in mehrere Phasen unterteilt wird, die parallel ausgeführt werden können. Diese Phasen sind in unserem Modell die zuvor genannten Schritte: **Fetch**, **Decode** und **Execute**. Während der Prozessor eine Operation ausführt (Execute), dekodiert er die nächste und holt die übernächste Instruktion schon aus dem Speicher.
Durch die parallele Ausführung dieser Phasen in einer Pipeline können somit mehrere Befehle gleichzeitig verarbeitet werden. Dies erhöht die Effizienz des Prozessors erheblich, da keine der Hauptkomponenten des Prozessors (wie die ALU oder Controlunit) untätig bleibt.

### Erweiterter Befehlzyklus in modernen Prozessoren
Moderne Prozessoren wie der ARMv7 erweitern den klassischen Befehlzyklus um zusätzliche Stufen. Diese zusätzlichen Stufen ermöglichen eine noch effizientere Verarbeitung der Befehle und tragen zur Verbesserung der Gesamtleistung des Prozessors bei. Diese erweiterten Stufen werden hier jedoch nicht weiter thematisiert.

In der ARMv7-A Architektur wird der Befehlzyklus, der Phasen wie **Fetch**, **Decode** und **Execute** umfasst, durch zusätzliche Stufen erweitert, um eine höhere Effizienz und Leistung zu erzielen.

Die 8-stufige Pipeline des ARM Cortex-A7 Prozessors besteht beispielsweise aus den folgenden Stufen:

1. **Fetch (F1):** Die erste Stufe des Fetch-Prozesses, bei der der Befehl aus dem Speicher geladen wird.
2. **Fetch (F2):** Die zweite Stufe des Fetch-Prozesses, in der der Befehl weiter vorbereitet wird.
3. **Decode (D1):** Die erste Dekodierstufe, in der der geladene Befehl dekodiert wird.
4. **Decode (D2):** Die zweite Dekodierstufe, in der zusätzliche Dekodierung und Vorbereitung für die Ausführung stattfindet.
5. **Execute (E):** Die Stufe, in der die eigentliche Ausführung des Befehls stattfindet.
6. **Memory (M):** Die Stufe, in der auf den Speicher zugegriffen wird, falls erforderlich (z.B. bei Load/Store-Operationen).
7. **Write Back (WB):** Die Stufe, in der die Ergebnisse der Ausführung zurück in die Register geschrieben werden.
8. **Commit (C):** Die letzte Stufe, in der die Ausführung abgeschlossen wird und der Befehl als abgeschlossen markiert wird.

#### Pipeline in ARMv7-A
Im Cortex-A7 wird eine **8-stufige Pipeline** verwendet, die es erlaubt, dass mehrere Befehle gleichzeitig in verschiedenen Phasen des Befehlzyklus bearbeitet werden. Diese Pipeline umfasst die oben genannten Phasen und verbessert die Gesamteffizienz des Prozessors erheblich.

Ein Aspekt, der in diesem Kontext dabei oft zu Verwirrung führt, ist die Rolle des **Program Counters (PC)** in der Pipeline-Architektur. Im ARM-Modus zeigt der PC während der Ausführung einer Instruktion nicht auf die aktuelle, sondern auf die übernächste Instruktion. Dies liegt daran, dass die Pipeline die nächsten Instruktionen bereits im Voraus lädt. Im **ARM-Modus** (bei 32-Bit-Instruktionen) ist der PC immer **8 Bytes voraus**, im **Thumb-Modus** (bei 16-Bit-Instruktionen) **4 Bytes**. Dieser Vorlauf ermöglicht es dem Prozessor, Instruktionen schneller zu verarbeiten, da er die nächsten Schritte bereits vorbereitet, während eine Instruktion noch dekodiert oder ausgeführt wird.

#### Warum zeigt der PC im Debugger trotzdem scheinbar auf die aktuelle Instruktion?

Beim Debuggen und der Ausführung in Emulatoren wie dem CPUlator scheint es teils jedoch, als würde der PC auf die **aktuell ausgeführte** Instruktion zeigen. Dies ist darauf zurückzuführen, dass Debugger speziell dafür ausgelegt sind, die Pipeline-Effekte zu abstrahieren. Sie passen den angezeigten Wert des PC an, um den Entwicklern eine klarere Darstellung des tatsächlichen Programmablaufs zu geben. Ohne diese Anpassung könnte der Vorlauf des PC zu Missverständnissen führen, da der Entwickler erwarten würde, dass der PC auf die Instruktion zeigt, die gerade ausgeführt wird. 

Diese Abstraktion stellt sicher, dass Breakpoints korrekt gesetzt werden und der Entwickler den Programmfluss besser nachvollziehen kann. Letztlich wird dadurch der Unterschied zwischen der tatsächlichen Funktionsweise der Pipeline und der Darstellung im Debugger verschleiert, um den Entwicklungsprozess zu vereinfachen.

#### Konsequenzen für die Praxis

Beim Programmieren in Assembler, müssen Entwickler den Vorlauf des PC berücksichtigen, vor allem bei **PC-relativen Berechnungen**. Solche Berechnungen, die auf den aktuellen PC-Wert zugreifen, haben den Pipeline-Vorlauf zu berücksichtigen, sodass Speicherzugriffe oder Sprungadressen korrekt berechnet werden. Im Debugging-Prozess selbst werden diese Details jedoch bewusst ausgeblendet, um Verwirrung zu vermeiden und den Fokus auf die Logik des Quellcodes zu legen.

### ISA und Mikroarchitektur

Die **ISA (Instruction Set Architecture)** definiert, welche Befehle ein Prozessor versteht und wie diese codiert sind. Die **Mikroarchitektur** beschreibt, wie der Prozessor intern aufgebaut ist, um diese Befehle auszuführen. Während die ISA vorgibt, was der Prozessor tun kann, bestimmt die Mikroarchitektur, wie dies umgesetzt wird.

Weitere Schlüsselkonzepte moderner CPU-Mikroarchitektur werden in folgendem [Video](https://www.youtube.com/watch?v=o_WXTRS2qTY) anschaulich erklärt.

|-----------------------|-------------------------------|---------------------------------------|
| [zurück](sbusintro.md)| [Hauptmenü](../ueberblick.md) | [weiter](../ZahlensysBitsBytes/zbb.md)| 


| **1.2 Grundlagen der Computerarchitektur**                                                |
|-------------------------------------------------------------------------------------------|
| [1.2.1 Die Ausführung von Programmen durch den Computer](../einführungarch/cpuintro.md)   |
| [1.2.2 Von-Neumann-Architektur](../einführungarch/archintro.md)                           |
| [1.2.3 Der Speicher](../einführungarch/memintro.md)                                       |
| [1.2.4 Cache](../einführungarch/cache.md)                                    |
| [1.2.5 Ein- und Ausgabegeräte](../einführungarch/eaintro.md)                              |
| [1.2.6 Der Systembus](../einführungarch/sbusintro.md)                                     |
| [1.2.7 Der Befehlszyklus](../einführungarch/archintro_pip.md)                             |