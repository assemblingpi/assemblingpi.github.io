Parameter

Parameter bezeichnen Werte, die an eine Prozedur übergeben werden und Rückgabewerte einer Prozeduren. 

Es gibt verschiedene Methoden, wie einer Prozedur Daten übergeben werden können, man kann ihr die Werte direkt übergeben oder man kann bei der Übergabe per Referenz, ihr einen Zeiger auf die zu verarbeitenden Daten übergeben. Die direkte übergabe von Werten, wählt man dann, wenn es sich um kleine, überschaubare Datenpakete handelt - wenn man jedoch größere Datenmengen an eine Prozedur übergeben möchte, dann bietet sich aus Effizienzgründen die Übergabe per Zeiger an. Bei der Übergabe mittels eines Zeigers muss die Prozedur diesen Zeiger dereferenzieren, um auf die Daten zuzugreifen.
Zudem können diese Parameter an verschiedenen Orten übergeben Werten: In Registern, in globalen Variablen oder im Stack. Wenn eine kleine Anzahl von Parametern übergeben werden soll, bieten sich die Register als Ort der Parameterübergabe an, anderenfalls der Stack. Üblicherweise verwendet man bei ARM nur die ersten 4 General purpose Register zur Parameterübergabe (R0 bis R3) und falls mehr Werte übergeben werden sollen geschieht dies über den Stack. Bei der Übergabe über den Stack muss natürlich beachtet werden, dass geklärt werden muss ob die Aufrufende Funktion oder die Aufgerufene dafür zuständig ist, den Stack wieder auf den zustand vor Aufruf der Prozedur rückzuversetzen, wenn das nämlich nicht geschieht, dann wird es unweigerlich dazu führen, dass das Programm über kurz oder lang crasht. Auf Werte die mittels des Stacks übergeben wurden lässt sich mit einem Offset zum Framepointer (Stichwort Stackframe) zugreifen.


Parameter sind Werte, die an eine Prozedur übergeben werden. Es gibt verschiedene Methoden, wie diese Übergabe erfolgen kann: Entweder werden die Werte direkt übergeben, was sich besonders für kleine Datenpakete eignet, oder es wird ein Zeiger auf die Daten übergeben, was bei größeren Datenmengen aus Effizienzgründen vorteilhaft ist. Dabei muss die Prozedur den Zeiger dereferenzieren, um auf die Daten zuzugreifen.

Parameter können an verschiedenen Orten abgelegt werden, zum Beispiel in Registern, globalen Variablen oder im Stack. Für die Übergabe einer kleinen Anzahl von Parametern werden in ARM-Architekturen üblicherweise die ersten vier General-Purpose-Register (R0 bis R3) verwendet. Sollen mehr Werte übergeben werden, erfolgt dies über den Stack. Hierbei muss sichergestellt werden, dass der Stack nach der Prozedur wieder in seinen ursprünglichen Zustand versetzt wird, um Programmabstürze zu vermeiden. Auf die im Stack abgelegten Parameter kann mit einem Offset zum Framepointer zugegriffen werden, was im Zusammenhang mit dem Stack-Frame steht.



## Parameterübergabe und Rückgabewerte

Prozeduren übernehmen spezifische Aufgaben innerhalb eines Programms und benötigen daher meist Eingabewerte (Parameter) sowie Rückgabewerte. Diese Parameter dienen dazu, Funktionen die notwendigen Daten zur Verarbeitung bereitzustellen und die Ergebnisse nach der Verarbeitung zurückzuerhalten.
Im Assembler gibt es meist spezifische Konventionen und Praktiken, die bei der Übergabe von Parametern beachtet werden müssen.

### Arten der Parameterübergabe
**Direkte Übergabe durch Register:** Bei ARMv7-Assembler werden die ersten vier Parameter einer Funktion typischerweise über die Register R0 bis R3 übergeben. Diese Methode ist effizient, da der Zugriff auf Register schneller ist als auf den Stack. 
Beispiel:
```asm
mov r0, #1       @ 1. Parameter in R0
mov r1, #2       @ 2. Parameter in R1
mov r2, #3       @ 3. Parameter in R2
mov r3, #4       @ 4. Parameter in R3
bl  my_function  @ Aufruf der Funktion
```

**Übergabe per Stack:** Wenn mehr als vier Parameter übergeben werden müssen, werden die zusätzlichen Parameter auf den Stack gelegt. Dies bedeutet, dass die Prozedur auf den Stack zugreifen muss, um die Werte abzurufen. Es ist wichtig, den Stack sorgfältig zu managen, um Stabilitätsprobleme zu vermeiden. Die Verantwortung dafür liegt entweder beim aufrufenden (Caller) oder beim aufgerufenen Code (Callee).
Beispiel:
```asm
Code kopieren
push {r4}        @ 5. Parameter auf den Stack legen
bl  my_function  @ Aufruf der Funktion
add sp, sp, #4   @ Stack aufräumen nach dem Funktionsaufruf durch den Caller
```
**Übergabe per Referenz:** Für größere Datenmengen wird ein Zeiger anstelle der tatsächlichen Daten übergeben. In diesem Fall wird ein Speicheradresszeiger in einem der Register (z.B. R0) übergeben, und die Funktion dereferenziert diesen Zeiger, um auf die Daten zuzugreifen.
Beispiel:
```asm
Code kopieren
ldr r0, =data_address  @ Zeiger auf die Daten in R0 laden
bl  process_data       @ Aufruf der Funktion mit dem Zeiger
```

## Rückgabewerte
Der Rückgabewert einer Funktion wird üblicherweise im Register R0 gespeichert. Wenn die Funktion mehrere Werte zurückgeben muss, könnten Register R1 bis R3 zusätzlich verwendet werden. Ist ein Rückgabewert größer als 32 Bit (z.B. bei 64-Bit-Werten), können zwei Register kombiniert werden, etwa R0 und R1.
Beispiel:
```asm
Code kopieren
mov r0, #result      @ Rückgabewert in R0 setzen
bx  lr               @ Rücksprung zum Aufrufer
```

### Stack-Frame-Management in ARMv7 Assembler

Das Stack-Frame-Management ist ein entscheidender Aspekt bei der Verwendung des Stacks zur Übergabe von Parametern Assembler. 

#### Zugriff auf Parameter im Stack

Parameter, die auf den Stack gelegt wurden, werden relativ zum Frame-Pointer (FP) adressiert. Da die ersten vier Register (R0 bis R3) für die Parameterübergabe verwendet werden, beginnen die Parameter auf dem Stack ab dem Offset `fp + 4`. Auf diese Weise kann auf Parameter und lokale Variablen in einer konsistenten und nachvollziehbaren Weise zugegriffen werden.

Die folgende Tabelle veranschaulicht, wo sich die verschiedenen Elemente im Stack befinden:

| **Stack-Element**          | **Offset zum Frame-Pointer (FP)** | **Beschreibung**                |
|----------------------------|-----------------------------------|---------------------------------|
| Zweiter Parameter          |    r11 + 12                       | 2. Parameter im Stack           |
| Erster Parameter           |    r11 + 8                        | 1. Parameter im Stack           |
| Gespeicherter LR-Wert      |    r11 + 4                        | Rücksprungadresse der Funktion  |
| Alter Framepointer         |    r11                            | ...                             |
| Lokale Variable 1          |    r11 - 4                        | Erste lokale Variable           |
| Lokale Variable 2          |    r11 - 8`                       | Zweite lokale Variable          |
| ...                        | ...                               | ...                             |
| Top of Stack               |    sp                             |                                 |

**Beispiel:**
```asm
ldr r0, [r11, #12]   // Zugriff auf den ersten Parameter (z.B. bei 3 Parametern auf dem Stack)
ldr r1, [r11, #16]   // Zugriff auf den zweiten Parameter
```

Hierbei wird der Offset ab `r11`(Framepointer) so gewählt, dass er die Anzahl der gespeicherten Register berücksichtigt, bevor auf die eigentlichen Parameter zugegriffen wird.


#### Beendigung des Stack-Frames

Am Ende der Funktion müssen der Frame-Pointer und der Stack-Pointer auf ihre ursprünglichen Werte zurückgesetzt werden, bevor zur aufrufenden Funktion zurückgekehrt wird. 

**Beispiel:**
```assembly
mov sp, fp          // Wiederherstellen des Stack-Pointers
pop {fp, lr}        // Wiederherstellen des Frame-Pointers und der Rücksprungadresse
bx  lr              // Rücksprung zur aufrufenden Funktion
```

Das Stack-Frame-Management stellt sicher, dass der Speicher korrekt genutzt und nach der Funktionsausführung bereinigt wird, was essentiell für die Stabilität des Programms ist【18†source】【21†source】【22†source】.














[weiter](param.md)