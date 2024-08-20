## Steuerung des Virtual Timer

Um den Virtual Timer nutzen zu können, sind mehrere Funktionen zur Überwachung und Steuerung des Timers erforderlich. 
Zunächst muss der Timer aktiviert werden, damit er Zeitintervalle zählen kann. Ein Zielwert muss festgelegt werden, bei dessen Erreichen der Timer einen Interrupt auslöst. Dieser Interrupt muss aktiviert werden, damit der Prozessor benachrichtigt wird, wenn das Zeitintervall abgelaufen ist. Während der Ausführung des Programms kann es erforderlich sein, den aktuellen Status und den Zählerstand des Timers abzurufen, um zu überprüfen, wie viel Zeit vergangen ist und ob ein Interrupt ausgelöst wurde. Zusätzlich ist es wichtig, den Offset des Timers abzurufen zu können, um sicherzustellen, dass der Timer korrekt justiert ist, und die Zählfrequenz des Timers auszulesen, um die genaue Geschwindigkeit der Zeitmessung zu kennen. Schließlich muss der Timer deaktiviert werden, wenn er nicht mehr benötigt wird, um Ressourcen freizugeben und unnötige Events zu vermeiden.



#### 1. Aktivieren des virtuellen Timers:
```asm
enable_cntv:                                            
          mov r1, #1
          mcr p15, #0, r1, c14, c3, 1
          bx  lr  
```

#### 2. Deaktivieren des virtuellen Timers:
```asm
disable_cntv:
          mov r1, #0
          mcr p15, #0, r1, c14, c3, 1
          bx  lr
```

#### 3. Aktivieren des Timer-Interrupts:
```asm
enable_cntv_irq:
          ldr r0, =C0TIMER_INTCTL
          mov r1, #0x8
          str r1, [r0]
          bx  lr 
```

#### 4. Lesen des Interruptstatus:
```asm
read_core0_timer_pending:                                      
          ldr r0, =C0_IRQSOURCE
          ldr r0, [r0]
          bx  lr  
```

#### 5. Lesen des aktuellen Zählerstands:
```asm
read_cntvct:
          mrrc p15, #1, r0, r1, c14                             
          bx lr
```

#### 6. Lesen des Timer-Offsets:
```asm
read_cntvoff:
          mrrc p15, #4, r0, r1, c14                             
          bx lr
```

#### 7. Lesen des verbleibenden Zählwertes bis zum nächsten Interrupt:
```asm
read_cntv_tval:
          mrc p15, #0, r0, c14, c3, 0
          bx lr
```

#### 8. Setzen des Zählwertes bis zum nächsten Interrupt:
```asm
write_cntv_tval:                                               
          mcr p15, #0, r1, c14, c3, 0
          bx lr 
```
#### 9. Auslesen der Timerfrequenz:
```asm
read_cntfrq:
          mrc p15, #0, r0, c14, c0, 0
          bx lr
```
Bei Punkt **5** und **6** werden 64-Bit Register gelesen und dementsprechend müssen zwei Zielregister angegeben werden, hier r0 und r1, wobei in r0 die least-significant 32 Bit gespeicher werden und in r1 die most significant 32 Bit.
