## Befehlzyklus: Fetch, Decode, Execute
Der Befehlszyklus in einem Computerprozessor beschreibt, wie Befehle vom Prozessor verarbeitet werden. Er besteht vereinfacht dargestellt aus drei grundlegenden Schritten: **Fetch (Holen)**, **Decode (Dekodieren)** und **Execute (Ausführen)**. Jeder dieser Schritte spielt eine wesentliche Rolle bei der Ausführung von Programmen.
1. **Fetch (Holen):** Der Prozessor holt den nächsten Befehl aus dem Speicher. Die Adresse des Befehls wird durch den **Program Counter (PC)** bestimmt, der nach jedem Fetch-Schritt auf den nächsten Befehl zeigt. Der Program Counter ist entscheidend, da er sicherstellt, dass die Befehle in der richtigen Reihenfolge ausgeführt werden.
2. **Decode (Dekodieren):** Der geholte Befehl wird vom Prozessor dekodiert. In dieser Phase interpretiert der Prozessor den Befehl, um festzustellen, welche Operation ausgeführt werden soll und welche Operanden (Daten) erforderlich sind.
3. **Execute (Ausführen):** Der Prozessor führt die im Befehl spezifizierte Operation aus. Dies kann eine mathematische Berechnung, ein Datenzugriff oder eine andere Operation sein, die im Befehl angegeben ist.

### Single-Cycle-Architektur
Bei einer **Single-Cycle-Architektur** wird jeder Befehl in genau einem Taktzyklus (einem „Cycle“) vollständig abgearbeitet. Die Phasen **Fetch**, **Decode** und **Execute** werden Schritt für Schritt nacheinander abgearbeitet.

### Pipelined-Architektur
Die **Pipelined-Architektur** ist ein Konzept, das die Verarbeitungsgeschwindigkeit von CPUs erheblich steigert. Sie basiert auf der Idee, dass der Prozess der Befehlsausführung in mehrere Phasen unterteilt wird, die parallel ausgeführt werden können. Diese Phasen sind in unserem Modell die zuvor genannten Schritte: **Fetch**, **Decode** und **Execute**. Während der Prozessor eine Operation ausführt (Execute), dekodiert er die nächste und holt die übernächste Instruktion schon aus dem Speicher.
Durch die parallele Ausführung dieser Phasen in einer Pipeline können somit mehrere Befehle gleichzeitig verarbeitet werden. Dies erhöht die Effizienz des Prozessors erheblich, da keine der Hauptkomponenten des Prozessors (wie die ALU oder Controlunit) untätig bleibt.

### Erweiterter Befehlzyklus in modernen Prozessoren
Moderne Prozessoren wie der ARMv7 erweitern den klassischen Befehlzyklus um zusätzliche Stufen. Diese zusätzlichen Stufen ermöglichen eine noch effizientere Verarbeitung der Befehle und tragen zur Verbesserung der Gesamtleistung des Prozessors bei. Diese erweiterten Stufen werden hier jedoch nicht weiter thematisiert.


In der ARMv7-A Architektur wird der Befehlzyklus, der normalerweise die Phasen **Fetch**, **Decode** und **Execute** umfasst, durch zusätzliche Stufen erweitert, um eine höhere Effizienz und Leistung zu erzielen.

Die 8-stufige Pipeline des ARM Cortex-A7 Prozessors besteht beispielsweise aus den folgenden Stufen:

1. **Fetch (F1):** Die erste Stufe des Fetch-Prozesses, bei der der Befehl aus dem Speicher geladen wird.
2. **Fetch (F2):** Die zweite Stufe des Fetch-Prozesses, in der der Befehl weiter vorbereitet wird.
3. **Decode (D1):** Die erste Dekodierstufe, in der der geladene Befehl dekodiert wird.
4. **Decode (D2):** Die zweite Dekodierstufe, in der zusätzliche Dekodierung und Vorbereitung für die Ausführung stattfindet.
5. **Execute (E):** Die Stufe, in der die eigentliche Ausführung des Befehls stattfindet.
6. **Memory (M):** Die Stufe, in der auf den Speicher zugegriffen wird, falls erforderlich (z.B. bei Load/Store-Operationen).
7. **Write Back (WB):** Die Stufe, in der die Ergebnisse der Ausführung zurück in die Register geschrieben werden.
8. **Commit (C):** Die letzte Stufe, in der die Ausführung abgeschlossen wird und der Befehl als abgeschlossen markiert wird.



### Pipeline in ARMv7-A:
Im Cortex-A7 wird eine **8-stufige Pipeline** verwendet, die es erlaubt, dass mehrere Befehle gleichzeitig in verschiedenen Phasen des Befehlzyklus bearbeitet werden. Diese Pipeline umfasst die oben genannten Phasen und verbessert die Gesamteffizienz des Prozessors erheblich.

### Offset des PC
In gepipelten ARM-Architekturen zeigt der Program Counter (PC) anders als beim CPUlator nicht auf die nächste, sondern auf die übernächste Instruktion. Das liegt daran, dass der PC typischerweise um **8 Bytes** (zwei Instruktionen) vor der gerade ausgeführten Instruktion steht. Dieser Offset resultiert aus der Pipeline-Architektur: Während eine Instruktion dekodiert wird, wird die nächste bereits geladen, weshalb der PC bereits die Adresse der übernächsten Instruktion enthält.