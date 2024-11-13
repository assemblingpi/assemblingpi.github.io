# B.2 Erweiterungen der CPU-Funktionalität
## 2.2.4 Timer: Übungsaufgabe Implementierung einer Delay-Funktion

#### Hintergrund

In eingebetteten Systemen, ist es oft notwendig, präzise **Zeitverzögerungen** zu implementieren. Solche Verzögerungen können für verschiedene Zwecke verwendet werden, beispielsweise zum Warten auf Sensorantworten, zur Steuerung von Timern oder zur Synchronisierung von Prozessen.

#### Aufgabenbeschreibung

Implementieren Sie eine ARM-Assembly-Funktion namens `delay_ms`, die eine Verzögerung in Millisekunden (ms) erzeugt. `delay_ms` soll die Funktionen zur Steuerung des virtuellen Timers, die wir zuvor implementiert hatten - verwenden. 

##### Anforderungen:
- Die Funktion `delay_ms` soll als global deklariert sein, damit sie von anderen Modulen aufgerufen werden kann.
- Der Eingabeparameter (Verzögerungsdauer in Millisekunden) wird im Register `r1` übergeben.

|---------------------------|------------------------------------|-----------------------------|
|   [zurück](timerctrl.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](delay_lsg.md)    |


|**2.2 Timer**                                              |
|-----------------------------------------------------------|
| [2.2.1 Intro](timerintro.md)                              |
| [2.2.2 Der ARMv7 Generic Timer](generictimer.md)          |
| [2.2.3 Steuerung des Virtual Timer](timerctrl.md)         |
| [2.2.4 Implementierung einer Delay-Funktion](delay.md)    |