# B.2 Erweiterungen der CPU-Funktionalität
## 2.2.2 Timer: Der ARMv7 Generic Timer

Der "Generic Timer" in der ARMv7-Architektur ist eine erweiterte Timer-Struktur, die unabhängig von der CPU-Taktfrequenz arbeitet. Diese Timer bieten eine konsistente Zeitbasis, die nicht von Prozessorzuständen wie Skalierung, Leistungszustand oder Drosselung beeinflusst wird.

### Eigenschaften des Generic Timers beim Cortex A7:

#### 1. Systemzähler (System Counter):
- Der Generic Timer enthält einen Systemzähler, der kontinuierlich mit einer festen Frequenz inkrementiert wird. Dieser Zähler beginnt bei Null und zählt bis zu  einer Maximalbreite von 64 Bits hoch.
- Dieser Zähler arbeitet unabhängig von der CPU-Frequenz, was bedeutet, dass 
  Zeitmessungen auch bei unterschiedlichen CPU-Taktraten konsistent bleiben.

#### 2. Physische und Virtuelle Zähler:
- Physischer Zähler (Physical Counter): Dieser Zähler gibt den tatsächlichen Wert des Systemzählers wieder und kann über das Register `CNTPCT` ausgelesen werden.  
- Virtueller Zähler (Virtual Counter): Der virtuelle Zähler basiert auf dem 
physischen Zähler, jedoch wird ein Offset (im `CNTVOFF`-Register gespeichert) abgezogen, um eine virtuelle Zeitbasis zu schaffen. Dieser Wert kann über das Register `CNTVCT` ausgelesen werden. Dies ist besonders nützlich in virtualisierten Umgebungen, wo unterschiedliche virtuelle Maschinen ihre eigene, isolierte Zeitmessung benötigen.

#### 3. Timer:
- Jeder CPU-Kern hat eine Reihe von Timern, die entweder auf der physischen oder der virtuellen Zeitbasis arbeiten. Diese Timer können so konfiguriert werden, dass sie entweder als Aufwärts- oder Abwärtstimer arbeiten und Interrupts auslösen, wenn bestimmte Bedingungen erfüllt sind.
- Die Timer unterstützen sowohl physische Zeitmessung als auch virtuelle 
Zeitmessung, was Flexibilität bei der Verwendung in unterschiedlichen Szenarien bietet, wie z.B. in Betriebssystemen und Hypervisoren.



[Für dieses Projekt ist der Virtual Timer die passende Wahl, da er flexibel und einfach zu implementieren ist und gleichzeitig keine unnötige Komplexität oder Hardwareabhängigkeit mit sich bringt.](https://wiki.osdev.org/ARMv7_Generic_Timers#:~:text=Virtual%20timer.-,The%20virtual%20timer%20should%20be%20used%20by%20non%2Dsecure%20PL1,be%20used%20by%20PL2%20hypervisors)

|----------------------------|------------------------------------|-----------------------------|
|   [zurück](timerintro.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](timerctrl.md)    |


|**2.2 Timer**                                              |
|-----------------------------------------------------------|
| [2.2.1 Intro](timerintro.md)                              |
| [2.2.2 Der ARMv7 Generic Timer](generictimer.md)          |
| [2.2.3 Steuerung des Virtual Timer](timerctrl.md)         |
| [2.2.4 Implementierung einer Delay-Funktion](delay.md)    |