## Assembling Pi: Ein praxisorientiertes Tutorial zur Assembler-Programmierung auf dem Raspberry Pi

Willkommen zu Assembling Pi – einem Tutorial, das Sie Schritt für Schritt in die Welt der Assembler-Programmierung auf dem Raspberry Pi einführt. Dieses Tutorial richtet sich an alle, die ein tieferes Verständnis für das Zusammenspiel von Hard- und Software auf einer grundlegenden Ebene erlangen möchten, auch ohne Vorkenntnisse in der Assembler-Programmierung.

In den kommenden Kapiteln werden Sie die Grundlagen der ARM-Assembler-Programmierung kennenlernen. Sie erfahren nicht nur mehr über wichtige Konzepte der ARM-Architektur und Systemprogrammierung, sondern auch über die faszinierende Welt der Grafikprogrammierung. Dieses praxisorientierte Tutorial bietet Ihnen nicht nur theoretisches Wissen, sondern begleitet jeden Schritt mit gezielten Übungsaufgaben, um das Gelernte direkt anzuwenden und zu festigen.

Begleiten Sie uns auf dem Weg die Sprache der ARM-Prozessoren zu erlernen!



### Hinweis zu den Übungsaufgaben:  
Es wird empfohlen, die gestellten Aufgaben zunächst eigenständig zu bearbeiten. Falls Schwierigkeiten auftreten, kann der bereitgestellte Quellcode als Orientierung dienen. Nutzen Sie Tools wie den CPUlator oder QEMU, um zu erkunden, wie bestimmte Befehle das System beeinflussen. Da dieses Tutorial nicht alle Aspekte des ARMv7 Assemblers abdeckt, ist ergänzende Recherche sinnvoll.

Der Fokus des Quellcodes liegt auf Übersichtlichkeit, nicht auf Perfektion oder Optimierung. Machen Sie aktiv Notizen, recherchieren Sie bei Bedarf und experimentieren Sie mit eigenen Ideen. Herausforderungen bieten wertvolle Lernmöglichkeiten und vertiefen Ihr Verständnis.


[Tutorial starten](wasistprog/wasistprog.md)


Test:

## Decodierung von Arm Befehlen

im folgenden das Befehlsformat einiger Instruktionstypen von ARMv7 Prozessoren:

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


