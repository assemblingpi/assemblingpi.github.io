# A.2 Basic Blocks implementieren
## 2.2.1 Sektionen (Abschnitte)
In der Assembler-Programmierung sind Sektionen entscheidend für die Organisation des Codes und der Daten. In den Assembler-Quelldateien werden Sektionen deklariert, um bestimmte Speicherbereiche zu definieren, die jeweils für unterschiedliche Zwecke vorgesehen sind. Diese Struktur hilft dabei, das Programm klar zu gliedern und zu verwalten, sowohl beim Übersetzen des Codes als auch beim späteren Ausführen des Programms.

### Was sind Sektionen?
Sektionen sind Speicherbereiche, die jeweils für einen bestimmten Zweck vorgesehen sind. Ein Assembler-Quellcode besteht meist aus mehreren Sektionen, wobei jede Sektion eine andere Art von Information enthält. Die wichtigsten Sektionen, die in den meisten Programmen vorkommen, sind `.data`, `.bss` und `.text`.

### .data-Sektion:
Die `.data`-Sektion wird für Variablen verwendet, die eine statische Lebensdauer haben und initialisiert werden. Statische Lebensdauer bedeutet, dass eine Variable während der gesamten Programmausführung im Speicher verbleibt. Das besondere an den Daten in dieser Sektion ist, dass hier die Ausgangswerte der Variablen bereits beim assemblieren festgelegt werden.
Im Assembler-Quellcode wird die `.data`-Sektion durch die Direktive `.data` eingeleitet. 

Ein Beispiel für die Definition einer Variablen in dieser Sektion könnte so aussehen:
```asm
.section .data
a: .word 1
```
   Hier wird die Variable `a` mit dem Wert `1` initialisiert und in der `.data`-Sektion
   gespeichert. 

### .bss-Sektion:
Die `.bss`-Sektion (Block Started by Symbol) wird für Variablen verwendet, die ebenfalls eine statische Lebensdauer haben, aber entweder nicht initialisiert sind oder mit Null initialisiert werden. Diese Sektion reserviert Platz im Speicher für solche Variablen, ohne ihnen beim Programmstart einen spezifischen Wert zuzuweisen. Üblicherweise werden die Variablen in der .bss-Sektion bei Systemstart auf Null gesetzt, 
Die Direktive `.bss` wird verwendet, um die nachfolgende Anweisung in diese Sektion einzuordnen. 
Beispiel:
```asm
.section .bss
a: .space 4
```
   Hier wird ein Speicherbereich von 4 Bytes für die Variable `a` reserviert, die in
   der `.bss`-Sektion gespeichert wird. 

### .text-Sektion:
Die `.text`-Sektion enthält den ausführbaren Code des Programms. Alle Assembler-Anweisungen, die dieser Sektion zugeordnet werden, gehören zum Programmcode, der zur Laufzeit ausgeführt wird. Die Direktive `.text` wird verwendet, um die nachfolgenden Instruktionen in dieser Sektion zu platzieren. 
Beispiel:
```asm
.section .text
main:
      mov r0, #1
```

|--------------------------------------|-------------------------------|--------------------|
| [zurück](../Datentransfer/LDR_STR.md)| [Hauptmenü](../ueberblick.md) | [weiter](label.md) |


| **2.2 Sektionen**                                                     |
|-----------------------------------------------------------------------|
| [2.2.1 Sektionen (Abschnitte)](sektionen.md)                          |
| [2.2.2 Label in ARM-Assembler](label.md)                              |