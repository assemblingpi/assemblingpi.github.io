# B.2 Erweiterungen der CPU-Funktionalität
## 2.4.3 Grafik & GPU: Die GPU des BCM2836 VideoCoreIV

Im Raspberry Pi 2 B ist die VideoCore IV GPU integriert, die im Broadcom BCM2836 Chip enthalten ist. Diese GPU ist für die Berechnung und Darstellung von 2D- und 3D-Grafiken sowie für die Hardware-Video-Decodierung zuständig. Sie arbeitet eng mit dem ARM Cortex A7 Prozessor zusammen, um die Systemleistung zu optimieren.

Die Kommunikation zwischen der ARM-CPU und der VideoCore IV GPU erfolgt über einen Mailbox-Mechanismus. Dieser Mechanismus ermöglicht den Austausch von Nachrichten und Daten. 


### Kommunikation zwischen CPU und GPU
Die Kommunikation zwischen der ARM-CPU und der VideoCore IV GPU des Raspberry Pi erfolgt über einen sogenannten Mailbox-Mechanismus. Eine Mailbox wird vom Cortex A7 gelesen und vom GPU beschrieben, während das andere vom GPU gelesen und vom Cortex A7 beschrieben wird. 

**Mailbox:** Mailboxes sind spezielle Speicherbereiche, die als Schnittstellen für die Kommunikation zwischen der CPU und der GPU dienen. Es gibt zwei Mailboxen auf dem Raspberry Pi: Mailbox 0 und Mailbox 1. Diese Mailboxen ermöglichen, dass Daten in beide Richtungen gleichzeitig gesendet und empfangen werden können.

Die Mailbox verfügt über mehrere Register, unter anderem:
```
    MAILBOX_READ
    MAILBOX_STATUS_WRITE
    MAILBOX_WRITE
    MAILBOX_STATUS_READ
```

**Mail:** Eine "Mail" ist die Einheit der Daten, die über die Mailboxen ausgetauscht wird. Jede Mail enthält sowohl die Daten als auch die Kanalinformationen innerhalb eines 32-Bit-Wortes. Die Mail wird in die Mailbox geschrieben oder daraus gelesen und dient als Grundlage für die Kommunikation.

**FIFO (First-In-First-Out):** Jede Mailbox funktioniert als FIFO-Puffer. Das bedeutet, dass die Daten in der Reihenfolge verarbeitet werden, in der sie empfangen wurden. 
Wenn eine Mail geschrieben wird, wird sie in die FIFO-Reihe eingefügt, und die ältesten Mails werden zuerst gelesen, sobald sie aus der Mailbox entfernt werden.

**Kanal:** Ein Kanal (Channel) ist ein spezifischer Kommunikationsweg innerhalb einer Mailbox. Channels helfen dabei, verschiedene Arten von Daten oder Nachrichten zu kategorisieren und zu verarbeiten. Der Raspberry Pi verwendet verschiedene Channels für unterschiedliche Arten von Datenübertragungen. Die least significant 4 Bits einer Mail geben den Kanal an.
```
Channel:  |  Name:          
   0        Power Management
   1        Framebuffer     
   2        Virtual UART    
   3        VCHIQ           
   4        LEDs            
   5        Buttons         
   6        Touchscreen     
   7           -            
   8        Property Tags   
   9        Property Tags   
```

#### Zweck der Mailbox: Datenstruktur zwischen GPU und CPU austauschen

Die Mailbox ermöglicht es, Zeiger auf Puffer zu übermitteln, wobei die Struktur des Pufferinhalts je nach Kanal unterschiedlich ist. 
Diese unterschiedlichen Datenstrukturen werden von Prozessor und GPU jeweils auf spezifische Weise interpretiert. Ein Puffer ist dabei ein temporärer Speicherbereich, der Daten zwischenspeichert, um sie zwischen verschiedenen Prozessen oder Geräten zu übertragen. Die genaue Struktur des Inhalts hängt vom verwendeten Kanal ab und bestimmt, wie die Daten von den beteiligten Komponenten verarbeitet werden.

|------------------------|-------------------------------|-------------------------|
| [zurück](gpuintro.md)  | [Hauptmenü](../ueberblick.md) | [weiter](kommb.md)      |


|**2.4 Grafik & GPU**                                                       |
|---------------------------------------------------------------------------|
| [2.4.1 Einführung in die Computergrafik & RGB565-Format](grafikintro.md)  |
| [2.4.2 Was ist eine GPU?](gpuintro.md)                                    |
| [2.4.3 Die GPU des BCM2836 VideoCoreIV](gpubcm2836.md)                    |
| [2.4.4 Kommunikation mittels Mailbox](kommb.md)                           |
| [2.4.5 Der Framebuffer und seine Nutzung](framebuff.md)                   |
| [2.4.6 Der Framebuffer-Mailbox-Kanal](framemailb.md)                      |
| [2.4.7 Senden der Initialisierungsnachricht](sendinit.md)                 |