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
