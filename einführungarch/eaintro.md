##  Ein- und Ausgabegeräte

E/A-Geräte sind Hardwarekomponenten, die Daten in das System einspeisen (z.B. Tastaturen, Sensoren) oder aus dem System herausgeben (z.B. Bildschirme, Drucker). Sie ermöglichen die Interaktion zwischen dem Computer und der Außenwelt.

### Memory Mapped I/O
Um effektiv mit diesen E/A-Geräten zu kommunizieren, verwenden Computersysteme häufig **Memory Mapped I/O (MMIO)**. 
Das ist eine Technik, bei der Ein- und Ausgabegeräte (E/A-Geräte) direkt in den Adressraum des Prozessors integriert werden. 

In einem System mit Memory Mapped I/O sind Teile des Adressraums in der Memory Map speziell für Peripheriegeräte reserviert, wodurch der Prozessor direkt über diese Adressen Daten an die E/A-Geräte senden oder von ihnen empfangen kann, indem er einfach in die entsprechenden Speicheradressen schreibt oder daraus liest.

### Memory Map
Eine Memory Map ist eine Übersicht, die den gesamten Adressraum eines Computersystems in verschiedene Bereiche wie Arbeitsspeicher, Peripheriegeräte und Systemkomponenten unterteilt und dabei definiert, welche Speicheradressen für den RAM, welche für Peripheriegeräte und welche für andere Systemkomponenten reserviert sind.

#### Beispiel: Raspberry Pi 2b:
Dieses Einplatinen-Computersystem basiert auf einer ARM-Architektur und verfügt über eine spezifische Memory Map, die die Interaktion mit seiner Hardware steuert. Der Arbeitsspeicher des Raspberry Pi ist in verschiedene Bereiche unterteilt, die jeweils bestimmten Funktionen dienen.

Die Memory Map des Raspberry Pi 2B lässt sich in mehrere Hauptbereiche unterteilen, von denen wir an dieser Stelle nur auf die beiden wichtigsten eingehen:

**Arbeitsspeicher (RAM):**

**Adressbereich:** 0x00000000 bis 0x3EFFFFFF
**Beschreibung:** Dieser Bereich umfasst den physischen Arbeitsspeicher des Systems. Hier werden Programme und Daten gespeichert, die vom Prozessor verarbeitet werden.

**Peripheriegeräte (Memory-Mapped I/O):**

**Adressbereich:** 0x3F000000 bis 0x3FFFFFFF
**Beschreibung:** In diesem Bereich sind die Peripherie-Register angesiedelt. Diese Register ermöglichen den direkten Zugriff auf Hardwarekomponenten wie GPIO, UART, SPI, I2C und weitere Schnittstellen.


|-------------------|--------------------------|-----------------------|
| [zurück](cache.md)| [Hauptmenü](../index.md) | [weiter](sbusintro.md)| 
