## Zahlensysteme
Das Binärsystem verwendet nur zwei Ziffern: 0 und 1. Es ist direkt auf die Art und Weise abgestimmt, wie Daten in Computern verarbeitet werden. Jede Ziffer in einem Binärsystem ist eine Potenz von 2. 
Beispielsweise stellt die Binärzahl 101 die Dezimalzahl 5 dar (1×2^2 + 0×2^1 + 1×2^0 = 4 + 0 + 1).

Das Hexadezimalsystem hingegen verwendet 16 Ziffern: 0-9 und A-F. Es bietet eine kompaktere Darstellung von Binärdaten, da jede hexadezimale Ziffer vier Binärstellen entspricht. Zum Beispiel wird die Binärzahl 1101 im Hexadezimalsystem als D dargestellt (1×2^3 + 1×2^2 + 0×2^1 + 1×2^0 = 8 + 4 + 0 + 1 = 13).

## Bits, Bytes und Nibbles
Bits (Binary Digits) sind die kleinsten Informationseinheiten in der digitalen Welt. Ein Bit kann nur 0 oder 1 sein. Mehrere Bits zusammen ermöglichen die Darstellung komplexerer Daten.
Ein Nibble besteht aus 4 Bits und kann 16 verschiedene Werte darstellen, da \(2^4 = 16\). Jeder Wert in einem Nibble entspricht genau einer Ziffer im Hexadezimalsystem.
Ein Byte besteht aus 8 Bits und kann \(2^8 = 256\) verschiedene Werte darstellen (von 0 bis 255). Diese 256 Werte ergeben sich, weil ein Byte aus zwei Nibbles besteht und somit \(16 \times 16 = 256\) mögliche Kombinationen von 4-Bit Nibbles ergibt. Ein Byte wird häufig verwendet, um ein Zeichen in einem Text oder eine kleine Datenmenge zu kodieren.

Hinweis: Die Taschenrechner-App in Windows verfügt über einen sogenannten Programmer-Mode der es ermöglicht auch größere Werte einfach zwischen den Zahlensystemen zu konvertieren.


## vorzeichenlose und vorzeichenbehaftete Ganzzahlen
In der Computerdarstellung gibt es zwei Hauptarten von Ganzzahlen: vorzeichenbehaftete (`signed`) und vorzeichenlose (`unsigned`) Zahlen.

### Vorzeichenbehaftete Zahlen (Signed):
Vorzeichenbehaftete Zahlen verwenden ein Bit, das sogenannte Vorzeichenbit, um anzuzeigen, ob die Zahl positiv oder negativ ist. Das höchste Bit (Most Significant Bit, MSB) wird als Vorzeichenbit verwendet: Eine `0` steht für eine positive Zahl und eine `1` für eine negative Zahl. Die restlichen Bits repräsentieren den Wert der Zahl. Diese Darstellung ermöglicht es, sowohl positive als auch negative Werte darzustellen. Die häufigste Methode zur Darstellung von vorzeichenbehafteten Zahlen ist das Zweierkomplement, bei dem negative Zahlen durch Invertieren aller Bits der positiven Zahl und Hinzufügen von 1 berechnet werden.

### Zweierkomplement
Das Zweierkomplement ist eine Methode zur Darstellung von positiven und negativen Ganzzahlen im Binärsystem. Es ermöglicht eine effiziente Handhabung von Vorzeichen und mathematischen Operationen mit Binärzahlen.

#### Grundprinzip des Zweierkomplements
Im Zweierkomplement wird das höchstwertige Bit (MSB) als Vorzeichenbit verwendet: Eine `0` steht für positive, eine `1` für negative Zahlen. Um eine negative Zahl im Zweierkomplement darzustellen, invertiert man alle Bits der positiven Darstellung und addiert anschließend `1`.


# BEISPIEL 


#### Interpretation von signed Binärzahlen:
Man beginnt mit dem höchstwertigen Bit. Wenn dieses eine 1 ist und von weiteren aufeinanderfolgenden 1en gefolgt wird, wird die niedrigste 1 in dieser Sequenz identifiziert (die direkt vor dem ersten 0 steht). Die Position dieser 1 wird in die entsprechende Zweierpotenz umgewandelt, wobei dieser Wert negativ ist. Anschließend werden alle Werte der „niedrigeren Bits“ zu dieser Summe addiert. Ist das MSB(Most signifikant Bit) keine 1, dann ist die Zahl genauso zu lesen, wie eine vorzeichenlose Zahl.

Beispiel:  
`11110101` ergibt `-16 + 5 = -11`.

Diese Methode ist schneller als der übliche Ansatz.

### Vorzeichenlose Zahlen (Unsigned):
Vorzeichenlose Zahlen verwenden alle verfügbaren Bits zur Darstellung des Zahlenwerts, ohne ein Bit für das Vorzeichen zu reservieren. Daher können vorzeichenlose Zahlen nur nicht-negative Werte darstellen, also Werte von 0 bis zu einem maximalen Wert, der durch die Anzahl der Bits bestimmt wird. Zum Beispiel kann eine 8-Bit vorzeichenlose Zahl Werte von 0 bis 255 darstellen. Da kein Bit für das Vorzeichen verwendet wird, kann jeder einzelne Bitplatz zur Darstellung von Zahlen verwendet werden, was zu einem größeren Bereich positiver Werte führt als bei vorzeichenbehafteten Zahlen mit derselben Bitanzahl.





