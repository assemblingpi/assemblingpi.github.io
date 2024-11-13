# B.2 Erweiterungen der CPU-Funktionalität
## 2.2.2 Timer: Der ARMv7 Generic Timer

Der Generic Timer in der ARMv7-Architektur ist eine erweiterte Timer-Struktur, die unabhängig von der CPU-Taktfrequenz arbeitet. Das bedeutet, dass der Timer eine konsistente und stabile Zeitbasis bietet, die nicht durch Prozessorzustände wie Taktfrequenzskalierung, Leistungszustand oder Drosselung beeinflusst wird. Diese Eigenschaft macht den Generic Timer besonders nützlich für zeitkritische Anwendungen, bei denen eine veränderliche CPU-Frequenz zu Ungenauigkeiten führen könnte.

### Eigenschaften des Generic Timers beim Cortex A7:

#### 1. Systemzähler (System Counter):
Der Generic Timer verfügt über einen Systemzähler, der kontinuierlich mit einer festen Frequenz inkrementiert wird. Dieser Zähler beginnt bei Null und hat eine Maximalbreite von 64 Bits, was eine sehr hohe Zeitauflösung ermöglicht. Da der Systemzähler unabhängig von der CPU-Frequenz arbeitet, bleibt die Zeitmessung auch bei unterschiedlichen CPU-Taktraten konsistent. Das stellt sicher, dass der Timer als verlässliche Zeitquelle in verschiedensten Szenarien genutzt werden kann.

#### 2. Physische und Virtuelle Zähler:
Der Generic Timer bietet zwei Arten von Countern: Physische und Virtuelle Counter.
- **Physical Counter:** Der physische Counter gibt den tatsächlichen Wert des Systemzählers wieder und kann über das Register `CNTPCT` ausgelesen werden. Er repräsentiert die reale, kontinuierliche Zeit, wie sie vom Systemzähler erfasst wird.
- **Virtual Counter:** Der virtuelle Counter basiert auf dem physischen Zähler, verwendet jedoch einen Offset, der im `CNTVOFF`-Register gespeichert wird, um eine sogenannte virtuelle Zeitbasis zu schaffen. 

#### 3. Timer:
Jeder CPU-Kern im Cortex-A7 verfügt über eine Reihe von Timern, die entweder auf der physischen oder der virtuellen Zeitbasis arbeiten können. Diese Timer lassen sich so konfigurieren, dass sie entweder als Aufwärts- oder Abwärtstimer arbeiten und Interrupts auslösen, wenn bestimmte Bedingungen erfüllt sind. Die Interrupt-Fähigkeit der Timer ist entscheidend für das Timing von Ereignissen in Betriebssystemen, da sie den Prozessor informieren können, dass ein bestimmtes Zeitintervall abgelaufen ist.
Die Timer im Cortex-A7 unterstützen sowohl physische als auch virtuelle Zeitmessungen, was große Flexibilität bei der Verwendung in unterschiedlichen Szenarien bietet. In virtualisierten Systemen können die virtuellen Timer genutzt werden, um den Betriebssystemen in den virtuellen Maschinen eine Zeitbasis zu bieten, die nicht durch die physischen Zustände der Hardware beeinflusst wird.

#### 4. Vorteile des Virtual Timers
Der Virtual Timer wird verwendet, da er insbesondere für die Nutzung durch nicht-sichere Betriebssysteme geeignet ist und einfach zu konfigurieren ist. ([weitere Infos...](https://wiki.osdev.org/ARMv7_Generic_Timers#:~:text=Virtual%20timer.-,The%20virtual%20timer%20should%20be%20used%20by%20non%2Dsecure%20PL1,be%20used%20by%20PL2%20hypervisors))

|----------------------------|------------------------------------|-----------------------------|
|   [zurück](timerintro.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](timerctrl.md)    |


|**2.2 Timer**                                              |
|-----------------------------------------------------------|
| [2.2.1 Intro](timerintro.md)                              |
| [2.2.2 Der ARMv7 Generic Timer](generictimer.md)          |
| [2.2.3 Steuerung des Virtual Timer](timerctrl.md)         |
| [2.2.4 Implementierung einer Delay-Funktion](delay.md)    |