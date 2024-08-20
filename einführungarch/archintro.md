# Einführung in die ARM-Architektur

Um mit der Assembler-Programmierung auf ARM-Prozessoren zu beginnen, ist es entscheidend, die grundlegende Funktionsweise der ARM-CPUs zu verstehen.

ARM-Prozessoren basieren auf der Von-Neumann-Architektur, die drei zentrale Komponenten umfasst: die CPU (Central Processing Unit), bestehend aus Kontrolleinheit und ALU, den Speicher und die Ein-/Ausgabegeräte (E/A). Diese Komponenten sind über einen Systembus miteinander verbunden.

## CPU (Central Processing Unit)

Die CPU ist das Herzstück eines Computers und führt die Befehle eines Programms aus. Sie besteht hauptsächlich aus zwei Teilen:

### Kontrolleinheit
Die Kontrolleinheit steuert den Ablauf der Befehlsverarbeitung, indem sie die Reihenfolge bestimmt, in der Befehle aus dem Speicher geholt, dekodiert und ausgeführt werden. Sie koordiniert auch den Datenfluss zwischen der CPU und anderen Komponenten wie Speicher und Ein-/Ausgabegeräten.

### ALU (Arithmetic Logic Unit)
Die ALU führt arithmetische (z.B. Addition, Subtraktion) und logische Operationen (z.B. AND, OR) durch. Sie ist verantwortlich für die eigentliche Berechnung und Datenverarbeitung. Bei der Durchführung von Operationen setzt die ALU sogenannte Flags, um den Status der Operation anzuzeigen, wie zum Beispiel das Überlauf-Flag (wenn das Ergebnis einer Operation zu groß ist, um im Zielregister gespeichert zu werden) oder das Null-Flag (wenn das Ergebnis einer Operation null ist).

## Speicher

Der Speicher ist ein zentraler Bereich in der Von-Neumann-Architektur, in dem sowohl Programmbefehle als auch Daten abgelegt werden. Dies ermöglicht der CPU einen flexiblen Zugriff, da sich Programme und Daten im gleichen Speicher befinden. Der Speicher kann als ein Array betrachtet werden, bei dem jedes Element die Größe eines Words hat und über eine eindeutige Adresse verfügt. Diese Adressen dienen als Indexe, die es der CPU erlauben, gezielt auf bestimmte Speicherplätze zuzugreifen, um Daten zu lesen oder zu schreiben. So wird eine effiziente und direkte Datenverarbeitung ermöglicht.

Im Folgenden wird eine Tabelle dargestellt, die den Speicher mit seinen Adressen und den entsprechenden Daten in Hexadezimalwerten zeigt. Die Daten sind als Bytes organisiert, wobei jede Zeile vier aufeinanderfolgende Bytes enthält:

| Adresse  | Byte 1 | Byte 2 | Byte 3 | Byte 4 |
|----------|--------|--------|--------|--------|
| 0x000    | 0x1A   | 0x2B   | 0x3C   | 0x4D   |
| 0x004    | 0x5E   | 0x6F   | 0x7A   | 0x8B   |
| 0x008    | 0x9C   | 0xAD   | 0xBE   | 0xCF   |
| 0x00C    | 0xD1   | 0xE2   | 0xF3   | 0x04   |
| 0x010    | 0x15   | 0x26   | 0x37   | 0x48   |

### Endianess

Endianess bezieht sich darauf, wie eine CPU mehrbytegroße Datenwerte im Speicher ablegt, also in welcher Reihenfolge (aufsteigend oder absteigend) die einzelnen Bytes eines Werts gespeichert werden.

**Little Endian**: Wenn die CPU einen mehrbytegroßen Wert in den Speicher schreibt, wird das niederwertigste Byte (Least Significant Byte) an die niedrigste Speicheradresse geschrieben. Das bedeutet, die Reihenfolge der Bytes im Speicher ist umgekehrt zur natürlichen Reihenfolge der Bytes im Wert. 
Beispielsweise würde die CPU den 32-Bit-Wert `0x12345678` als `78 56 34 12` im Speicher ablegen.

**Big Endian**: Hier schreibt die CPU das höchstwertige Byte (Most Significant Byte) an die niedrigste Speicheradresse. Das bedeutet, dass die Byte-Reihenfolge im Speicher der natürlichen Reihenfolge der Bytes im Wert entspricht. 
Der Wert `0x12345678` wird also als `12 34 56 78` im Speicher abgelegt.

Die Endianess spielt eine wichtige Rolle, wenn eine CPU Daten in den Speicher schreibt, da sie bestimmt, wie diese Daten später von der CPU oder anderen Systemen interpretiert werden müssen. Unterschiedliche Systeme können unterschiedliche Endianess verwenden, was beim Datenaustausch zwischen ihnen zu Problemen führen kann, wenn die Reihenfolge der Bytes nicht korrekt interpretiert wird.

Moderne ARM-Prozessoren unterstützen sowohl little, als auch big Endian, aber im folgenden Tutorial wollen wir uns auf little Endian beschränken.

[Übungsaufgabe zu Endianess](endiantest.md)

## E/A (Ein-/Ausgabegeräte)

E/A-Geräte sind Hardwarekomponenten, die Daten in das System einspeisen (z.B. Tastaturen, Sensoren) oder aus dem System herausgeben (z.B. Bildschirme, Drucker). Sie ermöglichen die Interaktion zwischen dem Computer und der Außenwelt.

## Systembus

Der Systembus ist das Kommunikationsmedium, das die CPU, den Speicher und die E/A-Geräte miteinander verbindet. Er besteht aus Leitungen, die Daten, Adressen und Steuerinformationen zwischen den Komponenten übertragen, sodass diese effizient zusammenarbeiten können. Der Systembus ist in der Von-Neumann-Architektur entscheidend, um die verschiedenen Komponenten zu koordinieren und Daten auszutauschen.

## Stored Program Concept

Ein zentrales Konzept der Von-Neumann-Architektur ist das "Stored Program Concept". Es besagt, dass Programme und Daten im selben Speicher abgelegt werden. Dies erlaubt es der CPU, Befehle aus dem Speicher zu lesen und auszuführen, wodurch die Flexibilität erhöht wird, verschiedene Programme einfach durch Ändern des Speicherinhalts auszuführen.

## Load/Store-Architektur

Bei ARM-CPUs handelt es sich um eine Load/Store-Architektur, bei denen Speicherzugriffe nur mittels spezieller Befehle (LDR und STR) durchgeführt werden können und direkte Berechnungen auf Daten im Speicher nicht möglich sind (im Gegensatz zu x86 CPUs). Rechenoperationen finden demnach ausschließlich in den Registern (kleine Speichereinheiten) der CPU statt. Dies bedeutet, dass Daten für ihre Verarbeitung zuerst vom Speicher in Register geladen und nach der Berechnung wieder in den Speicher zurückgeschrieben werden müssen. 

Innerhalb der ARM-CPU gibt es verschiedene 32-Bit Register, die für spezifische Aufgaben verwendet werden:

- **General-Purpose Register**: Diese Register dienen allgemeinen Berechnungen und Datenoperationen und sind vielseitig einsetzbar.

- **Stack Pointer (SP)**: Verwaltet den Stack im Speicher und zeigt auf den aktuellen Standort. Er wird verwendet, um temporäre Daten und Rücksprungadressen zu speichern.

- **Link Register (LR)**: Speichert die Rücksprungadresse bei Funktionsaufrufen, um nach der Ausführung der Funktion zur ursprünglichen Adresse zurückzukehren.

- **Frame Pointer (FP)**: Wird verwendet, um auf Parameter und lokale Variablen im Stack zuzugreifen.

- **Program Counter (PC)**: Beinhaltet die Adresse der nächsten auszuführenden Instruktion. Er wird nach jeder Instruktion automatisch erhöht, und bei Verzweigungen angepasst, um zur neuen Instruktionsadresse zu springen.

- **Statusregister**: Ein 32-Bit-Register, das Flags enthält, die den Status des Prozessors anzeigen, wie etwa den Zustand vorheriger Operationen.

## Speicher und Cache

Den Speicher kann man, wie bereits angedeutet, vereinfacht als einen linearen Array von Bytes betrachten. Moderne ARM-CPUs verwenden zudem einen schnellen Zwischenspeicher, bekannt als Cache, um häufig benötigte Daten schnell verfügbar zu machen und Engpässe durch den langsamen Hauptspeicher zu vermeiden, doch damit wollen wir uns im Rahmen dieses Tutorials nicht weiter befassen.
