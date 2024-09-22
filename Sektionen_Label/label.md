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
Das Label "Here" ist ein Platzhalter, nicht etwa für die Instruktion, die auf das Label folgt, sondern für die Adresse, bei der diese Instruktion im Speicher anfängt. Demnach handelt es sich bei dem Branchbefehl (Sprung) an dieser Stelle um eine Dauerschleife.

|-----------------------|-------------------------------|---------------------------------------------|
| [zurück](sektionen.md)| [Hauptmenü](../ueberblick.md) | [weiter](../Instruktionen/arithlogintro.md) |
