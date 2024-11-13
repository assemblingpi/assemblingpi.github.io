# B.2 Erweiterungen der CPU-Funktionalität
## 2.3.10 VFP und NEON: Das NEON und Floatingpoint Status Register

Das **FPSCR**(Floating Point Status and Control Register) enthält wichtige Status- und Kontrollinformationen für Gleitkomma- und NEON-Operationen. Es überwacht die Bedingungen der Operationen und ermöglicht es dem Prozessor, auf bestimmte Ereignisse wie Überläufe, Divisionen durch Null und andere Ausnahmen zu reagieren.

|31|30|29|28|  27  |  26  |  25  |  24  |23   22|21    20|19|18    16|
|--|--|--|--|------|------|------|------|-------|--------|--|--------|
|N |Z |C |V |  QC  |  AHP |  DN  |  FZ  | RMode | Stride |- |   Len  |

|   15   |14  13| 12 | 11  | 10  |  9  |   8   |  7  |6   5|  4   |  3  |  2  |  1  |  0  |
|--------|------|----|-----|-----|-----|-------|-----|-----|------|-----|-----|-----|-----|
|  IDE   |   -  |IXE | UFE | OFE | DZE |  IOE  | IDC |  -  | IXC  | UFC | OFC | DZC | IOC |

### Statusbits und Flags:
- **N (31):** Negative Flag - Setzt das Flag, wenn das Ergebnis einer Operation negativ ist.
- **Z (30):** Zero Flag - Setzt das Flag, wenn das Ergebnis einer Operation null ist.
- **C (29):** Carry Flag - Wird bei Gleitkommaoperationen nicht verwendet, bei einigen NEON-Operationen kann es zur Anzeige eines Übertrags verwendet werden.
- **V (28):** Overflow Flag - Setzt das Flag, wenn eine Operation zu einem Überlauf führt (d.h., wenn das Ergebnis größer ist als das darstellbare Maximum).

### Kontrollbits:
- **QC (27):** Cumulative Saturation Flag - Setzt das Flag, wenn eine NEON-Saturationsoperation zu einem gesättigten Ergebnis geführt hat. Dieses Flag akkumuliert diese Information über mehrere Operationen hinweg. Eine Sättigungsoperation tritt auf, wenn das Ergebnis einer Berechnung den maximal darstellbaren Wert für den Datentyp überschreiten würde. Statt den Wert einfach überlaufen zu lassen (was zu einem falschen Ergebnis führen könnte), wird der Wert auf den maximalen (oder minimalen) möglichen Wert begrenzt. Dies nennt man "Sättigung".
- **AHP (26):** Alternative Half-Precision - Legt fest, ob das 16-Bit-Gleitkommaformat verwendet wird.
- **DN (25):** Default NaN Mode - Wenn gesetzt, erzeugt der Prozessor eine "quiet NaN" (not-a-number) anstelle von Signaling NaNs. Das vereinfacht die Handhabung von NaN-Werten.
- **FZ (24):** Flush-to-Zero Mode - Wenn gesetzt, werden sehr kleine Werte (denormalisierte Zahlen) zu null gerundet, um die Rechenleistung zu verbessern.

### Rundungsmodus und Stride/Länge:
- **RMode (23–22):** Rounding Mode   
    **Bestimmt den Rundungsmodus:**
```    
    00: Rundung auf nächste Zahl 
    01: Rundung Richtung Plus Unendlich 
    10: Rundung Richtung Minus Unendlich 
    11: Rundung Richtung Null
```
- **Stride (21–20):** Vector Stride - Legt die Schrittweite für NEON-Lade-/Speicheroperationen fest. 

### Kontroll- und Statusbits für Vektoren:
- **Len (19–16):** Vector Length - Gibt die Länge der NEON-Vektoroperationen an, d.h: wie viele Elemente im Vektor verarbeitet werden.
- **IDE (15):** Input Denormal Exception - Wenn gesetzt, tritt eine Ausnahme auf, wenn eine denormalisierte Zahl als Eingabe verwendet wird.

### Exceptionflags:
- **IXE (12):** Inexact Exception Enable - Wenn gesetzt, wird eine Ausnahme ausgelöst, wenn das Ergebnis einer Operation ungenau ist.
- **UFE (11):** Underflow Exception Enable - Wenn gesetzt, wird eine Ausnahme ausgelöst, wenn das Ergebnis einer Operation zu klein ist, um dargestellt zu werden.
- **OFE (10):** Overflow Exception Enable - Wenn gesetzt, wird eine Ausnahme ausgelöst, wenn das Ergebnis einer Operation einen Overflow verursacht.
- **DZE (9):** Division by Zero Exception Enable - Wenn gesetzt, wird eine Ausnahme ausgelöst, wenn eine Division durch Null auftritt.
- **IOE (8):** Invalid Operation Exception Enable - Wenn gesetzt, wird eine Ausnahme ausgelöst, wenn eine ungültige Operation durchgeführt wird.

### Statusbits für Ausnahmen:
- **IDC (7):** Input Denormal Cumulative Flag - Setzt das Flag, wenn eine denormalisierte Zahl als Eingabe verwendet wurde.
- **IXC (4):** Inexact Cumulative Flag - Setzt das Flag, wenn das Ergebnis einer Operation ungenau war.
- **UFC (3):** Underflow Cumulative Flag - Setzt das Flag, wenn ein Unterlauf aufgetreten ist.
- **OFC (2):** Overflow Cumulative Flag - Setzt das Flag, wenn ein Überlauf aufgetreten ist.
- **DZC (1):** Division by Zero Cumulative Flag - Setzt das Flag, wenn eine Division durch Null aufgetreten ist.
- **IOC (0):** Invalid Operation Cumulative Flag - Setzt das Flag, wenn eine ungültige Operation durchgeführt wurde

|-----------------------|-------------------------------|----------------------------------|
| [zurück](neonadr.md)  | [Hauptmenü](../ueberblick.md) | [weiter](neonctrl.md)            |


|**2.3 VFP und NEON**                                                                                               |
|-------------------------------------------------------------------------------------------------------------------|
| [2.3.1 Intro](floatingintro.md)                                                                                   |
| [2.3.2 Gleitkommazahlen](bingleit.md)                                                                             |
| [2.3.3 Floating Point Format nach IEEE 754](floatingnums.md)                                                      |
| [2.3.4 VFP (Vector Floating Point) in der ARM-Architektur](vfp_intro.md)                                          |
| [2.3.5 VFP Data Conversion Befehle](vfpconv.md)                                                                   |
| [2.3.6 Was ist NEON?](neonintro.md)                                                                               |
| [2.3.7 Überblick über die ARMv7 NEON-Register](neonregs.md)                                                       |
| [2.3.8 Vektoren und Skalare](scalvekt.md)                                                                         |
| [2.3.9 Registeradressierung in NEON](neonadr.md)                                                                  |
| [2.3.10 Das NEON und Floatingpoint Status Register](neonstat.md)                                                  |
| [2.3.11 Steuerung und Statusübertragung zwischen ARM- und NEON/VFP-Statusregistern (VMSR und VMRS)](neonctrl.md)  |
| [2.3.12 NEON Instruktionen](neoninstr.md)                                                                         |
| [2.3.13 Datentransfer](vmov.md)                                                                                   |
| [2.3.14 NEON Load/Store Instruktionen](neonldstr.md)                                                              |
| [2.3.15 Arithmetische und logische NEON-Operationen](varithlog.md)                                                |
| [2.3.16 VTRN (Vector Transpose) Instruktionen](vtrn.md)                                                           |
| [2.3.17 Implementierung von Trigonometrischen Funktionen](trigon_ue.md)                                           |
| [2.3.18 Implementierung einer 4x4-Matrixmultiplikationsfunktion mit NEON](matrix_ue.md)                           |