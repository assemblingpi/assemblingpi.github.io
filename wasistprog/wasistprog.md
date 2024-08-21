## Was ist eigentlich ein Programm?

Ein Programm besteht aus einer Abfolge von sehr elementaren Anweisungen für einen Computer, deren schrittweise Abarbeitung dazu führt, dass eine bestimmte Aufgabe erledigt wird. Diese Anweisungen sind in einer Sprache geschrieben, die der Prozessor versteht, und sie werden im Speicher des Computers abgelegt, genauso wie die Daten, die das Programm verarbeitet. 

Um als Programm eine sinnvolle Tätigkeit zu verrichten, braucht es viele dieser elementaren Befehle, die nacheinander abgearbeitet werden müssen. Die Reihenfolge, in der dies stattfindet, nennt man "Programmfluss". Typischerweise werden die Befehle vom Prozessor sequentiell in der Reihenfolge ausgeführt, in der sie auch im Speicher liegen, einer nach dem anderen. Der Programmfluss kann jedoch durch Befehle verändert werden, sodass der Prozessor anstelle des nächsten Befehls im Speicher an eine andere Stelle im Code springt und dort mit der Ausführung von Befehlen fortfährt. 

Ein Programm besteht somit aus Blöcken von Befehlen, die linear abgearbeitet werden und aus Verzweigungen, die diese elementaren Blöcke miteinander verbinden. Diese Blöcke, "Basic Blocks" genannt, enthalten eine Abfolge von Befehlen, die ohne Unterbrechung durchlaufen werden, bis eine Bedingung oder ein Sprungbefehl auftritt. Verzweigungen bestimmen, wie der Programmfluss von einem Block zum nächsten erfolgt, sei es durch bedingte Anweisungen oder direkte Sprünge.

Programme auf diese Weise zu betrachten, ist nicht bloss als abstrakte Betrachtung ein Hilfsmittel für denjenigen, der gerade seine erste Schritte als Assemblerprogrammierer macht, sondern es spiegelt eine ganz grundlegende Eigenschaft von Programmen wider. Dies merkt man, wenn man ein Programm, in welcher Programmierstpache auch immer geschrieben - disassembliert und einen Controlflowgraph vom Disassembler anfertigen lässt.

### Mehr als ein Gedankenspiel
Programme auf diese Weise zu begreifen ist nicht nur eine theoretische Übung, die sich für Anfänger als nützlich erweist. Diese Betrachtungsweise spiegelt tatsächlich eine grundlegende Eigenschaft aller Programme wider. Disassembliert man eine Software und blickt auf den Controlflow-Graph, dann ergibt sich nämlich ein Bild wie das folgende:

![Programm](./prog.png)

[Was ist denn nun ein "Disassembler"?](disasm.md)

[weiter](../progspracheasm/progasmintro.md)

