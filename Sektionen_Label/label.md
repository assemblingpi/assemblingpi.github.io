# Label in ARM-Assembler

## Label, Variablennamen - Was hat es damit auf sich?
In der Assembler-Programmierung sind Labels wichtige Hilfsmittel zur Organisation von Speicher und Code. Sie fungieren als „Markierungen“, bzw. Aliasnamen, die bestimmten Speicheradressen  zugewiesen sind. Labels erleichtern die Arbeit, da sie anstelle von direkten Adressen verwendet werden können.
Ein Label ist für den Assembler (das Übersetzungsprogramm) eine Konstante, die für eine bestimmte Speicheradresse steht. 

### Label in der Daten-Sektion:
In der Daten-Sektion eines Assembler-programms werden Labels verwendet, um bestimmte Speicherstellen zu benennen. Zum Beispiel:
```asm
.section .data
My_data: .word 0xbb0000aa
```
Hier bezeichnet das Label My_data eine Speicheradresse. Ab dieser Adresse wird der Speicher so initialisiert, dass ein Word (also 4 Bytes) mit dem Wert 0xbb0000aa belegt wird. Das Label My_data fungiert also als Verweis auf die Adresse, die den festgelegten Wert enthält. Wenn später im Programm auf My_data verwiesen wird, greift der Code auf diese Speicheradresse zu, um den darin gespeicherten Wert zu lesen oder zu schreiben.

#### Assembler-Direktiven zum Anlegen von Daten
**Assembler-Direktiven** wie `.byte`, `.hword`, `.word` und `.asciz` werden in der **.data-Sektion** verwendet, um unterschiedliche Datentypen im Speicher zu **definieren** und zu **initialisieren**. Diese Direktiven sind **keine Maschinenbefehle**, sondern Anweisungen an den Assembler, die festlegen, wie viel Speicher reserviert werden soll und welche Werte dort abgelegt werden. Sie ermöglichen Entwicklern eine präzise Kontrolle über die Speicherorganisation, indem sie bestimmen, welche Daten wo und in welcher Form im Speicher platziert werden. 

- **.byte:** Reserviert ein oder mehrere Bytes und initialisiert sie mit den angegebenen Werten.
```
.byte 0x1, 0x2, 0x3
```
- **.hword:** Reserviert Halbwörter (2 Bytes) und initialisiert sie.
```
.hword 0x1234
```
- **.word:** Reserviert Wörter (4 Bytes) und initialisiert sie.
```
.word 0x12345678
```
- **.float:** Reserviert 4 Bytes für eine Fließkommazahl und initialisiert sie mit dem angegebenen Wert.
```
.float 3.14
```
- **.asciz:** Reserviert eine nullterminierte Zeichenkette (String). (Eine nullterminierte Zeichenkette ist eine Folge von Zeichen, die mit einem Byte mit dem Wert 0 endet.)
```
.asciz "Hello, World!"
```

Um die mit diesen Direktiven definierten Daten auch sinnvoll im Programm verwenden zu können, sollten sie mit einem Label versehen werden, wie etwa:
```
mydata: .word 0xabc
```


### Der Literal Pool
Achtung: Ein häufiges Missverständnis ist, dass der Befehl ```ldr r0, =My_data``` die Daten, (in diesem Fall den Wert 0xbb0000aa) in ein Register lädt. 
Tatsächlich lädt dieser Befehl jedoch die Adresse von My_data in das Register r0, nicht den gesuchten Datenwert. 
Woher wird diese Adresse geladen? Der Assembler platziert diese Adresse am Ende der Text-Sektion im Code, im sogenannten Literal Pool um den Zugriff auf die Daten zu ermöglichen. 

Um den tatsächlichen Wert von My_data in ein Register zu laden, müssen folglich zwei Schritte durchgeführt werden:
1.Laden der Adresse von My_data in ein Register:
```asm
ldr r0, =My_data
```
2.Zugriff auf den Wert an dieser Adresse:
```asm
ldr r0, [r0]
```


### Label in der Text-Sektion:
Im Code-Bereich eines Assemblers dienen Labels dazu, bestimmte Stellen im Programmfluss zu kennzeichnen:
```asm
.section .code
...
Here: b here
```
Das Label `Here` ist ein Platzhalter, nicht etwa für die Instruktion, die auf das Label folgt, sondern für die Adresse, bei der diese Instruktion im Speicher anfängt. Demnach handelt es sich bei dem Branchbefehl (Sprung) an dieser Stelle um eine Dauerschleife.

|-----------------------|-------------------------------|---------------------------------------------|
| [zurück](sektionen.md)| [Hauptmenü](../ueberblick.md) | [weiter](../Instruktionen/arithlogintro.md) |
