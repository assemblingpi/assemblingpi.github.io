# A.4 Datenstrukturen 
## 4.1 Lookup-Tables

**Lookup-Tabellen** beschleunigen Berechnungen, indem **vorab berechnete Ergebnisse** gespeichert und bei Bedarf abgerufen werden. Dies spart Rechenzeit, besonders bei wiederholten oder aufwändigen Operationen. Sie sind nützlich in Systemen mit begrenzten Ressourcen, wie eingebetteten Anwendungen, wo Rechenleistung und Speicher effizient genutzt werden müssen.

Durch die **direkte Zuordnung von Eingabewerten zu vordefinierten Ergebnissen** mittels eines **Index** entfällt die Notwendigkeit von Laufzeitberechnungen, was Energieverbrauch und Rechenzeit reduziert. Sie werden u.a. zur Berechnung mathematischer Funktionen oder der Verarbeitung von Sensordaten verwendet.

Ein praktisches Beispiel für die Verwendung einer Lookup-Tabelle ist die Berechnung der Fakultät `x!` eines gegebenen Wertes. Im folgenden ARM-Assembler-Code wird eine Lookup-Tabelle genutzt, um die Fakultät von `x` schnell und effizient zu berechnen:

### Berechnung von y= x! mittels Lookup-Tabelle

```asm
.data
Lookup:
	.byte 1, 1, 2, 6, 24, 120, 720, 5040, 40320, 362880, 3628800

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
   	.byte 1, 1, 2, 6, 24, 120, 720, 5040, 40320, 362880, 3628800
   ```
   Hier werden die Fakultätswerte für `x` von 0 bis 10 in der Datensektion definiert. Jeder Wert ist als Byte gespeichert, wobei größere Werte möglicherweise mehrere Bytes benötigen, abhängig von der Architektur.

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

|-------------------------------------------|------------------------------------|----------------------------|
|   [zurück](../Statemachine/Statemach.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](lookupue.md)    |