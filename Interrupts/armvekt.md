# B.1 Einführung
## 1.9.6 Interrupts: Aufbau und Funktion der Vector Table
Die Vector Table in der ARMv7-A Architektur beginnt bei der Speicheradresse 0 und ordnet verschiedene Speicheradressen den unterschiedlichen Exception-Typen zu:

- **0x00:** Reset – Neustart des Kernels nach einem Hardware-Reset.
- **0x04:** Undefined Instruction – Beenden des fehlerhaften Programms bei einem ungültigen Befehl.
- **0x08:** Software Interrupt (SWI) – Ausführen einer privilegierten Operation.
- **0x0C:** Prefetch Abort – Beenden des Programms bei ungültigem Speicherzugriff.
- **0x10:** Data Abort – Beenden des Programms bei ungültigem Datenzugriff.
- **0x14:** Reserved – Keine Aktion, reservierter Bereich.
- **0x18:** Interrupt Request (IRQ) – Verarbeitung eines Interrupts, ausgelöst durch ein Hardwaregerät.
- **0x1C:** Fast Interrupt Request (FIQ) – Schnellere Verarbeitung eines Interrupts mit hoher Priorität.

Jede dieser Adressen ist einem spezifischen Exception-Typ zugeordnet und verweist auf die entsprechenden Routinen zur Verarbeitung der Exceptions. Die Einträge in der Tabelle enthalten Verzweigungen zu den jeweiligen Interrupthandlern. Da die Exception Vector Table jedoch nur eine begrenzte Anzahl von Einträgen umfasst, verweist jeder Eintrag auf eine generische Interrupt-Handler-Routine für den entsprechenden Interrupt-Typ. Diese generische Routine muss dann feststellen, welcher spezifische Interrupt tatsächlich aufgetreten ist. Das bedeutet, dass die Vektortabelle selbst lediglich anzeigt, dass ein Interrupt einer bestimmten Art eingetreten ist, jedoch nicht, welcher spezifische Interrupt es genau war.

#### Beispiel für die Implementierung der Interrupt-Vektor Tabelle:
Es muss eine weitere Quelldatei namens `vector.s` erstellt werden, und die entsprechenden Kommandos zum Assemblieren und Linken müssen anschließend in die `build.sh`-Datei eingefügt werden. 

Die Vektor-Tabelle definiert die Adressen der Handler für verschiedene Ausnahmen. Jeder Eintrag lädt die Adresse des entsprechenden Handlers in das Programmzähler-Register (pc), was einen Sprung zur Handler-Funktion bewirkt:
```asm
.section .text
vector:
    ldr pc, reset_handler
    ldr pc, undefined_handler
    ldr pc, swi_handler
    ldr pc, prefetch_handler
    ldr pc, data_handler
    ldr pc, unused_handler
    ldr pc, irq_handler
    ldr pc, fiq_handler
```

- **reset_handler:** Wird aufgerufen, wenn das System neu gestartet wird.
- **undefined_handler:** Handhabt undefinierte Instruktionsausnahmen.
- **swi_handler:** Handhabt Software-Interrupts (SWI).
- **prefetch_handler:** Handhabt Prefetch-Ausnahmen, die bei fehlerhaften Instruktionsvorabrufen auftreten.
- **data_handler:** Handhabt Daten-Ausnahmen, die bei fehlerhaften Datenzugriffen auftreten.
- **unused_handler:** Reserviert für nicht verwendete Vektoren.
- **irq_handler:** Handhabt IRQ (Interrupt Request) Interrupts.
- **fiq_handler:** Handhabt FIQ (Fast Interrupt Request) Interrupts.


Jeder Handler wird als ein Wort definiert, das auf die entsprechende Funktion zeigt:

```
        
reset_handler:      .word reset
undefined_handler:  .word hang
swi_handler:        .word hang
prefetch_handler:   .word hang
data_handler:       .word hang
unused_handler:     .word hang
irq_handler:        .word irq
fiq_handler:        .word hang
```

- **reset_handler:** Verweist auf die reset-Funktion.
- **undefined_handler, swi_handler, prefetch_handler, data_handler, unused_handler, fiq_handler:** Verweisen alle auf die hang-Funktion, was bedeutet, dass bei diesen Ausnahmen das System in einer Endlosschleife verbleibt.
- **irq_handler:** Verweist auf die irq-Funktion, die für die Behandlung von IRQ-Interrupts zuständig ist.

Die reset-Funktion ist der Einstiegspunkt nach einem System-Reset:
```
reset:
    b   start
```
`Reset` führt einen unbedingten Sprung (b) zur start-Funktion aus, die den eigentlichen Initialisierungsprozess übernimmt.

Die irq-Funktion behandelt IRQ-Interrupts, in unserem Fall werden wir nur einen Timerinterrupt implementieren:
```
@ Timerinterrupt
irq:
    cpsid i                        @ interrupts ausmaskieren
    push {r0-r3, r12, lr}          @ speichere Prozessorstatus
    bl irq_handler_ext             @ springe zu Extended Interrupt Handler
    pop {r0-r3, r12, lr}           @ prozessorstatus wiederherstellen
    sub pc, lr, #4                 @ returnadresse anpassen
```
1. **cpsid i:** Deaktiviert weitere Interrupts, um die kritische Sektion vor gleichzeitigen Unterbrechungen zu schützen.
2. **push {r0-r3, r12, lr}:** Sichert die Register `r0` bis `r3`, `r12` und das `Link-Register` (lr) auf dem Stack, um den aktuellen Zustand zu bewahren.
3. **bl irq_handler_ext:** Ruft die erweiterte Interrupt-Handler-Funktion `irq_handler_ext` auf, welche die spezifische Interrupt-Verarbeitung (des Timer-Interrupts) übernimmt.
4. **pop {r0-r3, r12, lr}:** Stellt die zuvor gesicherten Register wieder her.
5. **sub pc, lr, #4:** Korrigiert die Rücksprungadresse, um zur aufrufenden Funktion zurückzukehren. 

Die `hang`-Funktion wird für nicht implementierte oder unerwartete Ausnahmen verwendet. Sie sorgt dafür, dass das System in einer Endlosschleife verbleibt:
```
@ Dauerschleife bei nicht implementierten Interrupts
hang:
    wfi
    b hang
```

|---------------------------|------------------------------------|----------------------------|
|   [zurück](raspiints.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](privints.md)    |


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