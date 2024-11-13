# A.1 Einführung
## 1.2.3 Grundlagen der Computerarchitektur: Der Speicher
### Übungsaufgabe: Speicherzugriffe in Little und Big Endian

Gegeben ist ein Speicherbereich, der vollständig mit 0 initialisiert wurde. Ihre Aufgabe besteht darin, die folgenden Werte nacheinander in den Speicher zu schreiben, wobei Sie zuerst alle Schritte in Little Endian und anschließend in Big Endian ausführen:

1. **32-Bit Wert (0x11223344)**: Schreibe diesen Wert an die Adresse 0x0004.
2. **16-Bit Wert (0x0ABB)**: Schreibe diesen Wert an die Adresse 0x0002.
3. **8-Bit Wert (0xAB)**: Schreibe diesen Wert an die Adresse 0x0000.

| Adresse  | Byte 1 | Byte 2 | Byte 3 | Byte 4 |
|----------|--------|--------|--------|--------|
| 0x000    | 0x00   | 0x00   | 0x00   | 0x00   |
| 0x004    | 0x00   | 0x00   | 0x00   | 0x00   |

**Bearbeiten sie diese Aufgabe mit Stift und Papier und vergleichen Sie diese im Anschluss mit der Lösung**

|----------------------|-------------------------------|---------------------|
| [zurück](memintro.md)| [Hauptmenü](../ueberblick.md) | [weiter](endilsg.md)| 


| **1.2 Grundlagen der Computerarchitektur**                                                |
|-------------------------------------------------------------------------------------------|
| [1.2.1 Die Ausführung von Programmen durch den Computer](../einführungarch/cpuintro.md)   |
| [1.2.2 Von-Neumann-Architektur](../einführungarch/archintro.md)                           |
| [1.2.3 Der Speicher](../einführungarch/memintro.md)                                       |
| [1.2.4 Cache](../einführungarch/cache.md)                                                 |
| [1.2.5 Ein- und Ausgabegeräte](../einführungarch/eaintro.md)                              |
| [1.2.6 Der Systembus](../einführungarch/sbusintro.md)                                     |
| [1.2.7 Der Befehlszyklus](../einführungarch/archintro_pip.md)                             |