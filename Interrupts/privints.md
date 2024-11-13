# B.1 Einführung
## 1.9.7 Interrupts: Privilegierungslevel und ihre Rolle bei Interrupts

ARMv7-A Prozessoren unterstützen verschiedene Privilegierungslevel wie den "User Mode" und den "Supervisor Mode". Wenn der Prozessor beispielsweise zurückgesetzt wird, startet er im Supervisor-Modus, einem privilegierten Modus, der es ermöglicht, auf alle Systemressourcen zuzugreifen. Jeder Exception-Typ hat einen zugeordneten Modus, und beim Auftreten eines Interrupts wechselt der Prozessor automatisch in den entsprechenden Modus, wie den IRQ-Modus für normale Interrupts, den FIQ-Modus für schnelle Interrupts oder den Supervisor-Modus für Supervisor Calls. Dies stellt sicher, dass die ISR den vollständigen Zugriff auf die notwendigen Ressourcen hat.

Beim Eintreten eines Interrupts wechselt der Prozessor in den dafür vorgesehenen privilegierteren IRQ-Modus, um sicherzustellen, dass der Interrupt-Handler vollständigen Zugriff auf die Systemressourcen hat. Dieser Moduswechsel soll verhindern, dass unprivilegierter Code unerlaubte Operationen ausführt, die das System destabilisieren könnten.


### Verwendung eines separaten Stacks für Interrupts
Jeder Modus, einschließlich des IRQ-Modus, verfügt aus Sicherheitsgründen über einen eigenen Stack, was uns die Architektur dadurch ermöglicht, dass die unterschiedlichen Betriebsmodi eigene Stackpointer besitzen. 
Ein eigener Stack ist im Interruptmodus notwendig, um bei Start der ISR den vorherigen Prozessorzustand zu speichern, bevor die ISR ausgeführt wird. 

#### Aufsetzen eines seperaten Stacks für den IRQ-Modus: 
Folgender Code ist in `boot.s` zu ergänzen!
```asm
@ EQUS für die benötigten Konstanten:
.equ   MODE_MASK,   0x1F
.equ   MODE_USR,    0x10
.equ   MODE_IRQ,    0x12
.equ   MODE_SVC,    0x13
.equ   STACK_IRQ,   0x7000

...

@ setze vector-adresse
    ldr r0, =vector
    mcr p15, #0, r0, c12, c0, 0

@ Wechsel in den Interruptmodus
    MRS r0, cpsr
    MVN r1, #MODE_MASK
    AND r0, r1
    ORR r0, #MODE_IRQ
    MSR cpsr, r0
         
@ Stack für Interruptmodus aufsetzen
    MOV sp, #STACK_IRQ

@ Wechsle zurück in den Supervisor Modus
    MRS r0, cpsr 
    MVN r1, #MODE_MASK
    AND r0, r1
    ORR r0, #MODE_SVC
    MSR cpsr, r0

...

@ Setze Stack-Basis-Adresse
    mov sp, #0x80000

... 
```

|-------------------------|------------------------------------|---------------------------|
|   [zurück](armvekt.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](implirq.md)    |


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