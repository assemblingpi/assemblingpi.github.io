# A.2 Basic Blocks implementieren
## 2.2.2 Sektionen: Label in ARM-Assembler

### Label, Variablennamen - Was hat es damit auf sich?
In der Assembler-Programmierung sind Labels wichtige Hilfsmittel zur Organisation von Speicher und Code. Sie fungieren als „Markierungen“, bzw. Aliasnamen, die bestimmten Speicheradressen  zugewiesen sind. Labels erleichtern die Arbeit, da sie anstelle von direkten Adressen verwendet werden können.
Ein Label ist für den Assembler (das Übersetzungsprogramm) eine Konstante, die für eine bestimmte Speicheradresse steht. 

#### Label in der Daten-Sektion:
In der Daten-Sektion eines Assembler-programms werden Labels verwendet, um bestimmte Speicherstellen zu benennen. Zum Beispiel:
```asm
.section .data
My_data: .word 0xbb0000aa
```
Hier bezeichnet das Label My_data eine Speicheradresse. Ab dieser Adresse wird der Speicher so initialisiert, dass ein Word (also 4 Bytes) mit dem Wert 0xbb0000aa belegt wird. Das Label My_data fungiert also als Verweis auf die Adresse, die den festgelegten Wert enthält. Wenn später im Programm auf My_data verwiesen wird, greift der Code auf diese Speicheradresse zu, um den darin gespeicherten Wert zu lesen oder zu schreiben.

##### Assembler-Direktiven zum Anlegen von Daten
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

#### Hintergrund

In der ARMv7-Architektur kann der Befehl `ldr r0, =My_data` eine 32-Bit-Adresse wie die von `My_data` nicht direkt als Immediate-Wert kodieren. Immediate-Werte in ARM-Instruktionen sind auf kleinere Größen beschränkt, und ein Maschinenbefehl selbst umfasst nur 32 Bit, was nicht ausreicht, um eine vollständige 32-Bit-Adresse direkt im Befehl zu integrieren. Wie lädt `ldr` also dennoch eine solche Adresse korrekt?

#### Der Literal Pool

Dieses Problem wird durch den **Literal Pool** gelöst. Der Literal Pool ist ein spezieller Speicherbereich, der Konstanten wie Adressen oder große Zahlen enthält, die nicht direkt in einem Maschinenbefehl kodiert werden können. Statt die Konstanten in den Befehl einzubetten, speichert der Assembler sie im Literal Pool, und der `ldr`-Befehl lädt sie von dort.

#### Funktionsweise

Der Zugriff auf den Literal Pool erfolgt mittels **PC-relativer Adressierung**, wobei der Programmzähler (PC) als Referenz dient, um die gewünschte Konstante im Literal Pool zu finden. Dies ermöglicht es dem `ldr`-Befehl, entweder eine Adresse (die eines Labels) oder einen unmittelbaren Wert zu laden, ohne dass diese direkt im Befehl kodiert werden müssen.

- **Laden einer Adresse**: Wird ein Label wie `My_data` referenziert, speichert der Literal Pool die **Adresse** dieses Labels. Der Befehl `ldr r0, =My_data` lädt dann die Adresse von `My_data` in das Register `r0`.
  
  Beispiel:
  ```assembly
  ldr r0, =My_data    @ Lädt die Adresse von My_data in r0
  ```

- **Laden eines Werts**: Handelt es sich hingegen um eine direkte Zahl, speichert der Literal Pool diesen **Wert**. Der Befehl `ldr r1, =0x12345678` lädt dann den Wert `0x12345678` direkt in das Register `r1`.

  Beispiel:
  ```assembly
  ldr r1, =0x12345678 @ Lädt den Wert 0x12345678 in r1
  ```

##### Detaillierter Ablauf:

1. **Platzierung des Literal Pools**: Der Literal Pool wird vom Assembler meist, aber nicht zwingend am Ende einer Code-Sektion eingefügt. Der Assembler kann ihn näher an den Befehlen platzieren, die ihn benötigen. In umfangreichen Code-Segmenten kann es mehrere Literal Pools geben, die strategisch platziert sind, um Zugriffszeiten zu minimieren.
   
2. **Speicherung der Konstanten**: Im Literal Pool wird die Konstante, wie die 32-Bit-Adresse von `My_data`, abgelegt. Der `ldr`-Befehl wird so umgewandelt, dass er die relative Position dieser Konstante im Literal Pool aufruft.

3. **PC-relative Adressierung**: Der Befehl `ldr r0, =My_data` wird vom Assembler in `ldr r0, [pc, #offset]` übersetzt, wobei `offset` die relative Position im Literal Pool angibt. Der PC enthält während der Ausführung die Adresse der aktuell geladenen Anweisung, sodass der Assembler den Offset zur nächsten Konstanten berechnen kann.

#### Beispiel

Ein Beispiel verdeutlicht die Funktionsweise des Literal Pools:

```
.section .data
My_data:
    .word 0x12345678    @ Definiert den Wert von My_data
    
.section .text    
    LDR r0, =My_data    @ Lädt die Adresse von My_data in r0
    @ ... weiterer Code ...

```

Der Assembler setzt dies wie folgt um:

```
.section .text
    ...
    LDR r0, [PC, #offset]     @ Lädt die Adresse von My_data aus dem Literal Pool in r0
    @ ... weiterer Code ...
    
    @ Literal Pool
    .word My_data             @ Im Literal Pool wird die Adresse von My_data abgelegt
```

### Label in der Text-Sektion:
Im Code-Bereich eines Assemblers dienen Labels dazu, bestimmte Stellen im Programmfluss zu kennzeichnen:
```asm
.section .text
...
Here: b here
```
Das Label `Here` ist ein Platzhalter, nicht etwa für die Instruktion, die auf das Label folgt, sondern für die Adresse, bei der diese Instruktion im Speicher anfängt. Demnach handelt es sich bei dem Branchbefehl (Sprung) an dieser Stelle um eine Dauerschleife.

|-----------------------|-------------------------------|---------------------------------------------|
| [zurück](sektionen.md)| [Hauptmenü](../ueberblick.md) | [weiter](../Instruktionen/arithlogintro.md) |


| **2.2 Sektionen**                                                     |
|-----------------------------------------------------------------------|
| [2.2.1 Sektionen (Abschnitte)](sektionen.md)                          |
| [2.2.2 Label in ARM-Assembler](label.md)                              |







