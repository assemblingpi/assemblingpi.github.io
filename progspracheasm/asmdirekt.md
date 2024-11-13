# A.1 Einführung
## 1.4.4 Die Programmiersprache Assembler: Assembler-Direktiven

**Assembler-Direktiven** sind Anweisungen, nicht in Maschinencode übersetzt werden, sondern die den Assembler, also das Programm zur Übersetzung des Quellcodes in maschinenlesbaren Binärcode, steuern. 
Sie erfüllen mehrere wesentliche Funktionen: Direktiven definieren Speicherbereiche, initialisieren Daten, strukturieren Codeabschnitte und steuern den Assemblierungsprozess. 

Direktiven wie `.data` und `.text` ermöglichen es dem Assembler zwischen statischen Daten und ausführbarem Code zu unterscheiden. Diese sections-Direktiven legen nicht nur fest, wie der Assembler Code und Daten organisiert, sondern auch, wie der Linker die verschiedenen Abschnitte eines Programms zusammenführt und im finalen Binärcode anordnet. Auf den Linker werden wir im **Teil B** des Tutorials noch genauer eingehen!

Andere Direktiven legen fest, wie Daten im Speicher abgelegt und initialisiert werden sollen. Die **`.align`**-Direktive beispielsweise richtet Daten an bestimmten Speicheradressen aus. Das Alignment, die AUsrichtung von Daten im Speicher ist bei vielen Prozessoren entscheidend, weil diese Daten nur dann effizient oder überhaupt korrekt lesen und verarbeiten können, wenn diese an adressgerechten Speichergrenzen liegen. Sind Daten oder Code nicht ausgerichtet – etwa eine 4-Byte-Zahl nicht auf eine durch 4 teilbare Adresse –, kann dies entweder zu Fehlern führen oder die Leistung deutlich beeinträchtigen, da der Prozessor zusätzliche Zyklen für den Zugriff benötigt.

Die Assembler-Anweisungen **.global** und **.extern** sind besonders in größeren Programmen mit Quellcode aus mehreren Dateien hilfreich, da sie dem Linker ermöglichen, Symbole und Funktionen über Modulgrenzen hinweg zu referenzieren und so eine strukturierte, modulare Programmgestaltung zu unterstützen.
- Mit `.global` wird ein Symbol (z. B. eine Funktion oder Variable) als global gekennzeichnet, sodass es von anderen Modulen aus zugänglich ist. Diese Kennzeichnung ermöglicht es, dass das Symbol in anderen Quellcodedateien referenziert werden kann, was die Wiederverwendbarkeit und Modularität des Codes fördert. 
- Die `.extern`-Direktive signalisiert dem Assembler hingegen, dass eine Funktion oder Variable, die in einer anderen Datei definiert ist, auch in diesem Modul verwendet werden soll, ohne erneut definiert werden zu müssen. 

### Anwendung von Assembler-Direktiven im Detail

- **Daten initialisieren und organisieren**: Direktiven wie `.byte` und `.word` bestimmen die Größe von Daten (1 bzw. 4 Bytes) und initialisieren den Speicher direkt mit Werten. Die Direktive `.asciz` speichert eine Zeichenkette und markiert deren Ende mit einem Nullzeichen. 
Die `.align <n>`-Direktive richtet den nachfolgenden Code oder Daten auf eine 2ⁿ-Byte-Grenze aus. 

Die `.space`-Direktive reserviert Speicherplatz für Daten und kann optional mit einem spezifischen Wert initialisiert werden. Dies ist besonders nützlich für Puffer oder Datenblöcke, die entweder uninitialisiert bleiben oder mit einem bestimmten Wert vorbereitet werden sollen, bevor sie zur Laufzeit verwendet werden.

**Space ohne Initialisierung:**
```
.space 8            @ Reserviert 8 Bytes uninitialisierten Speicher 
```

**Space mit Initialisierung:**
```
.space 8, 0xFF      @ Reserviert 8 Bytes und initialisiert jeden Byte mit 0xFF
```

**Beispiel mit verschiedenen Direktiven:**
```assembly
.data
.align 2            @ Richtet Daten auf eine 4-Byte-Grenze aus
.word 0x1234        @ Initialisiert eine 4-Byte-Variable
.asciz "Hallo"      @ Speichert die Zeichenkette "Hallo" mit Nullterminierung
.space 8            @ Reserviert 8 Bytes im Speicher
```

### Zahlenformate im Assembler

In ARM-Assemblersprachen können Zahlen in verschiedenen Formaten dargestellt werden, was die Lesbarkeit und Klarheit des Codes unterstützt. 
- **Dezimalzahlen** erscheinen ohne Präfix (z. B. `mov r0, #10`)
- **Hexadezimalzahlen** beginnen mit `0x` (z. B. `mov r0, #0xA`)
- **Binärzahlen** beginnen mit `0b` (z. B. `mov r0, #0b1010`). 

Unterschiedliche Zahlensysteme erleichtern die Arbeit, da Adressen oft im Hexadezimalformat und bitweise Operationen im Binärformat übersichtlicher sind.

### Kommentare

Kommentare im Assembler-Code verbessern die Lesbarkeit und Verständlichkeit des Codes, ohne dessen Funktionalität zu beeinflussen. Sie dienen zur Dokumentation und Erklärung und sind besonders bei komplexeren Abschnitten oder Speicherstrukturen hilfreich. Einzeilige Kommentare beginnen mit `@` oder `//`, während mehrzeilige Kommentare mit `/* ... */` eingeschlossen werden können. 
Eine gute Dokumentation im Code erleichtert die Wartung und die Übersichtlichkeit, insbesondere bei großen Projekten.

|---------------------|-------------------------------|--------------------------------|
| [zurück](adrmodi.md)| [Hauptmenü](../ueberblick.md) | [weiter](../CPUlator/erste.md) | 


| **1.4 Die Programmiersprache Assembler**  	                                            |
|-------------------------------------------------------------------------------------------|
| [1.4.1 Was ist Assembler?](../progspracheasm/progasmintro.md)                             |
| [1.4.2 Anweisungen und Operanden](../progspracheasm/anwops.md)                            |
| [1.4.3 Operanden und Adressierungsarten](../progspracheasm/adrmodi.md)                    |
| [1.4.4 Assembler-Direktiven](../progspracheasm/asmdirekt.md)                              |