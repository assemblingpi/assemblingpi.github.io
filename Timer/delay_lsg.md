# B.2 Erweiterungen der CPU-Funktionalität
## 2.2.4 Timer: Lösung zur Implementierung einer Delay-Funktion

### Funktionsdeklaration

```
.global delay_ms

.section .text
```

Die Direktive `.global delay_ms` macht die Funktion `delay_ms` global zugänglich, sodass sie von anderen Modulen aufgerufen werden kann. Mit `.section .text` wird angegeben, dass der folgende Code im `.text`-Abschnitt des Programms gespeichert wird, der den ausführbaren Code enthält.

### Funktion `delay_ms`

```
delay_ms:                           @ r1 = delay in ms
    push {lr}
    push {r1}
    cpsid if 
    bl read_cntfrq                  @ r0 = freq
    pop {r1}
    @ Berechne den Zählwert und setze den Timer entsprechend
    ldr r2, =1000
    udiv r0, r0, r2                 @ r0 = freq /1000
    mul  r1, r0, r1                 @ r0 = r0 * n
    bl write_cntv_tval
    bl enable_cntv_irq              @ enable den Interrupt
    bl enable_cntv
    wfi                             @ wait for interrupt
    bl disable_cntv
    cpsie i
    pop {pc}
```

Zunächst werden die Register gesichert und Interrupts deaktiviert, um eine störungsfreie Verzögerung zu gewährleisten. Danach wird die Systemtaktfrequenz ausgelesen, um den Timer-Wert zu berechnen, der die Verzögerungsdauer bestimmt. Anschließend wird der Timer gesetzt und aktiviert. Der Prozessor wartet im `wfi`-Zustand, bis der Timer abläuft und ein Interrupt ausgelöst wird. Nach der Verzögerung wird der Timer deaktiviert, die Interrupts wieder aktiviert, und das Programm kehrt zur aufrufenden Funktion zurück.

|-----------------------|------------------------------------|---------------------------------------------|
|   [zurück](delay.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](../vfpuNeon/floatingintro.md)    |


|**2.2 Timer**                                              |
|-----------------------------------------------------------|
| [2.2.1 Intro](timerintro.md)                              |
| [2.2.2 Der ARMv7 Generic Timer](generictimer.md)          |
| [2.2.3 Steuerung des Virtual Timer](timerctrl.md)         |
| [2.2.4 Implementierung einer Delay-Funktion](delay.md)    |