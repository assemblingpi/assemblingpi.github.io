# A.4 Datentypen 
## 4.3.5 Komplexe Datentypen: Lookup-Tables

**Lookup**-Tabellen (LUTs) beschleunigen Berechnungen, indem sie **vorab berechnete Ergebnisse** speichern und bei Bedarf abrufen. Dies spart Rechenzeit, besonders bei wiederholten oder aufwändigen Operationen. 

LUTs sind also im Grunde ein oder mehrdimensionale Arrays mit vordefinierten Werten, auf die lesend zugegriffen wird. Eindimensionale LUTs speichern Werte, die über einen Index direkt zugänglich sind, also einen Eingabeparameter haben - während mehrdimensionale LUTs mehrere Parameter berücksichtigen. Im Folgenden betrachten wir ausschließlich LUTs, die nur einen Eingabeparameter nutzen.

Durch die direkte Zuordnung von Eingabewerten zu vordefinierten Ergebnissen mittels eines **Indexes** entfällt die Notwendigkeit von Laufzeitberechnungen, was Energieverbrauch und Rechenzeit reduziert. Lookup-Tabellen werden häufig zur Berechnung mathematischer Funktionen oder zur Verarbeitung von Sensordaten eingesetzt.

Ein praktisches Beispiel für die Verwendung einer Lookup-Tabelle ist die Berechnung der Fakultät `x!` eines gegebenen Wertes. Im folgenden ARM-Assembler-Code wird eine Lookup-Tabelle genutzt, um die Fakultät von `x` schnell und effizient zu berechnen:

### Berechnung von y= x! mittels Lookup-Tabelle

```asm
.data
Lookup:
	.word 1, 1, 2, 6, 24, 120, 720, 5040, 40320, 362880, 3628800

.text
.global _start
_start:
	@ r1  = x
	@ r2  = Baseaddress
	@ r3  = y
	LDR  r2, =Lookup           @ Lade die Basisadresse der Lookup-Tabelle in r2
	LDR r3, [r2, r1, LSL #2]   @ Lade den Wert y = x! aus der Tabelle. LSL #2 multipliziert x mit 4 (Größe eines Wortes)
HLT:
	B HLT                      @ Halte das Programm
```

### Erklärung des Code-Beispiels

Im obigen Code wird die Fakultät eines Wertes `x` mithilfe einer Lookup-Tabelle berechnet. Die Tabelle `Lookup` enthält vorab berechnete Fakultätswerte für `x` von 0 bis 10. Jeder Eintrag in der Tabelle entspricht dem Wert `x!` für den entsprechenden Index `x`.

1. **Initialisierung der Lookup-Tabelle:**
   ```asm
   .data
   Lookup:
   	.word 1, 1, 2, 6, 24, 120, 720, 5040, 40320, 362880, 3628800
   ```
   Hier werden die Fakultätswerte für `x` von 0 bis 10 in der Datensektion definiert. Jeder Wert ist als Word gespeichert.

2. **Laden der Basisadresse:**
   ```asm
   LDR  r2, =Lookup
   ```
   Die Adresse der Lookup-Tabelle wird in das Register `r2` geladen. Dies ermöglicht den Zugriff auf die Tabelle im folgenden Schritt.

3. **Abrufen des Fakultätswerts:**
   ```asm
   LDR r3, [r2, r1, LSL #2]
   ```
   Der Wert von `x` ist im Register `r1` gespeichert. Durch `LSL #2` (Logical Shift Left um 2 Bits) wird `x` mit 4 multipliziert, was notwendig ist, wenn jeder Tabelleneintrag 4 Bytes (ein Wort) groß ist. Der berechnete Offset wird zur Basisadresse `r2` addiert, um den korrekten Tabellenindex zu erreichen. Der resultierende Wert `y = x!` wird dann in das Register `r3` geladen.

4. **Endlosschleife:**
   ```asm
   HLT:
   	B HLT
   ```
   Diese Anweisung sorgt dafür, dass das Programm nach dem Laden des Wertes in `r3` in einer Endlosschleife verharrt.

|----------------------------|------------------------------------|----------------------------|
|   [zurück](array1dlsg.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](lookupue.md)    |


| **4.3 Komplexe Datentypen**                                                   |
|-------------------------------------------------------------------------------|
| [4.3.1 Intro](komplexedtypen.md)                                              |
| [4.3.2 Structs (Strukturen)](structs.md)                                      |
| [4.3.3 Arrays in Assembler](arrays.md)                                        |
| [4.3.4 Zugriffsberechnung bei einem eindimensionalen Array](array1dim.md)     |
| [4.3.5 Lookup-Tables](lookuptable.md)                           		        |
| [4.3.6 Mehrdimensionalen Arrays](arraysmultidim.md)                           |