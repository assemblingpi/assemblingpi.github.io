# B.1 Einführung
## 1.5.2 BCM2836: Grundlegende Unterschiede RISC vs. CISC
In der Welt der Prozessorarchitekturen gibt es zwei Hauptansätze: **RISC (Reduced Instruction Set Computer)** und **CISC (Complex Instruction Set Computer)**. Diese beiden Ansätze unterscheiden sich grundlegend in der Art und Weise, wie Instruktionen verarbeitet werden.

RISC-Prozessoren zeichnen sich durch eine reduzierte und vereinfachte Befehlssatzarchitektur aus. Sie verwenden eine begrenzte Anzahl von einfachen Befehlen, der gleichen Breite, die häufig die gleiche Zeit für die Ausführung benötigen. Dies ermöglicht eine hohe Effizienz und einfache Implementierung von Pipeline-Techniken, bei denen mehrere Instruktionen gleichzeitig verarbeitet werden. Die einheitliche Befehlslänge und die einfache Struktur der Instruktionen tragen dazu bei, dass RISC-Prozessoren oft schneller und energieeffizienter sind. Beispiele für RISC-Architekturen sind ARM und MIPS.

Im Gegensatz dazu bietet CISC eine Vielzahl komplexer Befehle, die oft mehrere Mikrooperationen in einem einzigen Befehl kombinieren. Diese Befehle können unterschiedliche Längen haben, was die Dekodierung und Ausführung komplexer macht. Der Vorteil von CISC liegt in der Reduzierung der Programmlänge und der höheren Flexibilität bei der Programmierung. CISC-Prozessoren wie die x86-Architektur sind darauf ausgelegt, viele Aufgaben mit wenigen, aber komplexen Befehlen zu erledigen.

|-------------------------|------------------------------------|-----------------------------|
|   [zurück](bcm2836.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](armfamily.md)    |


|**1.5 BCM2836**                                                |
|---------------------------------------------------------------|
| [1.5.1 Intro](bcm2836.md)                                     |
| [1.5.2 Grundlegende Unterschiede RISC vs. CISC](risccisc.md)  |
| [1.5.3 Ein Überblick auf die ARM-Familien](armfamily.md)      |