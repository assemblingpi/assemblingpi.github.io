## Befehlzyklus: Fetch, Decode, Execute
Der Befehlszyklus in einem Computerprozessor beschreibt, wie Befehle vom Prozessor verarbeitet werden. Er besteht vereinfacht dargestellt aus drei grundlegenden Schritten: **Fetch (Holen)**, **Decode (Dekodieren)** und **Execute (Ausführen)**. Jeder dieser Schritte spielt eine wesentliche Rolle bei der Ausführung von Programmen.
1. **Fetch (Holen):** Der Prozessor holt den nächsten Befehl aus dem Speicher. Die Adresse des Befehls wird durch den **Program Counter (PC)** bestimmt, der nach jedem Fetch-Schritt auf den nächsten Befehl zeigt. Der Program Counter ist entscheidend, da er sicherstellt, dass die Befehle in der richtigen Reihenfolge ausgeführt werden.
2. **Decode (Dekodieren):** Der geholte Befehl wird vom Prozessor dekodiert. In dieser Phase interpretiert der Prozessor den Befehl, um festzustellen, welche Operation ausgeführt werden soll und welche Operanden (Daten) erforderlich sind.
3. **Execute (Ausführen):** Der Prozessor führt die im Befehl spezifizierte Operation aus. Dies kann eine mathematische Berechnung, ein Datenzugriff oder eine andere Operation sein, die im Befehl angegeben ist.

## Decodierung von Arm Befehlen
Nachdem der Prozessor einen Befehl aus dem Speicher geladen hat, muss er den Befehl korrekt deuten um die richtigen Signale für Recheneinheiten und Steuereinheiten zu generieren. Beim Dekodieren erkennt er anhand des Binärcodes die Art des Befehls und die beteiligten Operanden. Diese Information wird genutzt, um die nächsten Schritte zur Ausführung des Befehls festzulegen. Die Dekodierung bildet somit die Brücke zwischen dem Abrufen des Befehls und dessen Ausführung.

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

Wenn der Prozessor


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

### Pipeline in ARMv7-A:
Im Cortex-A7 wird eine **8-stufige Pipeline** verwendet, die es erlaubt, dass mehrere Befehle gleichzeitig in verschiedenen Phasen des Befehlzyklus bearbeitet werden. Diese Pipeline umfasst die oben genannten Phasen und verbessert die Gesamteffizienz des Prozessors erheblich.

