## Aufbau und Funktion der Vector Table
Die Vector Table in der ARMv7-A Architektur beginnt bei der Speicheradresse 0 und ordnet verschiedene Speicheradressen den unterschiedlichen Exception-Typen zu:
```
0x00: Reset – Neustart des Kernels nach einem Hardware-Reset.
0x04: Undefined Instruction – Beenden des fehlerhaften Programms bei einem ungültigen
                              Befehl.
0x08: Software Interrupt (SWI) – Ausführen einer privilegierten Operation.
0x0C: Prefetch Abort – Beenden des Programms bei ungültigem Speicherzugriff.
0x10: Data Abort – Beenden des Programms bei ungültigem Datenzugriff.
0x14: Reserved – Keine Aktion, reservierter Bereich.
0x18: Interrupt Request (IRQ) – Verarbeitung eines Interrupts, ausgelöst durch ein
                                Hardwaregerät.
0x1C: Fast Interrupt Request (FIQ) – Schnellere Verarbeitung eines Interrupts mit
                                     hoher Priorität.
```
Jede dieser Adressen ist einem spezifischen Exception-Typ zugeordnet und verweist auf die entsprechenden Routinen zur Verarbeitung der Exceptions. Die Einträge in der Tabelle enthalten Verzweigungen zu den jeweiligen Interrupthandlern. Da die Exception Vector Table jedoch nur eine begrenzte Anzahl von Einträgen umfasst, verweist jeder Eintrag auf eine generische Interrupt-Handler-Routine für den entsprechenden Interrupt-Typ. Diese generische Routine muss dann feststellen, welcher spezifische Interrupt tatsächlich aufgetreten ist. Das bedeutet, dass die Vektortabelle selbst lediglich anzeigt, dass ein Interrupt einer bestimmten Art eingetreten ist, jedoch nicht, welcher spezifische Interrupt es genau war.

#### Beispiel für die Implementierung der Interrupt-Vektor Tabelle:
```asm
    vector:
    ldr pc, reset_handler
    ldr pc, undefined_handler
    ldr pc, swi_handler
    ldr pc, prefetch_handler
    ldr pc, data_handler
    ldr pc, unused_handler
    ldr pc, irq_handler
    ldr pc, fiq_handler

        
reset_handler:      .word reset
undefined_handler:  .word hang
swi_handler:        .word hang
prefetch_handler:   .word hang
data_handler:       .word hang
unused_handler:     .word hang
irq_handler:        .word irq
fiq_handler:        .word hang
 


reset:
                b                start
                b                .

@ Timerinterrupt
irq:
                
                cpsid i                        @ interrupts ausmaskieren
                push {r0-r3, r12, lr}          @ speichere Prozessorstatus
                bl irq_handler_ext             @ springe zu Extended Interrupt Handler
                pop {r0-r3, r12, lr}           @ prozessorstatus wiederherstellen
                cpsie i                        @ interrupts werden wieder durchgelassen
                sub pc, lr, #4                 @ returnadresse anpassen

@ Dauerschleife        bei nicht implementierten Interrupts
hang:
                wfi
                b hang
```

|---------------------------|------------------------------------|----------------------------|
|   [zurück](raspiints.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](privints.md)    |
