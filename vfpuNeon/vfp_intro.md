# B.2 Erweiterungen der CPU-Funktionalität
## 2.3.4 VFP und NEON: VFP (Vector Floating Point) in der ARM-Architektur

Der **Vector Floating Point (VFP) Coprozessor** ist eine Erweiterung der ARM-Architektur, die Hardware-Unterstützung für Gleitkommaoperationen bietet. VFP ermöglicht effiziente Berechnungen mit Gleitkommazahlen im Single-Precision (32-Bit) und Double-Precision (64-Bit) Format. Diese Erweiterung ist entscheidend für Anwendungen, die umfangreiche mathematische Berechnungen erfordern, wie beispielsweise wissenschaftliche Simulationen, Signalverarbeitung und Multimedia-Anwendungen.

**Hinweis: Im folgenden werden wir uns auf Single-Precision Arithmetik beschränken**

### VFP-Coprozessor
Der VFP wird als eigenständiger Coprozessor innerhalb des ARM-Systems implementiert. Dieser arbeitet parallel zum Hauptprozessor und verfügt über eigene Register und Instruktionen, die speziell für Gleitkommaoperationen entwickelt wurden. Durch die Auslagerung dieser rechenintensiven Aufgaben an den VFP-Coprozessor wird der Hauptprozessor entlastet, was zu einer verbesserten Gesamtleistung des Systems führt.

### VFP-Register
Der VFP-Coprozessor verfügt über eine Reihe von Registern zur Speicherung von Operanden und Ergebnissen:

**S-Register (32-Bit Single Precision):** Es gibt 32 S-Register, bezeichnet als S0 bis S31. Diese werden für 32-Bit-Gleitkommawerte verwendet.
**D-Register (64-Bit Double Precision):** Die D-Register bestehen aus Paaren von S-Registern. Zum Beispiel bildet S0 zusammen mit S1 das D0-Register. Es gibt insgesamt 16 D-Register (D0 bis D15).

### VFP-Instruktionssatz
Der VFP-Coprozessor bietet einen umfangreichen Instruktionssatz, der speziell für Gleitkommaoperationen entwickelt wurde. Dazu gehören:

**Arithmetische Instruktionen:** Addition, Subtraktion, Multiplikation, Division und Quadratwurzelberechnungen.
**Konvertierungsinstruktionen:** Umwandlung zwischen Gleitkomma- und Integerformaten, beispielsweise von einem 32-Bit-Float zu einem 32-Bit-Integer und umgekehrt.
**Vergleichsinstruktionen:** Vergleiche zwischen Gleitkommazahlen, um Bedingungen wie Gleichheit, Ungleichheit oder Größenvergleiche zu evaluieren.
**Datentransferinstruktionen:** Laden und Speichern von Gleitkommawerten aus bzw. in den Speicher.
**Spezielle Funktionen:** Instruktionen zur Behandlung von Ausnahmebedingungen, Rundungsmodi und anderen Kontrollfunktionen.
Diese Instruktionen sind darauf ausgelegt, hohe Genauigkeit und Leistung bei Gleitkommaberechnungen zu gewährleisten.

### Das Floating Point Status and Control Register (FPSCR)
Das FPSCR ist ein Register im VFP-Coprozessor, das Status- und Kontrollinformationen für Gleitkommaoperationen speichert. 



### Überschneidungen und Schnittmengen mit NEON: Gemeinsame Nutzung von Registern
Sowohl VFP als auch der NEON-Coprozessor nutzen die gleichen physikalischen Register im Prozessor:

**S- und D-Register:** Diese Register werden sowohl von VFP als auch von NEON verwendet. Während VFP sie hauptsächlich für skalare Gleitkommaoperationen nutzt, verwendet NEON sie für SIMD-Operationen (Single Instruction, Multiple Data) auf Vektordaten.
**Q-Register:** Diese 128-Bit-Register bestehen aus Paaren von D-Registern. NEON nutzt Q-Register intensiv für parallele Datenverarbeitung. VFP greift indirekt auf Q-Register zu, indem es die darunterliegenden D-Register verwendet.
**Gemeinsames FPSCR**
Beide Einheiten, VFP und NEON, teilen sich das Floating Point Status and Control Register (FPSCR). Das bedeutet, Änderungen an den Rundungsmodi oder Statusflags im FPSCR wirken sich auf beide aus. Dies erfordert bei der Programmierung besondere Aufmerksamkeit, um unerwartete Effekte zu vermeiden.


### Dekodierung und Ausführung von Gleitkommaoperationen
**Dekodierung der Instruktion:** Der ARM-Hauptprozessor erkennt eine VFP-Instruktion und delegiert sie an den VFP-Coprozessor.
**Operandenzugriff:** Die benötigten Operanden werden aus den VFP-Registern entnommen.
**Berechnung:** Der VFP-Coprozessor führt die angeforderte Operation durch.
**Ergebnisspeicherung:** Das Resultat wird in einem VFP-Register abgelegt.
**Statusaktualisierung:** Das FPSCR wird aktualisiert, um den Status der Operation widerzuspiegeln, beispielsweise das Setzen von Flags bei Ausnahmebedingungen.

|----------------------------|-------------------------------|--------------------------------|
| [zurück](floatingnums.md)  | [Hauptmenü](../ueberblick.md) | [weiter](vfpconv.md)           | 