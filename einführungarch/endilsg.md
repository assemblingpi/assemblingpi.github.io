# A.1 Einführung
## 1.2.3 Grundlagen der Computerarchitektur: Der Speicher
### Lösung: Speicherzugriffe in Little und Big Endian

#### Lösung Little Endian:

| Adresse  | Byte 1 | Byte 2 | Byte 3 | Byte 4 |
|----------|--------|--------|--------|--------|
| 0x000    | 0xAB   | 0x00   | 0xBB   | 0x0A   |
| 0x004    | 0x44   | 0x33   | 0x22   | 0x11   |


#### Lösung Big Endian:

| Adresse  | Byte 1 | Byte 2 | Byte 3 | Byte 4 |
|----------|--------|--------|--------|--------|
| 0x000    | 0xAB   | 0x00   | 0x0A   | 0xBB   |
| 0x004    | 0x11   | 0x22   | 0x33   | 0x44   |

|------------------------|-------------------------------|-------------------|
| [zurück](endiantest.md)| [Hauptmenü](../ueberblick.md) | [weiter](cache.md)| 


| **1.2 Grundlagen der Computerarchitektur**                                                |
|-------------------------------------------------------------------------------------------|
| [1.2.1 Die Ausführung von Programmen durch den Computer](../einführungarch/cpuintro.md)   |
| [1.2.2 Von-Neumann-Architektur](../einführungarch/archintro.md)                           |
| [1.2.3 Der Speicher](../einführungarch/memintro.md)                                       |
| [1.2.4 Cache](../einführungarch/cache.md)                                                 |
| [1.2.5 Ein- und Ausgabegeräte](../einführungarch/eaintro.md)                              |
| [1.2.6 Der Systembus](../einführungarch/sbusintro.md)                                     |
| [1.2.7 Der Befehlszyklus](../einführungarch/archintro_pip.md)                             |