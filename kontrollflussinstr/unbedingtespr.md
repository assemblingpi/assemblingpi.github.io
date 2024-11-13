# A.3 Verknüpfungen von Basic Blocks
## 3.1.2 Kontrollflussinstruktionen: Unbedingte Sprünge

### B 
Unbedingte Sprünge, wie der `B`-Befehl (Branch), ändern den Programmfluss immer zu einer angegebenen Adresse, ohne Rücksicht auf etwaige Bedingungen. Das bedeutet, dass das Programm immer zu der angegebenen Stelle im Code springt, wenn dieser Befehl ausgeführt wird.

#### Syntax
```
B <label>
```
Hierbei ist `label` der Block, zu dem das Programm springen soll.

#### Beispiel
```
	MOV R0, #3			@ Speichere die Zahl 3 in Register R0
	MOV R1, #4			@ Speichere die Zahl 4 in Regster R1
	B addierer			@ Spring in Block "addierer"
	MOV R3, #5			@ Dieser Befehl wird übersprungen
	
	
addierer:
	ADD R2, R0, R1			@ Addiere die Werte 3 (R0) und 4 (R1) auf, speichere das Ergebnis in R2
```

In diesem Beispiel wird die Anweisung `MOV R3, #5` nicht ausgeführt, weil der Befehl `B addierer` die Ausführung des Programms direkt zur Sprungmarke `addierer` weiterleitet. Sobald der Befehl `B addierer` ausgeführt wird, springt das Programm sofort zum Block `addierer` und die nachfolgende Anweisung (MOV R3, #5) wird übersprungen. Nach der Addition in `addierer` wird das Programm beendet.


### BX
Der BX-Befehl ("Branch and Exchange") wird verwendet, um zu einer Adresse zu springen, die in einem Register gespeichert ist.

#### Syntax
```
BX <Register>
```
Hierbei ist `<Register>` das Register, das die Adresse enthält, zu der gesprungen werden soll. 

#### Beispiel
```
    MOV R0, #3           @ Speichere die Zahl 3 in Register R0
    MOV R1, #4           @ Speichere die Zahl 4 in Register R1
    LDR R3, =seg2        @ Lade die Adresse des Blocks 'seg2' in Register R3
    B seg1               @ Springe zu 'seg1'

seg2:
    LDR R4, =ende        @ Lade die Adresse des Blocks 'ende' in Register R4
    BX R4                @ Springe zu 'ende' (die Adresse in R4)

seg1:
    ADD R2, R0, R1       @ Addiere die Werte in R0 und R1, speichere das Ergebnis in R2
    BX R3                @ Springe zur Adresse in R3 (zu Block 'seg2')
	
ende:
    MOV R5, R2           @ Speichere das Ergebnis der Addition (R2) in R5
```

In diesem Beispiel zeigt der Code, wie mit `B` und `BX` verschiedene Blöcke angesprungen werden können. Der `B-Befehl` springt bedingungslos zu einem Label, während `BX` einen Sprung zur Adresse ermöglicht, die in einem Register gespeichert ist. 


### BL
Der BL-Befehl ("Branch with Link") ermöglicht es, zu einer bestimmten Speicheradresse oder einem Code-Block zu springen und gleichzeitig die Rücksprungadresse im Link-Register (LR) zu speichern. Dies ist besonders nützlich für Funktionsaufrufe, bei denen nach der Ausführung einer Funktion zum ursprünglichen Programmfluss zurückgekehrt werden soll.

#### Syntax
```
BL <label>
```
Hierbei ist `label` der Block, zu dem das Programm springen soll.

#### Beispiel
```
    MOV R0, #3          @ Speichere die Zahl 3 in Register R0
    MOV R1, #4          @ Speichere die Zahl 4 in Register R1
    BL addierer         @ Springe in Block "addierer" und speichere die Rücksprungadresse in LR
    MOV R0, R2          @ Dieser Befehl wird ausgeführt, nachdem 'addierer' beendet ist
    LDR R4, =ende       @ Lade die Adresse des Blocks 'ende' in Register R4
    BX R4               @ Springe zu Block 'ende'
    
addierer:
    ADD R2, R0, R1      @ Addiere die Werte 3 (R0) und 4 (R1) auf, speichere das Ergebnis in R2
    BX lr               @ Springe zurück zur Adresse im LR (zurück zu MOV R0, R2)
	
ende:
```
In diesem Beispiel wird der Befehl `BL addierer` verwendet, um in den Block `addierer` zu springen und gleichzeitig die Adresse des nächsten Befehls (MOV R0, R2) im Link-Register (LR) zu speichern. Nach der Addition in addierer wird das Programm durch den Befehl `BX lr` zur gespeicherten Adresse im Link-Register zurückgeleitet und MOV R0, R2 wird ausgeführt.

|--------------------------|------------------------------------|-------------------------------|
|   [zurück](ctrlflow.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](bedingtespr.md)    |


| **3.1 Kontrollflussinstruktionen**                                    |
|-----------------------------------------------------------------------|
| [3.1.1 Intro](ctrlflow.md)                                            |
| [3.1.2 Unbedingte Sprünge](unbedingtespr.md)                          |
| [3.1.3 Bedingte Sprünge](bedingtespr.md)                              |
| [3.1.4 Manipulation des Programmcounters (Übungsaufgabe)](ctrlue.md)	|