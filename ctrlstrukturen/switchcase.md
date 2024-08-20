## Switch-Case

Ein `switch case` ist eine Kontrollstruktur, die es ermöglicht, eine Variable oder einen Ausdruck mit mehreren möglichen Werten zu vergleichen. Basierend auf dem Wert wird der entsprechende Fall (Case) ausgewählt und dessen Anweisungen ausgeführt. Diese Struktur ermöglicht eine effiziente und übersichtliche Handhabung von mehreren Bedingungen im Vergleich zu einer langen Kette von `if-else`-Anweisungen.
```
case(X)
 0: statement 0; break;
 1: statement 1; break;
 2: statement 2; break;
 3: statement 2; break;
```
**Wichtig: X darf nicht größer als Anzahl cases sein, es liegt am Programmierer dies zu gewährleisten!**

### Switch case in Assembler
In Assembler wird die `switch-case`-Struktur durch indirekte Sprünge effizient umgesetzt. Ein indirekter Sprung ist ein Sprungbefehl, bei dem die Zieladresse nicht direkt angegeben ist. Stattdessen wird die Adresse in einem Register oder Speicherort gespeichert, und der Sprung erfolgt zur in diesem Register oder in dem Speicherort angegebenen Adresse. 
Anstelle einer ineffizienten, linearen Reihe von Vergleichen wie bei `if-else`, bei denen jede Bedingung nacheinander geprüft wird, wird bei `switch-case` ein Sprungtabelle verwendet. 
Diese Tabelle enthält Adressen zu den verschiedenen `case`-Labels. Die Implementierung erfolgt typischerweise so:
```
1. Der Wert, der überprüft werden soll, wird als Index verwendet, um den entsprechenden Fall in der Tabelle auszuwählen. Der Index wird verrechnet mit der Breite von Maschinenbefehlen bei ARM (* 4). Das Produkt davon dient als Offset in eine Sprungtabelle
2. Eine Sprungtabelle mit Adressen zu den `case`-Labels wird erstellt.
3. Der Wert wird genutzt, um in der Tabelle die Adresse des entsprechenden Codeblocks zu finden.
4. Ein indirekter Sprung (`BX`) wird verwendet, um direkt zum passenden Codeblock zu springen.
```

#### Beispiel in ARM-Assembler:
```asm 
.data
table: .word case0
                .word case1
                .word case2
                .word case3
```
Eine Sprungtabelle wie diese funktioniert, indem sie eine Liste von Adressen (Sprungziele) enthält, die den Code für verschiedene cases repräsentieren.

```asm 
.text
.start:
MOV r0, #2           @ r0 = X

    @ CASE(X) 
        AND r0, r0, #3
        LDR r1, =table
        LDR r1, [r1, r0, LSL #2]
        BX  r1

case0:
        MOV r2, #1
        B endcase
case1:
        MOV r2, #2
        B endcase
case2:
        MOV r2, #3
        B endcase
case3:
        MOV r2, #4
default:
endcase:
        MOV r0, r2
...
```


