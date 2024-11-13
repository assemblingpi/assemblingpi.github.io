# B.1 Einführung
## 1.5.3 BCM2836: Ein Überblick auf die ARM-Familien 

Die ARM-Architektur ist in drei Hauptfamilien unterteilt, die jeweils auf unterschiedliche Anwendungen und Leistungsanforderungen zugeschnitten sind:

#### ARM Cortex-A Serie

Die ARM Cortex-A Serie ist für leistungsstarke Anwendungen in Smartphones, Tablets und anderen Geräten konzipiert. Diese Prozessoren bieten erweiterte Befehlssätze wie etwa NEON für SIMD (Single Instruction, Multiple Data), was besonders nützlich für Multimedia-Anwendungen ist. Diese Prozessoren sind ideal für Betriebssysteme und Anwendungen, die hohe Rechenleistung erfordern.

#### ARM Cortex-M Serie

Die ARM Cortex-M Serie richtet sich an eingebettete Systeme und Mikrocontroller. Sie sind für Anwendungen optimiert, bei denen Energieeffizienz und einfache Implementierung wichtig sind. 

#### ARM Cortex-R Serie

Die ARM Cortex-R Serie ist für Echtzeitanwendungen gedacht, bei denen Zuverlässigkeit und deterministische Leistung von entscheidender Bedeutung sind. 

## Der ARM Cortex-A7: Details und Merkmale
Der ARM Cortex-A7, der Prozessor der sich auf dem Raspberry Pi 2 B befindet gehört zur Cortex-A Serie. Er zeichnet sich durch seine In-Order Execution aus, wobei er jedoch bis zu zwei Instruktionen pro Taktzyklus bearbeiten kann. Die 8-Stufen-Pipeline des Cortex-A7 ermöglicht es, verschiedene Phasen der Instruktionsverarbeitung parallel abzuwickeln, was die Gesamtlaufzeit der Instruktionen verkürzt und die Verarbeitungsleistung steigert. 
Ein wesentliches Merkmal des Cortex-A7 ist die NEON SIMD (Single Instruction - Multiple Data) Erweiterung, die es dem Prozessor ermöglicht, mehrere Daten mit nur einer Instruktion gleichzeitig zu verarbeiten. Dies ist besonders nützlich für Multimedia-Anwendungen, da es die Verarbeitung von Audio, Video und anderen datenintensiven Aufgaben erheblich beschleunigt. Zusätzlich verfügt der Prozessor über eine VFPv4 Floating Point Unit, die präzise Fließkommaoperationen unterstützt und für mathematisch komplexe Berechnungen optimiert ist.

|--------------------------|------------------------------------|----------------------------------------------|
|   [zurück](risccisc.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](../privlmodi/privmodiintro.md)    |


|**1.5 BCM2836**                                                |
|---------------------------------------------------------------|
| [1.5.1 Intro](bcm2836.md)                                     |
| [1.5.2 Grundlegende Unterschiede RISC vs. CISC](risccisc.md)  |
| [1.5.3 Ein Überblick auf die ARM-Familien](armfamily.md)      |