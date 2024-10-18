# B.2 Erweiterungen der CPU-Funktionalität
## 2.3.2 VFP und NEON: Gleitkommazahlen

Aufbau einer binären Gleitkommazahl: 

`...` + `x1 * 2^3` + `x2 * 2^2` + `x3 * 2^1` + `x4 * 2^0` `. (Radixpunkt)` + `x5 * 2^(-1)` + `x6 * 2^(-2)` + `x7 * 2^(-3)` + `...`

Um eine dezimale Gleitkommazahl in eine binäre Gleitkommazahl umzuwandeln, ist es wichtig, den Vorkommateil und den Nachkommanteil der Zahl getrennt zu behandeln. 

### 1. Umwandlung des Vorkommateils
Der Vorkommateil einer Zahl wird nach demselben Verfahren konvertiert, das auch bei der Umwandlung einer ganzen Dezimalzahl in eine Binärzahl verwendet wird. Dieser Prozess lässt sich in den folgenden Schritten zusammenfassen:

- **Teilen durch 2:** Die Zahl wird durch 2 geteilt, wobei der Rest, der entweder 0 oder 1 beträgt, notiert wird.
- **Wiederholen:** Das Ergebnis der Division wird für den nächsten Schritt verwendet, und der Prozess wird wiederholt, bis das Ergebnis 0 ist.
- **Lesen der Restwerte:** Die Restwerte werden **von unten nach oben** abgelesen, um den binären Vorkommateil zu erhalten.

**Beispiel:**
Die Dezimalzahl 13 wird wie folgt in eine binäre Zahl konvertiert:
- 13 ÷ 2 = 6 Rest 1
-  6 ÷ 2 = 3 Rest 0
-  3 ÷ 2 = 1 Rest 1
-  1 ÷ 2 = 0 Rest 1

Die Restwerte ergeben von unten nach oben gelesen die Zahl **1101**. Somit ist 13 in Binär **1101**.

### 2. Umwandlung des Nachkommanteils
Der Nachkommateil einer Dezimalzahl wird anschließend folgendermaßen in eine Binärzahl umgewandelt:
- **Multiplizieren mit 2:** Der Nachkommanteil der Zahl wird mit 2 multipliziert.
- **Vorkommateil notieren:** Der Vorkommateil des Ergebnisses, entweder 0 oder 1, wird als erster binärer Stellenwert nach dem Komma notiert.
- **Wiederholen:** Der neue Nachkommanteil wird erneut mit 2 multipliziert, und der Vorgang wird wiederholt, bis der Nachkommanteil 0 erreicht oder bis die gewünschte Genauigkeit erzielt ist.

**Beispiel:**
Für die Dezimalzahl 0.375 wird das Verfahren wie folgt angewendet:
- 0.375 × 2 = 0.75 -> Vorkommateil 0
- 0.75 × 2 = 1.5 -> Vorkommateil 1
- 0.5 × 2 = 1.0 -> Vorkommateil 1 (Nachkommanteil ist nun 0)

Das Ergebnis ist **0.011** in Binär.

### 3. Zusammensetzen der binären Gleitkommazahl
Nach der Umwandlung des Vorkommateils und des Nachkommanteils in Binär werden beide Teile kombiniert, um die gesamte binäre Gleitkommazahl zu erhalten.

**Beispiel:**
Die Dezimalzahl 13.375 besteht aus dem Vorkommateil **13** und dem Nachkommanteil **0.375**. Die binäre Darstellung lautet:

- Vorkommateil: **1101**
- Nachkommanteil: **011**

Das Ergebnis ist **1101.011** in Binär.

Diese Methode stellt eine exakte binäre Darstellung der Zahl dar und unterscheidet sich vom Floating-Point-Format, das in Computern zur Speicherung von Gleitkommazahlen verwendet wird. Im Floating-Point-Format werden zusätzlich ein **Exponent** und eine **Mantisse** berücksichtigt, was bei dieser Umwandlung noch nicht der Fall ist.

|-----------------------------|-------------------------------|-------------------------------------|
| [zurück](floatingintro.md)  | [Hauptmenü](../ueberblick.md) | [weiter](floatingnums.md)           | 
