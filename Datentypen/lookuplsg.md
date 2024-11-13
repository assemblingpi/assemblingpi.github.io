# A.4 Datentypen
## 4.3.5 Komplexe Datentypen: Lookup-Tables 
### Lösungen der Übungsaufgaben

#### Aufgabenstellung 1
**Lösung:**

Der folgende Code verwendet eine Lookup-Tabelle, um die Werte der Funktion `y = x² + 2x + 3` für `x` von 0 bis 9 vorab zu berechnen und zu speichern. Durch den Zugriff auf die Tabelle kann der Wert von `y` schnell ermittelt werden, ohne die Berechnung zur Laufzeit durchführen zu müssen.

```asm
.data
Lookup:
	.byte 3, 6, 11, 18, 27, 38, 51, 66, 83, 102

.text
.global _start
_start:
	@ r1  = x
	@ r2  = Baseadresse der Lookup-Tabelle
	@ r3 = y

	LDR  r2, =Lookup        @ Lade die Basisadresse der Lookup-Tabelle in r2
	LDRB r3, [r2, r1]       @ Lade den Wert y = x^2 + 2x + 3 aus der Tabelle basierend auf x in r3
HLT:
	B HLT                   @ Halte das Programm
```

##### Erklärung des Code-Beispiels

1. **Initialisierung der Lookup-Tabelle:**
   ```asm
   .data
   Lookup:
   	.byte 3, 6, 11, 18, 27, 38, 51, 66, 83, 102
   ```
   Die Tabelle `Lookup` enthält die vorab berechneten Werte der Funktion `y = x² + 2x + 3` für `x` von 0 bis 9. Jeder Eintrag entspricht dem Ergebnis der Funktion für den jeweiligen `x`-Wert.

2. **Laden der Basisadresse:**
   ```asm
   LDR  r2, =Lookup
   ```
   Die Adresse der Lookup-Tabelle wird in das Register `r2` geladen, um später auf die Tabelle zugreifen zu können.

3. **Abrufen des Funktionswerts:**
   ```asm
   LDRB r3, [r2, r1]
   ```
   Der Wert von `x` ist im Register `r1` gespeichert. Durch `LDRB` (Load Register Byte) wird der entsprechende Wert `y` aus der Tabelle geladen und in das Register `r3` geschrieben. Der Ausdruck `[r2, r1]` berechnet den Offset innerhalb der Tabelle basierend auf dem Wert von `x`.

4. **Endlosschleife:**
   ```asm
   HLT:
   	B HLT
   ```
   Diese Anweisung sorgt dafür, dass das Programm nach dem Laden des Wertes in `r3` in einer Endlosschleife verharrt.


#### Aufgabenstellung 1
**Lösung:**

Der folgende Code implementiert eine Lookup-Tabelle, welche die Potenzen von 10 von `10^0` bis `10^9` speichert. Durch den Zugriff auf diese Tabelle kann der Wert von `y` für einen gegebenen `x` schnell ermittelt werden.

```asm
.data
Lookup:
	.word 1, 10, 100, 1000, 10000, 100000, 1000000, 10000000, 100000000, 1000000000

.text
.global _start
_start:
	@ r1  = x
	@ r2  = Baseadresse der Lookup-Tabelle
	@ r3 = y

	LDR  r2, =Lookup           @ Lade die Basisadresse der Lookup-Tabelle in r2
	LDR  r3, [r2, r1, LSL #2]  @ Lade den Wert y = 10^x aus der Tabelle. LSL #2 multipliziert x mit 4 (Größe eines Wortes)
HLT:
	B HLT                      @ Halte das Programm
```

##### Erklärung des Code-Beispiels

1. **Initialisierung der Lookup-Tabelle:**
   ```asm
   .data
   Lookup:
   	.word 1, 10, 100, 1000, 10000, 100000, 1000000, 10000000, 100000000, 1000000000
   ```
   Die Tabelle `Lookup` enthält die vorab berechneten Potenzen von 10 von `10^0` bis `10^9`. Jeder Eintrag ist als Wort (`.word`) gespeichert, da die Werte größer sind und mehrere Bytes benötigen.

2. **Laden der Basisadresse:**
   ```asm
   LDR  r2, =Lookup
   ```
   Die Adresse der Lookup-Tabelle wird in das Register `r2` geladen, um später auf die Tabelle zugreifen zu können.

3. **Abrufen der Potenz:**
   ```asm
   LDR  r3, [r2, r1, LSL #2]
   ```
   Der Wert von `x` ist im Register `r1` gespeichert. Durch `LSL #2` (Logical Shift Left um 2 Bits) wird `x` mit 4 multipliziert, was der Größe eines Wortes entspricht. Dies ist notwendig, da jeder Tabelleneintrag 4 Bytes groß ist. Der berechnete Offset wird zur Basisadresse `r2` addiert, um den korrekten Tabellenindex zu erreichen. Der resultierende Wert `y = 10^x` wird dann in das Register `r3` geladen.

4. **Endlosschleife:**
   ```asm
   HLT:
   	B HLT
   ```
   Diese Anweisung sorgt dafür, dass das Programm nach dem Laden des Wertes in `r3` in einer Endlosschleife verharrt.

|--------------------------|------------------------------------|----------------------------------|
|   [zurück](lookupue.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](arraysmultidim.md)    |


| **4.3 Komplexe Datentypen**                                                   |
|-------------------------------------------------------------------------------|
| [4.3.1 Intro](komplexedtypen.md)                                              |
| [4.3.2 Structs (Strukturen)](structs.md)                                      |
| [4.3.3 Arrays in Assembler](arrays.md)                                        |
| [4.3.4 Zugriffsberechnung bei einem eindimensionalen Array](array1dim.md)     |
| [4.3.5 Lookup-Tables](lookuptable.md)                           		        |
| [4.3.6 Mehrdimensionalen Arrays](arraysmultidim.md)                           |