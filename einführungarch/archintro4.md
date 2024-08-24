## Befehlzyklus: Fetch, Decode, Execute

Der Befehlszyklus in einem Computerprozessor beschreibt, wie Befehle vom Prozessor verarbeitet werden. Er besteht vereinfacht dargestellt aus drei grundlegenden Schritten: **Fetch (Holen)**, **Decode (Dekodieren)** und **Execute (Ausführen)**. Jeder dieser Schritte spielt eine wesentliche Rolle bei der Ausführung von Programmen.
1. **Fetch (Holen):** Der Prozessor holt den nächsten Befehl aus dem Speicher. Die Adresse des Befehls wird durch den **Program Counter (PC)** bestimmt, der nach jedem Fetch-Schritt auf den nächsten Befehl zeigt. Der Program Counter ist entscheidend, da er sicherstellt, dass die Befehle in der richtigen Reihenfolge ausgeführt werden.
2. **Decode (Dekodieren):** Der geholte Befehl wird vom Prozessor dekodiert. In dieser Phase interpretiert der Prozessor den Befehl, um festzustellen, welche Operation ausgeführt werden soll und welche Operanden (Daten) erforderlich sind.
3. **Execute (Ausführen):** Der Prozessor führt die im Befehl spezifizierte Operation aus. Dies kann eine mathematische Berechnung, ein Datenzugriff oder eine andere Operation sein, die im Befehl angegeben ist.

### Single-Cycle-Architektur

Bei einer **Single-Cycle-Architektur** wird jeder Befehl in genau einem Taktzyklus (einem „Cycle“) vollständig abgearbeitet. Die Phasen **Fetch**, **Decode** und **Execute** werden Schritt für Schritt nacheinander abgearbeitet.

### Pipelined-Architektur

Die **Pipelined-Architektur** ist ein grundlegendes Konzept, das die Verarbeitungsgeschwindigkeit von CPUs erheblich steigern kann. Sie basiert auf der Idee, dass der Prozess der Befehlsausführung in mehrere Phasen unterteilt wird, die parallel ausgeführt werden können. Diese Phasen in unserem Modell die zuvor genannten Schritte: **Fetch**, **Decode** und **Execute**.
Durch die parallele Ausführung dieser Phasen in einer Pipeline können mehrere Befehle gleichzeitig verarbeitet werden. Während ein Befehl dekodiert wird, kann der nächste bereits geholt und ein vorheriger ausgeführt werden. Dies erhöht die Effizienz des Prozessors erheblich, da keine der Hauptkomponenten des Prozessors (wie das Rechenwerk oder das Steuerwerk) untätig bleibt.

### Erweiterter Befehlzyklus in modernen Prozessoren

Moderne Prozessoren wie der ARMv7 erweitern den klassischen Befehlzyklus um zusätzliche Stufen. Diese zusätzlichen Stufen ermöglichen eine noch effizientere Verarbeitung der Befehle und tragen zur Verbesserung der Gesamtleistung des Prozessors bei. Diese erweiterten Stufen werden hier jedoch nicht weiter thematisiert.


In der ARMv7-A Architektur wird der Befehlzyklus, der normalerweise die Phasen **Fetch**, **Decode** und **Execute** umfasst, durch zusätzliche Stufen erweitert, um eine höhere Effizienz und Leistung zu erzielen.

Die 8-stufige Pipeline des ARM Cortex-A7 Prozessors besteht aus den folgenden Stufen:

1. **Fetch (F1):** Die erste Stufe des Fetch-Prozesses, bei der der Befehl aus dem Speicher geladen wird.
2. **Fetch (F2):** Die zweite Stufe des Fetch-Prozesses, in der der Befehl weiter vorbereitet wird.
3. **Decode (D1):** Die erste Dekodierstufe, in der der geladene Befehl dekodiert wird.
4. **Decode (D2):** Die zweite Dekodierstufe, in der zusätzliche Dekodierung und Vorbereitung für die Ausführung stattfindet.
5. **Execute (E):** Die Stufe, in der die eigentliche Ausführung des Befehls stattfindet.
6. **Memory (M):** Die Stufe, in der auf den Speicher zugegriffen wird, falls erforderlich (z.B. bei Load/Store-Operationen).
7. **Write Back (WB):** Die Stufe, in der die Ergebnisse der Ausführung zurück in die Register geschrieben werden.
8. **Commit (C):** Die letzte Stufe, in der die Ausführung abgeschlossen wird und der Befehl als abgeschlossen markiert wird.

Diese Pipeline-Stufen ermöglichen eine effiziente Verarbeitung und unterstützen die hohe Leistung und Energieeffizienz, für die der Cortex-A7 bekannt ist【18†source】.

### Pipeline in ARMv7-A:
In ARMv7-A Prozessoren wie dem Cortex-A9 wird eine **5-stufige Pipeline** verwendet, die es erlaubt, dass mehrere Befehle gleichzeitig in verschiedenen Phasen des Befehlzyklus bearbeitet werden. Diese Pipeline umfasst die oben genannten Phasen und verbessert die Gesamteffizienz des Prozessors erheblich, indem sie sicherstellt, dass keine der CPU-Komponenten untätig bleibt【6†source】【7†source】.






In der ARMv7-A Architektur hat der Program Counter (PC) aufgrund der Pipeline eine Besonderheit, die zu Verwirrung führen kann: Der PC zeigt nicht immer auf die nächste auszuführende Instruktion, sondern ist häufig um mehrere Instruktionen voraus. Dies liegt an der Art und Weise, wie die Pipeline die Befehle bearbeitet.

### Pipeline und PC in ARMv7-A

ARMv7-A Prozessoren verwenden eine Pipeline, die es ermöglicht, mehrere Befehle gleichzeitig zu verarbeiten. In einem typischen 5-stufigen Pipeline-Design durchläuft eine Instruktion die Phasen **Fetch**, **Decode**, **Execute**, **Memory Access** und **Write Back**. Aufgrund dieser parallelen Verarbeitung zeigt der PC in der Regel auf die Instruktion, die gerade geholt wird, und nicht auf diejenige, die ausgeführt wird.

### Offset des PC

In ARM-Architekturen, insbesondere bei der Ausführung von ARM-Befehlen, zeigt der PC oft auf die Adresse der aktuellen Instruktion plus einen Offset. Typischerweise wird der PC um **8 Bytes** (zwei Instruktionen) vor der aktuell ausgeführten Instruktion inkrementiert. Bei Thumb-Befehlen (die 16- oder 32-bit groß sein können) ist dieser Offset **4 Bytes**.

Der Grund für diesen Offset liegt in der Pipeline-Architektur selbst: Während eine Instruktion dekodiert wird, wird die nächste bereits geholt. Dadurch hat der PC, wenn er während der Ausführung einer Instruktion gelesen wird, bereits die Adresse der Instruktion plus 8 (oder 4 bei Thumb).

### Auswirkungen

Dieser Unterschied bedeutet, dass, wenn ein Programmierer den PC direkt manipuliert oder ihn in einem Befehl verwendet, der tatsächliche Wert des PC nicht auf die nächste Instruktion zeigt, die ausgeführt werden soll, sondern auf eine weiter voraus liegende Instruktion. Das ist besonders wichtig bei der Behandlung von Ausnahmen (Exceptions), wo der PC-Wert genutzt wird, um festzustellen, wo die Instruktion war, die die Ausnahme ausgelöst hat.

Dies ist ein historisches Designmerkmal, das für die Kompatibilität mit älterer Software beibehalten wurde und nicht unbedingt die tatsächliche Position der ausgeführten Instruktion widerspiegelt【15†source】【16†source】【17†source】【18†source】.

**Möchtest du mehr über spezifische Beispiele erfahren, bei denen dieses Verhalten zu Problemen führen kann, oder über die Auswirkungen auf bestimmte Programmiertechniken in ARM?**