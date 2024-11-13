# A.1 Einführung
## 1.4.3 Die Programmiersprache Assembler: Operanden und Adressierungsarten

In der Assemblerprogrammierung sind Operanden und Adressierungsarten entscheidend, da sie bestimmen, welche Daten von einer Anweisung verarbeitet werden und wie diese Daten im Speicher oder in Registern gefunden werden. Operandenadressen geben den Speicherort der Daten an, sei es im Arbeitsspeicher oder in CPU-Registern, die über eindeutige Kennungen (z.B. R0, R1) angesprochen werden. Manche Anweisungen benötigen keine Operanden, andere hingegen ein bis drei. Bei zwei Operanden dient der erste meist als Zielregister, während der zweite die Quelle ist, die entweder die Daten oder deren Adresse angibt.

### Operanden: Die Bausteine der Anweisungen

Jede Assembleranweisung benötigt **Operanden**, um Daten zu verarbeiten. Ein Operand kann entweder ein direkter Wert, ein Register oder eine Speicheradresse sein. Die Art und Weise, wie diese Operanden adressiert werden, bestimmt, wie die CPU auf die benötigten Daten zugreift und diese verarbeitet.

- **Direkte Operanden:** Hierbei handelt es sich um feste Werte, die direkt in der Anweisung enthalten sind. Zum Beispiel enthält der Befehl `MOV R1, #5` den direkten Wert `5`, der sofort in das Register `R1` geladen wird.
  
- **Register-Operanden:** Diese beziehen sich auf Daten, die in den schnellen **Registern** der CPU gespeichert sind. Ein Beispiel wäre `ADD R1, R2`, bei dem die Werte in den Registern `R1` und `R2` addiert werden.

- **Speicher-Operanden:** Diese Operanden verweisen auf Daten, die im **Hauptspeicher** abgelegt sind. Zum Beispiel bedeutet `LDR R1, [R2]`, dass der Inhalt der Speicheradresse, auf die `R2` zeigt, in das Register `R1` geladen wird.

### Adressierungsarten: Zugriffsmethoden auf Daten

Die **Adressierungsart** einer Anweisung legt fest, wie die Adresse des Operanden berechnet wird. Unterschiedliche Adressierungsarten ermöglichen verschiedene Zugriffsmethoden auf Daten, was die Flexibilität und Effizienz der Datenverarbeitung erhöht.

#### Immediate Adressierung
```
MOV R1, #10
```
Bei der **Immediate Adressierung** ist der Operand ein direkter Wert, der unmittelbar in der Anweisung angegeben wird. Die ALU verarbeitet diesen Wert direkt, ohne auf den Speicher zugreifen zu müssen. Dies ermöglicht schnelle und einfache Operationen.

#### Register-Adressierung
```
ADD R1, R2
```
In der **Register-Adressierung** beziehen sich die Operanden auf Register innerhalb der CPU. Die ALU kann schnell auf die Daten in den Registern zugreifen, was die Verarbeitungsgeschwindigkeit erhöht.

#### Register-indirekte Adressierung
```
LDR R1, [R2]
```
Die **Register-indirekte Adressierung** verwendet den Inhalt eines Registers als Speicheradresse. In diesem Beispiel enthält `R2` die Adresse, an der die zu ladenden Daten gespeichert sind. Dies ermöglicht den Zugriff auf Daten im Hauptspeicher basierend auf dynamischen Adressen.

#### Register-indirekte Adressierung mit Offset
```
LDR R1, [R2, #4]
```
Diese Form der **Register-indirekten Adressierung** fügt einen festen Offset zum Inhalt des Basisregisters hinzu, um die Zieladresse zu berechnen. Dies ist nützlich für den Zugriff auf strukturierte Daten wie Arrays oder Felder innerhalb von Datenstrukturen.

#### Pre-Indexed Adressierung
```
LDR R1, [R2, #4]!
```
Bei der **Pre-Indexed Adressierung** wird der Offset vor dem Zugriff auf den Speicher zur Basisadresse hinzugefügt. Der Inhalt des Basisregisters wird ebenfalls aktualisiert, was den nächsten Zugriff vorbereitet.

#### Post-Indexed Adressierung
```
LDR R1, [R2], #4
```
Die **Post-Indexed Adressierung** führt den Speicherzugriff zuerst durch und fügt anschließend den Offset zur Basisadresse hinzu. Dies ermöglicht den Zugriff auf aktuelle Daten, während die Basisadresse für zukünftige Zugriffe angepasst wird.

#### Register-indirekte Adressierung mit zwei Registern
```
LDR R1, [R2, R3]
```
Hier werden zwei Register verwendet, um die Speicheradresse zu berechnen. `R2` enthält die Basisadresse, und `R3` liefert einen dynamischen Offset, was flexible und komplexe Datenzugriffe ermöglicht.

### Integration der Adressierungsarten in Basic Blocks und Programmfluss

Die **Adressierungsarten** sind integrale Bestandteile der Assemblerprogrammierung, die den Zugriff auf Daten steuern und damit die **Datenverarbeitung** und den **Programmfluss** innerhalb von **Basic Blocks**, sowie deren Verknüpfung beeinflussen. Durch das 

**Beispiel: Adressierungsarten im Kontext eines Basic Blocks**

Betrachten wir ein einfaches Beispiel, das verschiedene Adressierungsarten innerhalb eines Basic Blocks verwendet:

```
MOV R1, #5          @ Immediate Adressierung: R1 = 5
ADD R2, R1          @ Register-Adressierung: R2 = R2 + R1
LDR R3, [R2]        @ Register-indirekte Adressierung: R3 = Speicher[R2]
CMP R3, #10         @ Immediate Adressierung: Vergleiche R3 mit 10
BGE label_end       @ Verzweigung: Springe zu 'label_end' wenn R3 >= 10
SUB R3, R1          @ Register-Adressierung: R3 = R3 - R1
label_end:
...
```


### Vorteile der vielfältigen Adressierungsarten

- **Flexibilität:** Unterschiedliche Adressierungsarten ermöglichen den Zugriff auf eine Vielzahl von Datenstrukturen und Speicherbereichen, was die Programmierung vielseitiger und effizienter macht.
  
- **Effizienz:** Durch die Auswahl der passenden Adressierungsart können Speicherzugriffe optimiert werden, was die Ausführungsgeschwindigkeit von Programmen erhöht.
  
     
|--------------------|-------------------------------|--------------------------------|
| [zurück](anwops.md)| [Hauptmenü](../ueberblick.md) | [weiter](asmdirekt.md)         | 


| **1.4 Die Programmiersprache Assembler**  	                                            |
|-------------------------------------------------------------------------------------------|
| [1.4.1 Was ist Assembler?](../progspracheasm/progasmintro.md)                             |
| [1.4.2 Anweisungen und Operanden](../progspracheasm/anwops.md)                            |
| [1.4.3 Operanden und Adressierungsarten](../progspracheasm/adrmodi.md)                    |
| [1.4.4 Assembler-Direktiven](../progspracheasm/asmdirekt.md)                              |