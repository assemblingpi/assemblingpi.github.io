# A.1 Einführung
## 1.2.3 Grundlagen der Computerarchitektur: Der Speicher

Der Speicher ist ein zentraler Bereich in der Von-Neumann-Architektur, in dem sowohl Programmbefehle als auch Daten abgelegt werden. Dies ermöglicht der CPU einen flexiblen Zugriff, da sich Programme und Daten im gleichen Speicher befinden. Der Speicher kann als ein Array betrachtet werden, bei dem jedes Element die Größe eines Words hat und über eine eindeutige Adresse verfügt. Diese Adressen dienen als Indexe, die es der CPU erlauben, gezielt auf bestimmte Speicherplätze zuzugreifen, um Daten zu lesen oder zu schreiben. So wird eine effiziente und direkte Datenverarbeitung ermöglicht.

In einem modernen Computersystem ist der Speicher jedoch in kleinere Einheiten unterteilt, sogenannte Bytes, die die kleinste adressierbare Einheit darstellen. Das bedeutet, dass die CPU auf jede einzelne dieser Einheiten zugreifen und sie manipulieren kann. Ein Byte besteht aus 8 Bit und ist die kleinste Einheit, die über eine eindeutige Adresse angesprochen werden kann. In der Hexadezimaldarstellung wird ein Byte durch zwei Ziffern repräsentiert, die jeweils einen Wert von 0 bis 15 (0-F) annehmen können. Mehrere Bytes können zusammen größere Datenstrukturen wie Words, Doppelwords oder ganze Speicherblöcke bilden.

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

|----------------------|-------------------------------|------------------------|
| [zurück](cpuintro.md)| [Hauptmenü](../ueberblick.md) | [weiter](endiantest.md)| 


| **1.2 Grundlagen der Computerarchitektur**                                                |
|-------------------------------------------------------------------------------------------|
| [1.2.1 Die Ausführung von Programmen durch den Computer](../einführungarch/cpuintro.md)   |
| [1.2.2 Von-Neumann-Architektur](../einführungarch/archintro.md)                           |
| [1.2.3 Der Speicher](../einführungarch/memintro.md)                                       |
| [1.2.4 Cache](../einführungarch/cache.md)                                    |
| [1.2.5 Ein- und Ausgabegeräte](../einführungarch/eaintro.md)                              |
| [1.2.6 Der Systembus](../einführungarch/sbusintro.md)                                     |
| [1.2.7 Der Befehlszyklus](../einführungarch/archintro_pip.md)                             |