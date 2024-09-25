##  Ein- und Ausgabegeräte

E/A-Geräte sind Hardwarekomponenten, die es ermöglichen, Daten in ein Computersystem einzugeben (z.B. Tastaturen, Sensoren) oder Daten aus dem System auszugeben (z.B. Bildschirme, Drucker). Sie bilden die Schnittstelle zwischen dem Computer und der Außenwelt und machen die Interaktion mit dem System möglich.

### Memory Mapped I/O
Um effizient mit diesen E/A-Geräten zu kommunizieren, verwenden Computersysteme oft eine Technik namens Memory Mapped I/O (MMIO). Bei dieser Methode werden die Ein- und Ausgabegeräte direkt in den Adressraum des Prozessors eingebunden. Das bedeutet, dass der Prozessor mit den E/A-Geräten kommunizieren kann, indem er auf bestimmte Speicheradressen zugreift.

In einem System mit MMIO ist ein Teil des Adressraums speziell für Peripheriegeräte reserviert. Der Prozessor kann auf diese Geräte zugreifen, indem er Daten in die entsprechenden Adressen schreibt oder sie daraus liest. Dies ermöglicht eine einfache und direkte Steuerung der Peripheriegeräte.

### Memory Map
Die Memory Map eines Computersystems ist eine Übersicht, die den gesamten Adressraum in verschiedene Bereiche unterteilt. Diese Bereiche legen fest, welche Speicheradressen für den Arbeitsspeicher (RAM), welche für Peripheriegeräte und welche für andere Systemkomponenten reserviert sind. 

#### Beispiel: Raspberry Pi 2b:
Der Raspberry Pi 2B ist ein Einplatinencomputer mit ARM-Architektur, der eine spezifische Memory Map zur Steuerung seiner Hardware verwendet. Der Adressraum des Raspberry Pi ist in mehrere Bereiche unterteilt, von denen an dieser Stelle nur zwei besonders wichtige hervorgehoben werden:

**Arbeitsspeicher (RAM):**

- **Adressbereich:** `0x00000000` bis `0x3EFFFFFF`

- **Beschreibung:** Dieser Bereich umfasst den physischen Arbeitsspeicher des Systems. Hier werden Programme und Daten gespeichert, die vom Prozessor verarbeitet werden.

**Peripheriegeräte (Memory-Mapped I/O):**

- **Adressbereich:** `0x3F000000` bis `0x3FFFFFFF`

- **Beschreibung:** In diesem Bereich sind die Peripherie-Register angesiedelt. Diese Register ermöglichen den direkten Zugriff auf Hardwarekomponenten wie GPIO, UART, SPI, I2C und weitere Schnittstellen.


|-------------------|-------------------------------|-----------------------|
| [zurück](cache.md)| [Hauptmenü](../ueberblick.md) | [weiter](sbusintro.md)| 
