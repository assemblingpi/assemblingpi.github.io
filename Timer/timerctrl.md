# B.2 Erweiterungen der CPU-Funktionalität
## 2.2.3 Timer: Steuerung des Virtual Timer

Um den `Virtual Timer` nutzen zu können, sind mehrere Funktionen zur Überwachung und Steuerung des Timers erforderlich. 
Zunächst muss der Timer aktiviert werden, damit er Zeitintervalle zählen kann. Ein **Zielwert** muss festgelegt werden, bei dessen Erreichen der Timer einen **Interrupt** auslöst. Dieser Interrupt muss aktiviert werden, damit der Prozessor benachrichtigt wird, wenn das Zeitintervall abgelaufen ist. Während der Ausführung des Programms kann es erforderlich sein, den aktuellen Status und den Zählerstand des Timers abzurufen, um zu überprüfen, wie viel Zeit vergangen ist und ob ein Interrupt ausgelöst wurde. Zusätzlich ist es wichtig, den **Offset** des Timers abzurufen zu können, um sicherzustellen, dass der Timer korrekt justiert ist, und die **Zählfrequenz** des Timers auszulesen, um die genaue Geschwindigkeit der Zeitmessung zu kennen. Schließlich muss der Timer deaktiviert werden, wenn er nicht mehr benötigt wird, um Ressourcen freizugeben und unnötige Events zu vermeiden.

Es ist eine Datei namens `k_timer.s` zu erstellen und entsprechende Änderungen in `build.sh` sind vorzunehmen.
Der Inhalt der Sourcedatei ist folgender:

Es ist zu beachten, dass alle Funktionen global definiert sein müssen, damit diese von anderen Funktionen aufgerufen werden können:
```
.global enable_cntv 
.global disable_cntv
.global enable_cntv_irq
.global disable_cntv_irq
.global read_core0_timer_pending
.global read_cntvtv
.global read_cntvct
.global read_cntvoff	  
.global read_cntv_tval
.global write_cntv_tval
.global read_cntfrq

```
Die .equ Direktiven definieren Konstanten, die im Code zur einfachen Referenzierung von Hardware-Registeradressen verwendet werden:

```
.equ CTIMER_CTL,        0x40000000		@ Control register 
.equ C0TIMER_INTCTL,    0x40000040		@ Core0 timers Interrupt control 
.equ C0_IRQSOURCE,      0x40000060
```

- **CTIMER_CTL:**	Control Register für die Timer-Steuerung. Wird verwendet, um den virtuellen Timer zu aktivieren oder zu deaktivieren.
- **C0TIMER_INTCTL:**	Interrupt Control Register der Core0-Timer. Steuert die Aktivierung und Deaktivierung von Timer-Interrupts.
- **C0_IRQSOURCE:**	Interrupt Source Register der Core0-Timer. Dient zum Abfragen, ob ein Timer-Interrupt ausgelöst wurde.


Nun zum eigentlichen code, der wie üblich in der `.section .text` platziert wird:

### 1. Aktivieren des virtuellen Timers:
```
enable_cntv:											
	mov r1, #1
	mcr p15, #0, r1, c14, c3, 1
	bx  lr  
```
### 2. Deaktivieren des virtuellen Timers:
```
disable_cntv:
	mov r1, #0
	mcr p15, #0, r1, c14, c3, 1
	bx  lr
```
### 3. Aktivieren des Timerinterrupts:
```
enable_cntv_irq:
	ldr r0, =C0TIMER_INTCTL
	mov r1, #0x8
	str r1, [r0]
	bx  lr 
```
### 4. Deaktivieren des Timerinterrupts:
```
disable_cntv_irq:
	ldr r0, =C0TIMER_INTCTL
	mov r1, #0x0				@ Setze auf 0, um den Interrupt zu deaktivieren
	str r1, [r0]
	bx lr
```
### 5. Prüfen des Timerinterrupt-Status
```
read_core0_timer_pending:			@ returnwert in r1
	ldr r0, =C0_IRQSOURCE
	ldr r0, [r0]
	bx  lr   
```
### 6. Lesen des aktuellen Timerzählerstandes:
```
read_cntvct:
	mrrc p15, #1, r0, r1, c14		@ r0 low 32 bit r1 high 32 bit
	bx lr
```
### 7. Lesen des Timeroffsets:
```
read_cntvoff:
	mrrc p15, #4, r0, r1, c14		@ r0 low 32 bit r1 high 32 bit
	bx lr
```
### 8. Lesen des verbleibenden Zählerwerts bis zum nächsten Interrupt:
```
read_cntv_tval:
	mrc p15, #0, r0, c14, c3, 0
	bx lr
```
### 9. Festlegen des Zählerwertes bis zum nächsten Interrupt:
```
write_cntv_tval:				@ input in r1
	mcr p15, #0, r1, c14, c3, 0
	bx lr 
```
### 10. Lesen der Zählfrequenz des Timers:
```
read_cntfrq:
	mrc p15, #0, r0, c14, c0, 0
	bx lr
```
Bei Punkt **6** und **7** werden `64`-Bit Register gelesen und dementsprechend müssen **zwei Zielregister** angegeben werden, hier `r0` und `r1`, wobei in `r0` die **least-significant** 32 Bit gespeicher werden und in `r1` die **most-significant** 32 Bit.

|------------------------------|------------------------------------|-------------------------|
|   [zurück](generictimer.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](delay.md)    |


|**2.2 Timer**                                              |
|-----------------------------------------------------------|
| [2.2.1 Intro](timerintro.md)                              |
| [2.2.2 Der ARMv7 Generic Timer](generictimer.md)          |
| [2.2.3 Steuerung des Virtual Timer](timerctrl.md)         |
| [2.2.4 Implementierung einer Delay-Funktion](delay.md)    |