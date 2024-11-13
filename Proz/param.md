# A.5 Prozedurale Programmierung
## 5.2.6 Was ist eine Prozedur: Parameterübergabe und Rückgabewerte

Prozeduren übernehmen spezifische Aufgaben innerhalb eines Programms und benötigen daher meist Eingabewerte (Parameter) sowie Rückgabewerte. Diese Parameter dienen dazu, Funktionen die notwendigen Daten zur Verarbeitung bereitzustellen und die Ergebnisse nach der Verarbeitung zurückzuerhalten.
Im Assembler gibt es meist spezifische Konventionen und Praktiken, die bei der Übergabe von Parametern beachtet werden müssen.

### Arten der Parameterübergabe
**Direkte Übergabe durch Register:** Bei ARMv7-Assembler werden die ersten vier Parameter einer Funktion typischerweise über die Register R0 bis R3 übergeben. Diese Methode ist effizient, da der Zugriff auf Register schneller ist als auf den Stack. 
Beispiel:
```asm
mov r0, #1          @ 1. Parameter in R0
mov r1, #2          @ 2. Parameter in R1
mov r2, #3          @ 3. Parameter in R2
mov r3, #4          @ 4. Parameter in R3
bl  my_function     @ Aufruf der Funktion
```

**Übergabe per Stack:** Wenn mehr als vier Parameter übergeben werden müssen, werden die zusätzlichen Parameter auf den Stack gelegt. Dies bedeutet, dass die Prozedur auf den Stack zugreifen muss, um die Werte abzurufen. Es ist wichtig, den Stack sorgfältig zu managen, um Stabilitätsprobleme zu vermeiden. Die Verantwortung dafür liegt entweder beim aufrufenden (Caller) oder beim aufgerufenen Code (Callee).
Beispiel:
```asm
push {r4}           @ 5. Parameter auf den Stack legen
bl  my_function     @ Aufruf der Funktion
add sp, sp, #4      @ Stack aufräumen nach dem Funktionsaufruf durch den Caller
```

**Übergabe per Referenz:** Für größere Datenmengen wird ein Zeiger anstelle der tatsächlichen Daten übergeben. In diesem Fall wird ein Speicheradresszeiger in einem der Register (z.B. R0) übergeben, und die Funktion dereferenziert diesen Zeiger, um auf die Daten zuzugreifen.
Beispiel:
```asm
ldr r0, =data_address  @ Zeiger auf die Daten in R0 laden
bl  process_data       @ Aufruf der Funktion mit dem Zeiger
```

### Stack-Frame-Management in ARMv7 Assembler
Das Stack-Frame-Management ist ein entscheidender Aspekt bei der Verwendung des Stacks zur Übergabe von Parametern Assembler. 

### Zugriff auf Parameter im Stack

Parameter, die auf den Stack gelegt wurden, werden relativ zum Frame-Pointer (`r11`) adressiert. Da Rücksprungadresse und der vorige Framepointer-Wert ebenfalls auf dem Stack gesichert werden, beginnen die Parameter auf dem Stack ab dem Offset `r11 + 4`. Auf diese Weise kann auf Parameter und lokale Variablen in einer konsistenten und nachvollziehbaren Weise zugegriffen werden.

Die folgende Tabelle veranschaulicht, wo sich die verschiedenen Elemente im Stack befinden:

| **Stack-Element**          | **Offset zum Frame-Pointer (FP)** | **Beschreibung**                                              |
|----------------------------|-----------------------------------|---------------------------------------------------------------|
| Zweiter Parameter          |    r11 + 12                       | 2. Parameter im Stack                                         |
| Erster Parameter           |    r11 + 8                        | 1. Parameter im Stack                                         |
| Gespeicherter LR-Wert      |    r11 + 4                        | Rücksprungadresse der Funktion                                |
| Alter Framepointer         |    r11                            | gespeicherter Zeiger auf den Boden des vorherigen Stackframes |
| Lokale Variable 1          |    r11 - 4                        | Erste lokale Variable                                         |
| Lokale Variable 2          |    r11 - 8`                       | Zweite lokale Variable                                        |
| ...                        |    ...                            | ...                                                           |
| Top of Stack               |    sp                             | letzter auf den Stack gepushter Wert                          |

**Beispiel:**
```asm
ldr r0, [r11, #12]   @ Zugriff auf den ersten Parameter (z.B. bei 3 Parametern auf dem Stack)
ldr r1, [r11, #16]   @ Zugriff auf den zweiten Parameter
```

Hierbei wird der Offset ab `r11` so gewählt, dass er die Anzahl der gespeicherten Register berücksichtigt, bevor auf die eigentlichen Parameter zugegriffen wird.

### Rückgabewerte
Der Rückgabewert einer Funktion wird üblicherweise im Register R0 gespeichert. Wenn die Funktion mehrere Werte zurückgeben muss, könnten Register R1 bis R3 zusätzlich verwendet werden. Ist ein Rückgabewert größer als 32 Bit (z.B. bei 64-Bit-Werten), können zwei Register kombiniert werden, etwa R0 und R1.
Beispiel:
```asm
mov r0, #result      @ Rückgabewert in R0 setzen
bx  lr               @ Rücksprung zum Aufrufer
```

|-----------------------------|------------------------------------|----------------------------------------|
|   [zurück](prozlrstack.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](disasm_ue.md)               |


| **5.2 Was Was ist eine Prozedur**                                             |
|-------------------------------------------------------------------------------|
| [5.2.1 Intro](wasistproz.md)                                                  |
| [5.2.2 Der Stack](wasiststack.md)                                             |
| [5.2.3 Was ist der Stackframe einer Prozedur?](wasiststackframe.md)           |
| [5.2.4 Was ist der Calltree?](wasistcalltree.md)                              |
| [5.2.5 Prozeduren, Link Register und Stack](prozlrstack.md)                   |
| [5.2.6 Parameterübergabe und Rückgabewerte](param.md)                         |
| [5.2.7 Finde das geheime Passwort!](disasm_ue.md)                             |