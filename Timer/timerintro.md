# B.2 Erweiterungen der CPU-Funktionalität
## 2.2.1 Timer: Intro

Ein Timer ist eine Hardwarekomponente, die Zeitintervalle misst, meist durch Zählen in festen Schritten. Sobald der Timer einen vorgegebenen Wert erreicht, löst er ein einen Interrupt aus, der dem Prozessor signalisiert, dass das Zeitintervall abgelaufen ist. Timer arbeiten oft unabhängig von der CPU und sind entscheidend für das Timing von Ereignissen und die Steuerung zeitbasierter Prozesse. 

### Timer-Konfiguration
Um einen Timer zu verwenden, muss er zunächst konfiguriert werden. Diese Konfiguration erfolgt über Kontrollregister, die spezielle Speicherbereiche sind, in denen die Betriebsmodi und Parameter des Timers festgelegt werden. 
Beispiele für Einstellungen in Kontrollregistern sind etwa das Starten oder Stoppen des Timers, die Festlegung des Zählmodus (aufwärts oder abwärts) und das Festlegen von Bedingungen, unter denen der Timer ein Ereignis auslöst.

#### Zielwert setzen
Ein Timer arbeitet oft mit einem Zielwert. Das ist der Wert, den der Timer erreichen muss, um ein Ereignis auszulösen, wie etwa einen Interrupt. Dieser Zielwert wird in einem speziellen Register gespeichert und kann so konfiguriert werden, dass der Timer bei Erreichen dieses Wertes eine Aktion ausführt, beispielsweise das Auslösen eines Signals, das den Prozessor unterbricht.

#### Interrupts und deren Rolle bei Timern
Bei Timern werden Interrupts oft verwendet, um dem Prozessor mitzuteilen, dass eine bestimmte Zeitspanne abgelaufen ist. Dies ermöglicht dem System, auf zeitkritische Ereignisse zu reagieren, wie etwa das Umschalten zwischen verschiedenen Aufgaben oder das Synchronisieren von Abläufen.

#### Lesen und Schreiben von Timerwerten
Während ein Timer läuft, kann es notwendig sein, den aktuellen Zählerstand auszulesen, um festzustellen, wie viel Zeit vergangen ist oder wie viel Zeit noch verbleibt, bevor der Timer sein Ziel erreicht. Manchmal muss man auch die Frequenz des Timers einstellen, also wie schnell der Timer zählt, oder einen Offset angeben, der den Startwert des Timers verändert.

|---------------------------------------|------------------------------------|-------------------------------|
|   [zurück](../Coproc/Coprocessor.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](generictimer.md)   |


|**2.2 Timer**                                              |
|-----------------------------------------------------------|
| [2.2.1 Intro](timerintro.md)                              |
| [2.2.2 Der ARMv7 Generic Timer](generictimer.md)          |
| [2.2.3 Steuerung des Virtual Timer](timerctrl.md)         |
| [2.2.4 Implementierung einer Delay-Funktion](delay.md)    |