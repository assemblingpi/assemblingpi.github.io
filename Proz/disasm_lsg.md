# A.5 Prozedurale Programmierung
## 5.2.7 Was ist eine Prozedur: Lösung zur Übungsaufgabe 

### Lokalisierung der main-Funktion
Der erste Schritt zur Analyse des Programms ist das Auffinden der main-Funktion. Diese Funktion bildet den Ausgangspunkt des Programms und steuert den gesamten Ablauf. Indem wir die main-Funktion untersuchen, erhalten wir einen Überblick über die Struktur und den grundlegenden Ablauf des Codes. 
```
main:
        push {r11, lr}
        mov r11, sp
        sub sp, sp, #120
        mov r0, #0
        str r0, [r11, #-4]
        ldr r0, .LCPI2_0
        bl printf
        ldr r1, .LCPI2_1
        add r2, sp, #16
        str r0, [sp, #8]            
        mov r0, r1
        mov r1, r2
        str r2, [sp, #4]            
        bl scanf
        ldr r1, [sp, #4]            
        str r0, [sp]                
        mov r0, r1
        bl .LBB0_4
        str r0, [sp, #12]
        ldr r0, [sp, #12]
        cmp r0, #0
        bne .LBB2_2
        b .LBB2_1
``` 
In diesem Fall lässt sich die main-Funktion problemlos identifizieren, da sie den Namen `main` trägt. Im nächsten Schritt analysieren wir den Ablauf dieser Funktion, um ihre Funktionsweise besser zu verstehen:

Zunächst wird der Stackframe eingerichtet und Speicherplatz für 30 lokale Variablen auf dem Stack reserviert (120 Bytes, aufgeteilt in 4-Byte-Einheiten):
```
        push {r11, lr}
        mov r11, sp
        sub sp, sp, #120
```
Anschließend wird eine dieser lokalen Variablen auf `0` gesetzt, ein Wert aus dem Konstantenpool in r0 geladen und `printf` aufgerufen:

```
        mov r0, #0
        str r0, [r11, #-4]
        ldr r0, .LCPI2_0
        bl printf
```

Um welchen Wert aus dem Konstantenpool handelt es sich?

```
.LCPI2_0:
        .long   .L.str
```
Mit `ldr r0, .LCPI2_0` wird also die Adresse von `.L.str`in r0 geladen, bevor `printf` aufgerufen wird.
Wofür also steht `.L.str`?

```
.L.str:
        .asciz  "Geben Sie den Schluessel ein: "
```

Der String "Geben Sie den Schluessel ein: " wird ausgegeben – das Programm fordert den Benutzer somit zur Eingabe eines Schlüssels auf. Wie geht es anschließend in der main-Funktion weiter?

```
        ldr r1, .LCPI2_1
```

Hier wird ein weiterer String aus dem Konstantenpool geladen:

```
.LCPI2_1:
        .long   .L.str.1
.L.str.1:
        .asciz  "%s"
```
In diesem Fall liegt ein Formatstring vor, der womöglich für die Ausgabe oder das Einlesen eines weiteren Strings verwendet wird (z.B. durch Funktionen wie printf oder scanf). 

weiter im code
```
        add r2, sp, #16    -> r2 zeigt auf sp + 16
        str r0, [sp, #8]   -> r0 wird auf dem Stack gesichert      
        mov r0, r1         -> Adresse von %s wird in r0 geladen
        mov r1, r2         -> r1 = r2
        str r2, [sp, #4]   -> der Pointer in r2 wird auf dem Stack gesichert
        bl  scanf          -> scanf wird aufgerufen
```
Wir erinnern uns an die Funktionsweise von `scanf`: Der Zeiger auf den Formatstring wird in das Register `r0` geladen, während die Speicheradressen, in die die Benutzereingaben geschrieben werden sollen, in den Registern `r1` bis `r3` übergeben werden. Diese Adressen müssen als Zeiger auf die entsprechenden Variablen übermittelt werden, da `scanf` die eingegebenen Werte direkt an diesen Speicherorten ablegt. Da der Formatstring `"%s"` nur eine Eingabe erwartet, wird die Zieladresse beim Aufruf in `r1` gespeichert. In diesem Fall entspricht `sp + 16` der Speicheradresse des vom Benutzer eingegebenen Strings.

Nach dem Aufruf von `scanf` wird die Benutzereingabe in `r0` geladen und vermutlich in der Funktion `.LBB0_4` weiterverarbeitet:
```
        ldr r1, [sp, #4]   -> r1 zeigt auf den Pointer der zuvor in r2 war         
        str r0, [sp]       -> r0 wird auf dem Stack gespeichert
        mov r0, r1         -> r0 = Adresse der vorigen Benutzereingabe
        bl  .LBB0_4        -> Funktionsaufruf
```
Bevor wir den Programmfluss in dieser Funktion weiterverfolgen, werfen wir einen Blick auf den verbleibenden Teil der main-Funktion.
```
        str r0, [sp, #12]
        ldr r0, [sp, #12]
        cmp r0, #0
        bne .LBB2_2
        b  .LBB2_1
```
Nach der Rückkehr aus `.LBB0_4` wird ein Vergleich mit dem Wert in `r0` durchgeführt, und abhängig vom Ergebnis wird entweder zur Funktion `.LBB2_2` oder `.LBB2_1` gesprungen. Der Sprung zu `.LBB2_2` erfolgt, wenn `r0` ungleich `0` ist, während die Verzweigung nach `.LBB2_1` genommen wird, wenn `r0` gleich `0` ist. Dies ist besonders interessant, da es darauf hinweist, dass in `.LBB0_4` eine Berechnung stattfindet, die den weiteren Verlauf des Programms bestimmt – möglicherweise ein Vergleich mit dem eingegebenen Passwort.

Da diese Funktion eine Berechnung durchführt, die den weiteren Programmverlauf beeinflusst, sollten wir zunächst klären, welche Auswirkungen die verschiedenen Werte von `r0` haben. So können wir besser nachvollziehen, was bei der Analyse von `.LBB0_4` zu erwarten ist.

Was passiert in `.LBB2_1` und  `LBB2_2`?
```
.LBB2_1:
        ldr r0, .LCPI2_3
        bl printf
        b .LBB2_3
.LBB2_2:
        ldr r0, .LCPI2_2
        bl printf
        b .LBB2_3
.LBB2_3:
        mov r0, #0
        mov sp, r11
        pop {r11, lr}
        bx lr
```
Auffällig ist, dass beide Funktionen am Ende zu einer Routine springen, die den zu Beginn von `main` erstellten Stackframe auflöst und `r0` auf `0` setzt. Für jemanden, der mit `C` vertraut ist, deutet dies auf ein `return (0)` hin, was das Ende der main-Funktion markiert.

Beide Funktionen rufen `printf`mit einem String auf. 
```
.LCPI2_2:
        .long   .L.str.3
.LCPI2_3:
        .long   .L.str.2
```

Es handelt sich hierbei um folgende Strings:
```
.L.str.2:
        .asciz  "Success\n"
.L.str.3:
        .asciz  "Error\n"
```

Es scheint also, dass diese Ausgabe nach der Überprüfung der Benutzereingabe erfolgt. Da `"Success\n"` in `.LBB2_1` aufgerufen wird, sollte `r0` nach `.LBB0_4` den Wert `0` haben, wenn das korrekte Passwort eingegeben wurde.

Nun analysieren wir die Funktion `.LBB0_4`, wobei zu beachten ist, dass beim Aufruf ein Zeiger auf die Benutzereingabe in `r0` übergeben wurde.
```
.LBB0_4:
        push {r11, lr}      -> Neuer Stackframe
        mov r11, sp
        sub sp, sp, #16     -> Es wird auf dem Stack Platz für 4 lokale Variablen geschaffen
        str r0, [sp, #8]    -> [sp + 8] beinhaltet den Pointer auf die Benutzereingabe
        ldr r0, [sp, #8]    -> Compilerirrsinn, da keine Optimierungen vorgenommen wurden
        bl .LBB0_0          -> Eine Funktion wird aufgerufen
        mov r0, #10         
        strb r0, [sp, #7]   -> [sp + 7] = 10
        mov r0, #0          -> r0 = 0
        str r0, [sp]        -> [sp] = 0
        b .LBB1_1           -> Eine weitere Funktion wird aufgerufen
```

Da der Wert in `r0` unmittelbar nach dem Aufruf von `.LBB0_0` überschrieben wird, ist eine detaillierte Analyse dieser Funktion nicht notwendig. Der Rückgabewert würde vom C-Compiler ohnehin über `r0` bereitgestellt. Daher ist anzunehmen, dass diese Funktion entweder der Irreführung beim Reverse Engineering dient oder eine Aufgabe erfüllt, die für den weiteren Programmfluss unerheblich ist – es sei denn, sie manipuliert eine globale Variable, die später dazu führt, dass `.LBB0_4 r0 == 0` setzt. Wir wenden uns daher zunächst der Funktion .`LBB1_1` zu.

```
.LBB1_1:                                
        ldr r0, [sp]          -> r0 = 0
        ldr r1, .LCPI1_0      -> Eine im Konstantenpool hinterlegte Adresse wird geladen
        ldrb r0, [r1, r0]     -> Ein Byte von dieser Adresse wird geladen
        cmp r0, #0            -> Es wird geprüft ob das geladene Byte == 0 ist, was das Ende eines nullterminierten Strings markieren würde 
        beq .LBB1_4           -> falls das Byte == 0 -> springe zu .LBB1_4 
        b .LBB1_2             -> falls das Byte != 0 -> springe zu .LBB1_2  
```
Welche Adresse wird hier referenziert?
```
.LCPI1_0:
        .long   .L.str.4
```

Es ist die Adresse von folgendem String:
```
.L.str.4:
        .asciz  "KygZc"
```
Wir wollen diesem String im folgenden einen Namen geben. Nennen wir ihn `weird_str`

Da sein erstes Byte nicht `0` ist, nehmen wir die zweite Abzweigung (`.LBB1_2 `): 
```
.LBB1_2:                                
        ldrb r0, [sp, #7]   -> r0 = 10
        ldr r1, [sp]        -> r1 = 0
        ldr r2, .LCPI1_0    -> Erneut wird die Adresse von weird_str geladen,
        ldrb r3, [r2, r1]   -> r3 = weird_str[0]
        eor r0, r3, r0      -> r0 = K xor 10
        strb r0, [r2, r1]   -> weird_str[0] = r0
        b .LBB1_3
```
Das erste Zeichen des Strings wird mit `10` per XOR verknüpft und ersetzt das ursprüngliche Zeichen. Da der ASCII-Wert von `K` `0x4B` beträgt und `10` in hexadezimaler Darstellung `0x0A` ist, ergibt die XOR-Verknüpfung der binären Werte `0100 1011` und `0000 1010` das Ergebnis `0100 0001`. In Hex entspricht dies `0x41`, was im ASCII-Code dem Buchstaben `A` entspricht.

Betrachten wir den weiteren Programmfluss und werfen einen Blick in `.LBB1_3`
```
.LBB1_3:                               
        ldr r0, [sp]     -> r0 = [sp] = 0 
        add r0, r0, #1   -> r0 = r0 + 1
        str r0, [sp]     -> [sp] = 1
        b  .LBB1_1       -> Sprung zurück zu `.LBB1_1`
```

Bei genauer Betrachtung des Codes erkennen wir, dass die Schleife nur dann beendet wird, wenn in `.LBB1_1` das Ende von `weird_str` erreicht ist. Es lässt sich also schlussfolgern, dass `[sp]` als Zähler dient, der die Iteration über jedes Zeichen des Strings steuert. Während dieser Iteration wird jedes Zeichen mit `0x0A` per XOR verknüpft.

In C-Code würde dies etwa folgendermaßen aussehen:
```
for (int i = 0; weird_str[i] != '\0'; i++) 
    weird_str[i] ^= key;
```
Wir können also nach den entsprechenden Hexcodes für die Asciizeichen `K`,`y`,`g`,`Z` und `c` suchen und erhalten: `0x4b`, `0x79`, `0x67`, `0x5a`, `0x63`.
Wenn wir jedes dieser Zeichen mit `0x0a`ver-xoren erhält man als Resultat den String `AsmPi`. Interessant!  

Was passiert, nachdem das Ende des Strings erreicht ist? Die Zeiger auf die Benutzereingabe und `weird_str` werden in `r0` und `r1` geladen, und die C-Funktion `strcmp` wird aufgerufen:
```
.LBB1_4:
        ldr r0, [sp, #8] -> r0 = Zeiger auf die Benutzereingabe
        ldr r1, .LCPI1_0 -> r1 = Zeiger auf weird_str
        bl  strcmp       
```
Zur Erinnerung: `strcmp` vergleicht zwei Strings, die in `r0` und `r1` übergeben werden. Das Ergebnis wird ebenfalls in `r0` zurückgegeben: Ein Wert von `0` bedeutet, dass die Strings identisch sind, während ein anderer Wert auf eine Abweichung hinweist. Was passiert also, wenn die Benutzereingabe `AsmPi` lautet?
```
        cmp r0, #0
        bne .LBB1_6
        b   .LBB1_5
```
Der Programmfluss würde zu `.LBB1_5` springen. Doch was passiert in dieser Funktion?
```
.LBB1_5:
        mov r0, #0
        str r0, [r11, #-4] -> [r11, #-4] = 0 
        b  .LBB1_7
```
Was passiert in `.LBB1_7`?
```
.LBB1_7:
        ldr r0, [r11, #-4]
        mov sp, r11
        pop {r11, lr}
        bx  lr
```
In `.LBB1_7` wird die Funktion, welche die Benutzereingabe prüft, beendet - sofern die Eingabe `AsmPi` ist und `r0` den Wert `0` hat. Damit haben wir das korrekte Passwort gefunden.



|-----------------------------|------------------------------------|----------------------------------------|
|   [zurück](disasm_ue.md)    |   [Hauptmenü](../ueberblick.md)    |   [weiter](../Advanced/advanced.md)    |


| **5.2 Was Was ist eine Prozedur**                                             |
|-------------------------------------------------------------------------------|
| [5.2.1 Intro](wasistproz.md)                                                  |
| [5.2.2 Der Stack](wasiststack.md)                                             |
| [5.2.3 Was ist der Stackframe einer Prozedur?](wasiststackframe.md)           |
| [5.2.4 Was ist der Calltree?](wasistcalltree.md)                              |
| [5.2.5 Prozeduren, Link Register und Stack](prozlrstack.md)                   |
| [5.2.6 Parameterübergabe und Rückgabewerte](param.md)                         |
| [5.2.7 Finde das geheime Passwort!](disasm_ue.md)                             |