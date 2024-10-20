# B.1 Einführung
## 1.3.1 Assembler und Linker: Intro

### Was ist ein Assembler?
Die Bedeutung des Begriffs "Assembler" ist doppeldeutig.
Assembler ist zum einen eine Programmiersprache, die Maschinensprache menschenlesbar darstellt. Im Gegensatz zu höheren Programmiersprachen ist Assembler stark hardwareabhängig und erfordert Kenntnisse über die zugrunde liegende CPU-Architektur. Dieser Abschnitt handelt jedoch nicht über die Programmiersprache: Als Assembler bezeichnet man nämlich nicht nur die Programmiersprache, Assembler ist auch ein Programm, das Assembler- Quellcode in maschinenlesbaren Code übersetzt. 
Der Assembler übernimmt zudem die Zuordnung von Symbolen zu Speicheradressen und die Erstellung von sogenannten Objektdateien, die später vom Linker zu ausführbaren Programmen zusammengefügt werden können.

```bash
arm-none-eabi-as -march=armv7-a -mfloat-abi=hard  -mfpu=neon-vfpv4 -mcpu=cortex-a7 -c boot.s -g -o boot.o 
```

In dem gegebenen Befehl wird der `arm-none-eabi-as` Assembler verwendet, der Teil des GNU Toolchain für ARM ist. Dieser Assembler wird benutzt, um Assemblerdateien (`.s`-Dateien) in Objektdateien (`.o`-Dateien) zu übersetzen.

### Was ist ein Linker?
Ein Linker ist ein Programm, das mehrere Objektdateien, zu einer einzigen ausführbaren Datei kombiniert. Sein Hauptzweck ist es, die verschiedenen Teile eines Programms zusammenzuführen und sicherzustellen, dass alle Verweise auf Funktionen und Variablen korrekt aufgelöst werden. Der Linker ersetzt Platzhalter für Adressen durch die tatsächlichen Adressen, passt die Speicheradressen an und kann auch externe Bibliotheken einbinden. Kurz gesagt, der Linker stellt sicher, dass alle Teile des Programms richtig miteinander verbunden sind, sodass das Programm zur Ausführung bereit ist.

```bash
arm-none-eabi-ld -g  boot.o vektor.o -T link.lds -o out/kernel7.elf 
```

Nachdem die Assemblerdateien in Objektdateien übersetzt wurden, werden diese Objektdateien mit dem `arm-none-eabi-ld` Linker gelinkt. Der Linker verwendet die angegebene Linkerskriptdatei (`link.lds`) mit der Option `-T`, um die endgültige Ausgabedatei (`kernel7.elf`) zu erstellen. 

|---------------------------------------|------------------------------------|-----------------------------|
|   [zurück](../Systemprog/sysprog.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](linkerscr.md)    |


|**1.3 Assembler und Linker**                               |
|-----------------------------------------------------------|
| [1.3.1 Intro](wasistasmlinker.md)                         |
| [1.3.2 Grundkonzepte von Linker-Scripten](linkerscr.md)   |