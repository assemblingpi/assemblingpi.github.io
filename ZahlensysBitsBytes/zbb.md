## Zahlensysteme
Das Binärsystem verwendet nur zwei Ziffern: 0 und 1. Es ist direkt auf die Art und Weise abgestimmt, wie Daten in Computern verarbeitet werden. Jede Ziffer in einem Binärsystem ist eine Potenz von 2. 
Beispielsweise stellt die Binärzahl **101** die Dezimalzahl **5** dar (1×2^2 + 0×2^1 + 1×2^0 = 4 + 0 + 1).

Das Hexadezimalsystem hingegen verwendet 16 Ziffern: 0-9 und A-F. Es bietet eine kompaktere Darstellung von Binärdaten, da jede hexadezimale Ziffer vier Binärstellen entspricht. 
Zum Beispiel wird die Binärzahl **1101** im Hexadezimalsystem als **D** dargestellt 
```
(1×2^3 + 1×2^2 + 0×2^1 + 1×2^0 = 8 + 4 + 0 + 1 = 13)
```

## Bits, Bytes und Nibbles
Bits (Binary Digits) sind die kleinsten Informationseinheiten in der digitalen Welt. Ein Bit kann nur 0 oder 1 sein. Mehrere Bits zusammen ermöglichen die Darstellung komplexerer Daten.
Ein Nibble besteht aus 4 Bits und kann 16 verschiedene Werte darstellen, da \(2^4 = 16\). Jeder Wert in einem Nibble entspricht genau einer Ziffer im Hexadezimalsystem.
Ein Byte besteht aus 8 Bits und kann \(2^8 = 256\) verschiedene Werte darstellen (von 0 bis 255). Diese 256 Werte ergeben sich, weil ein Byte aus zwei Nibbles besteht und somit \(16 \cdot 16 = 256\) mögliche Kombinationen von 4-Bit Nibbles ergibt. Ein Byte wird häufig verwendet, um ein Zeichen in einem Text oder eine kleine Datenmenge zu kodieren.

Hinweis: Die Taschenrechner-App in Windows verfügt über einen sogenannten Programmer-Mode der es ermöglicht auch größere Werte einfach zwischen den Zahlensystemen zu konvertieren.


## vorzeichenlose und vorzeichenbehaftete Ganzzahlen
In der Computerdarstellung gibt es zwei Hauptarten von Ganzzahlen: vorzeichenbehaftete (`signed`) und vorzeichenlose (`unsigned`) Zahlen.

### Vorzeichenbehaftete Zahlen (Signed):
Vorzeichenbehaftete Zahlen verwenden ein Bit, das sogenannte Vorzeichenbit, um anzuzeigen, ob die Zahl positiv oder negativ ist. Das höchste Bit (Most Significant Bit, MSB) wird als Vorzeichenbit verwendet: Eine `0` steht für eine positive Zahl und eine `1` für eine negative Zahl. Die restlichen Bits repräsentieren den Wert der Zahl. Diese Darstellung ermöglicht es, sowohl positive als auch negative Werte darzustellen. Die häufigste Methode zur Darstellung von vorzeichenbehafteten Zahlen ist das Zweierkomplement, bei dem negative Zahlen durch Invertieren aller Bits der positiven Zahl und Hinzufügen von 1 berechnet werden.

### Zweierkomplement
Das Zweierkomplement ist eine Methode zur Darstellung von positiven und negativen Ganzzahlen im Binärsystem. Es ermöglicht eine effiziente Handhabung von Vorzeichen und mathematischen Operationen mit Binärzahlen.

#### Grundprinzip des Zweierkomplements (klassische Methode)
Im Zweierkomplement wird das höchstwertige Bit (MSB) als Vorzeichenbit verwendet: Eine `0` steht für positive, eine `1` für negative Zahlen. Um eine negative Zahl im Zweierkomplement darzustellen, invertiert man alle Bits der positiven Darstellung und addiert anschließend `1`.

#### Schnelle Methode zur Umwandlung von vorzeichenbehafteten Zahlen

Hier sind einfache und schnelle Methoden, die ich entdeckt habe, um vorzeichenbehaftete Zahlen im Kopf umzurechnen. Ich habe sie bisher nirgendwo anders erwähnt gesehen, aber sie haben sich für mich als effektiv erwiesen.

##### Binär -> Dezimal
Wenn man eine Binärzahl im Zweierkomplement in ihr dezimales Äquivalent umwandeln möchte, beginnt man mit dem höchstwertigen Bit (MSB). Wenn dieses Bit eine 1 ist, gefolgt von aufeinanderfolgenden 1en, identifiziert man die niedrigste 1 in der Sequenz (direkt vor der ersten 0). Wandeln Sie die Position dieses Bits in die entsprechende negative Zweierpotenz um und addieren Sie dann alle Werte der niedrigeren Bits zu dieser Summe.

Zum Beispiel würde die Binärzahl **11110101** wie folgt berechnet: **-16** (aus der Position der niedrigsten 1 in der Sequenz) plus **5** (die Summe der Werte der niedrigeren Bits), was zu **-11** führt.

##### Negative Dezimalzahl -> Binär
Wenn man eine negative Dezimalzahl in ihre Zweierkomplement-Binärdarstellung umwandeln möchte, beginnt man damit, den Absolutwert der Zahl zu nehmen und auf die nächste höhere Zweierpotenz aufzurunden. Diese Zweierpotenz bestimmt, wie weit die Binärdarstellung ab dem MSB mit einer kontinuierlichen Folge von Einsen gefüllt wird. Die verbleibenden Bits werden auf Null gesetzt. Dann berechnet man die Differenz zwischen dem Absolutwert und der Zweierpotenz, zu der man aufgerundet hat. Kombinieren Sie diese Differenz mit der ersten Binärfolge mittels einer logischen ODER-Operation.

Zum Beispiel würde die Umwandlung von **-30** beinhalten, den Absolutwert **30** auf **32** aufzurunden und die führenden **1en** hinzuzufügen, was zu **1110000** führt. Dann subtrahiert man 30 von 32, was 2 ergibt, was in Binär **0000010** ist. Die Kombination dieser Ergebnisse ergibt **1110010**, das Zweierkomplement von **-30**.



Beispiel:  
`11110101` ergibt `-16 + 5 = -11`.

Diese Methode ist schneller als der übliche Ansatz.

### Vorzeichenlose Zahlen (Unsigned):
Vorzeichenlose Zahlen verwenden alle verfügbaren Bits zur Darstellung des Zahlenwerts, ohne ein Bit für das Vorzeichen zu reservieren. Daher können vorzeichenlose Zahlen nur nicht-negative Werte darstellen, also Werte von 0 bis zu einem maximalen Wert, der durch die Anzahl der Bits bestimmt wird. Zum Beispiel kann eine 8-Bit vorzeichenlose Zahl Werte von 0 bis 255 darstellen. Da kein Bit für das Vorzeichen verwendet wird, kann jeder einzelne Bitplatz zur Darstellung von Zahlen verwendet werden, was zu einem größeren Bereich positiver Werte führt als bei vorzeichenbehafteten Zahlen mit derselben Bitanzahl.





