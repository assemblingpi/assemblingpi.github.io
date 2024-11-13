# A.5 Prozedurale Programmierung
## 5.2.5 Was ist eine Prozedur: Prozeduren, Link Register und Stack

Bei der Programmierung in Assembler ist es in diesem Kontext besonders wichtig, das Link Register korrekt zu verwalten. 
Das LR-Register enthält die Rücksprungadresse, also die Adresse, zu der die Ausführung zurückkehren soll, nachdem eine Prozedur beendet ist. Die Handhabung dieses Registers ist entscheidend für das korrekte Funktionieren von Prozeduren und deren Aufrufen.

### Non-Leaf Prozeduren und Link Register
Wenn eine Non-Leaf-Prozedur eine andere Prozedur aufruft, muss die Rücksprungadresse, die im LR-Register gespeichert ist, gesichert werden, bevor der neue Aufruf stattfindet. Dies liegt daran, dass der Aufruf einer weiteren Prozedur das LR-Register überschreiben wird, da der Prozessor die Rücksprungadresse für die neu aufgerufene Prozedur dort speichert. Um die ursprüngliche Rücksprungadresse nicht zu verlieren, wird der aktuelle Wert des LR-Registers vor dem weiteren Aufruf auf dem Stack gespeichert. Dies sorgt dafür, dass nach dem Beenden der inneren Prozedur die Ausführung an der richtigen Stelle im Programm fortgesetzt wird.

#### Beispiel für den Ablauf bei einer Non-Leaf Prozedur:
```asm
Prolog:
    PUSH {lr}       @ Speichern der Rücksprungadresse
    PUSH {r11}      @ Speichern des alten Frame-Pointers
    MOV  r11, sp

Body:
    ...

Epilog:
    MOV sp, r11      
    POP {r11}        @ Wiederherstellen des alten Frame-Pointers
    POP  {lr}        @ Wiederherstellung der Rücksprungadresse
    BX lr            @ Rückkehr aus der Funktion
```

### Leaf Prozeduren und Link Register
Eine Leaf-Prozedur hingegen ruft keine weiteren Prozeduren auf. Da diese Prozeduren keine weiteren Rücksprungadressen speichern müssen, ist es nicht erforderlich, das LR-Register auf dem Stack zu sichern. 

#### Beispiel für den Ablauf bei einer Leaf Prozedur:
```asm
Prolog:
    PUSH {r11}      @ Sichern des alten Frame-Pointers
    MOV  r11, sp

Body: 
    ...
Epilog:
    MOV sp, r11  
    POP {r11}       @ Wiederherstellen des alten Frame-Pointers
    BX lr           @ Rückkehr zur aufrufenden Funktion
```

|--------------------------------|------------------------------------|-------------------------|
|   [zurück](wasistcalltree.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](param.md)    |


| **5.2 Was Was ist eine Prozedur**                                             |
|-------------------------------------------------------------------------------|
| [5.2.1 Intro](wasistproz.md)                                                  |
| [5.2.2 Der Stack](wasiststack.md)                                             |
| [5.2.3 Was ist der Stackframe einer Prozedur?](wasiststackframe.md)           |
| [5.2.4 Was ist der Calltree?](wasistcalltree.md)                              |
| [5.2.5 Prozeduren, Link Register und Stack](prozlrstack.md)                   |
| [5.2.6 Parameterübergabe und Rückgabewerte](param.md)                         |
| [5.2.7 Finde das geheime Passwort!](disasm_ue.md)                             |