### Analyse und Erklärung der Funktion `delay_ms` in ARM-Assembly

Der gegebene Code implementiert die Funktion `delay_ms`. 

---

### Funktionsdeklaration

```assembly
.global delay_ms

.section .text
```

- **`.global delay_ms`**: Diese Direktive macht die Funktion `delay_ms` global zugänglich, sodass sie von anderen Modulen oder Dateien aus aufgerufen werden kann.
- **`.section .text`**: Der folgende Code befindet sich im `.text`-Abschnitt, der den ausführbaren Code des Programms enthält.

---

### Funktion `delay_ms`

```asm
delay_ms:                           @ r1 = delay in ms
    push {lr}
    push {r1}
    cpsid if 
    bl read_cntfrq                  @ r0 = freq
    pop {r1}
    @ Berechne den Zählwert und setze den Timer entsprechend
    ldr r2, =1000
    udiv r0, r0, r2                 @ r0 = freq /1000
    mul r0, r0, r1                  @ r0 = r0 * n
    udiv r1, r0, r2
    bl write_cntv_tval
    bl enable_cntv_irq              @ enable den Interrupt
    bl enable_cntv
    wfi                             @ wait for interrupt
    bl disable_cntv
    cpsie i
    pop {pc}
```

#### Schritt-für-Schritt-Erklärung

1. **Sichern der Register und Deaktivieren von Interrupts**

    ```asm
    push {lr}
    push {r1}
    cpsid if 
    ```

    - **push {lr}:** Sichert das Link-Register (`lr`) auf dem Stack, um die Rücksprungadresse zu speichern.
    - **push {r1}:** Sichert das Register `r1`, das die Verzögerungsdauer in Millisekunden enthält.
    - **cpsid if:** Deaktiviert sowohl Interrupt Request als auch Fast Interrupt Request, um zu verhindern, dass Interrupts während der Verzögerung auftreten.

2. **Lesen der Systemtaktfrequenz**

    ```asm
    bl read_cntfrq                  @ r0 = freq
    ```

    - **bl read_cntfrq**: Führt einen Branch mit Link zu der Funktion `read_cntfrq` aus, die die aktuelle Zählerfrequenz in das Register `r0` lädt.

3. **Wiederherstellen von `r1`**

    ```asm
    pop {r1}
    ```

    - **pop {r1}**: Stellt den vorher gesicherten Wert von `r1` wieder her, der die gewünschte Verzögerungsdauer in Millisekunden enthält.

4. **Berechnung des Timer-Werts**

    ```asm
    @ Berechne den Zählwert und setze den Timer entsprechend
    ldr r2, =#1000
    udiv r0, r0, r2                 @ r0 = freq /1000
    mul r0, r0, r1                  @ r0 = r0 * n
    udiv r1, r0, r2
    ```
5. **Setzen des Timer-Werts und Aktivieren des Timers**

    ```asm
    bl write_cntv_tval
    bl enable_cntv_irq              @ enable den Interrupt
    bl enable_cntv
    ```

    - **bl write_cntv_tval**: Ruft die Funktion `write_cntv_tval` auf, die den berechneten Zählwert in das Timer-Register schreibt.
    - **bl enable_cntv_irq**: Aktiviert den Timer-Interrupt, sodass ein Interrupt ausgelöst wird, wenn der Timer abläuft.
    - **bl enable_cntv**: Aktiviert den Timer, damit dieser zu zählen beginnt.

6. **Warten auf den Interrupt**

    ```asm
    wfi                             @ wait for interrupt
    ```

    - **wfi**: Steht für `Wait For Interrupt`. Der Prozessor geht in einen wartenden Zustand und hält an, bis ein Interrupt eintritt. Dies ermöglicht eine effiziente Verzögerung, ohne dass der Prozessor unnötig beschäftigt wird.

7. **Deaktivieren des Timers und Wiederaktivieren von Interrupts**

    ```asm
    bl disable_cntv
    cpsie i
    pop {pc}
    ```

    - **bl disable_cntv**: Ruft die Funktion `disable_cntv` auf, die den Timer deaktiviert, nachdem die Verzögerung abgeschlossen ist.
    - **cpsie i**: Reaktiviert IRQ-Interrupts, sodass das System wieder auf Interrupts reagieren kann.
    - **pop {pc}**: Stellt den Programmzähler (`pc`) wieder her, was effektiv einen Rücksprung zur aufrufenden Funktion bewirkt, indem das zuvor gespeicherte Link-Register (`lr`) geladen wird.

|-----------------------|------------------------------------|---------------------------------------------|
|   [zurück](delay.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](../vfpuNeon/floatingintro.md)    |

