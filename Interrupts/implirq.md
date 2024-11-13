# B.1 Einführung
## 1.9.8 Interrupts: Implementierung eines IRQ-Handlers

Ein typischer IRQ-Handler in ARMv7-A speichert den aktuellen Zustand des Prozessors, identifiziert den Interrupt, führt die notwendige Verarbeitung durch und stellt anschließend den vorherigen Zustand wieder her. 

### Prozessorzustand speichern
Nachdem ein Interrupthandler die Ausführung beendet hat, sollen alle allgemeinen Register wieder die gleichen Werte haben, wie sie vor Auftreten des Interrupts hatten. Wenn wir eine solche Funktionalität nicht implementieren, kann ein Interrupt, der nichts mit dem aktuell ausgeführten Code zu tun hat, das Verhalten dieses Codes unvorhersehbar beeinflussen. Deshalb ist es wichtig, dass wir als erstes nach der Generierung einer Ausnahme den aktuellen Prozessorzustand auf dem Stack speichern. Das stellt sicher, dass der Prozessor nach der ISR in einem konsistenten Zustand bleibt und den normalen Programmablauf korrekt fortsetzen kann.

### Masking von Interrupts
Masking bedeutet das Deaktivieren von Interrupts, um sicherzustellen, dass kritische Codeabschnitte, nicht durch asynchrone Interrupts unterbrochen werden. 
Das verhindert etwa, dass der Prozessorzustand verloren geht. Während ein Interrupthandler läuft, werden Interrupts automatisch maskiert. 

#### Für diesen Zweck gibt es zwei Befehle:

- cpsid i: Deaktiviert (maskiert) Interrupts.
- cpsie i: Aktiviert   (demaskiert) Interrupts

#### Ein Beispiel für einen generischen IRQ-Handler:
Ein Ausschnitt aus dem bereits zuvor gezeigten Code `vector.s`: 
```asm
irq:
    cpsid i                        @ Interrupts maskieren
    push {r0-r3, r12, lr}          @ Prozessorstatus sichern
    bl irq_handler_ext             @ Erweiterte IRQ-Bearbeitung
    pop {r0-r3, r12, lr}           @ Prozessorstatus wiederherstellen
    cpsie i                        @ Interrupts wieder aktivieren
    sub pc, lr, #4                 @ Rücksprungadresse anpassen
```
Die erweiterte Interruptbehandlung (irq_handler_ext) findet an anderer Stelle statt, prüft die genaue Ursache des Interrupts und leitet die nötigen Schritte zu seiner Abarbeitung ein. 

|--------------------------|------------------------------------|-----------------------------------------|
|   [zurück](privints.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](../Coproc/Coprocessor.md)    |


|**1.9 Interrupts**                                                             |
|-------------------------------------------------------------------------------|
| [1.9.1 Was sind Interrupts?](intintro.md)                                     |
| [1.9.2 Die Interruptvektortabelle](ivektable.md)                              |
| [1.9.3 Die Interrupt Service Routine/ der Interrupt-Handler](ihandler.md)     |
| [1.9.4 Der Interruptcontroller](ictrl.md)                                     |
| [1.9.5 Interrupts im Raspberry Pi 2B](raspiints.md)                           |
| [1.9.6 Aufbau und Funktion der Vector Table](armvekt.md)                      |
| [1.9.7 Privilegierungslevel und ihre Rolle bei Interrupts](privints.md)       |
| [1.9.8 Implementierung eines IRQ-Handlers](implirq.md)                        |