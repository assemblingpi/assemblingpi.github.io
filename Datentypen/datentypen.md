# A.4 Datentypen 
## 4.1 Datentypen allgemein

Programme arbeiten mit Informationen ganz unterschiedlicher Art. Diese Informationen können **Texte** sein, die vom Benutzer eingegeben oder angezeigt werden, **Ganzzahlen** für einfache mathematische Berechnungen, **Gleitkommazahlen** für präzisere, wissenschaftliche Berechnungen oder **Zeiger**, um auf bestimmte Speicheradressen zuzugreifen. Für jede dieser Informationen gibt es spezifische Datentypen, die bestimmen, wie groß die Daten sind, wie sie strukturiert sind und welche Operationen darauf angewendet werden können.

Auf Maschinenebene beziehen sich Datentypen auf die Art und Weise, wie Daten im Speicher und in Registern des Prozessors interpretiert werden. Es gibt **keinen physischen Unterschied** zwischen einem 32-Bit-Wert im Speicher, der als Ganzzahl, Gleitkommazahl oder Zeiger verwendet wird – letztlich sind all diese Daten nur **Bitmuster**. Der Unterschied liegt einzig in der **Interpretation** dieser Werte im Rahmen eines Programms, das heißt darin, welche Operationen an ihnen durchgeführt werden. 

Assemblerprogramme bieten keine eingebauten Datentypen, sondern stellen spezielle Instruktionen bereit, die für bestimmte Daten geeignet sind. Daher liegt es in der Verantwortung des Programmierers, die Bitmuster so zu interpretieren, dass die gewünschten Ergebnisse erzielt werden. Beispielsweise muss eine Zeichenkette korrekt als solche interpretiert werden, damit entsprechende Operationen richtig ausgeführt werden. Diese Aufgabe übernimmt in Hochsprachen wie C der Compiler und stellt sicher, dass der Datentyp einer Variable im Rahmen des Programms korrekt interpretiert wird. In Assembler jedoch muss der Programmierer selbst darauf achten, die passenden Instruktionen zu wählen.

Einige Operationen sind unabhängig vom Datentyp, wie beispielsweise die Addition zweier binärer Werte. Es ist für die Maschine irrelevant, ob sie mit vorzeichenbehafteten oder vorzeichenlosen Ganzzahlen arbeitet. Der Assemblercode für die Addition bleibt gleich, so entspricht folgender C-Code:
```
unsigned int a = 10, b = 20;
unsigned int ures = a + b;
```
folgendem Assemblercode:

```
    mov     r0, #10
    mov     r1, #20
    add     r0, r0, r1
```
Dasselbe gilt für vorzeichenbehaftete Werte:
```
int c = -15, d = 25;
int sres = c + d;
```

führt zu folgendem Assemblercode:
```
    mvn     r0, #14          @ r0 = -15
    mov     r1, #25
    add     r0, r0, r1
```
Der Unterschied wird erst bei der Interpretation von Statusflags relevant, die für vorzeichenlose und vorzeichenbehaftete Ganzzahlen unterschiedlich ausgewertet werden. Der Datentyp beeinflusst auch die Art der Verschiebungsoperationen. Bei vorzeichenlosen Ganzzahlen wird ein logischer Rechtsshift verwendet, während für vorzeichenbehaftete Werte ein arithmetischer Rechtsshift erforderlich ist:

```
unsigned int ushift = ures >> 1;
int sshift = sres >> 1;
```
```
lsr     r0, r0, #1    @ logischer Shift
    ...
asr     r0, r0, #1    @ arithmetischer Shift
```
Die Zeigerarithmetik folgt ähnlichen Regeln wie Ganzzahloperationen. Bei der Operation ptr++ beispielsweise wird der Wert des Zeigers um die Größe des Datentyps erhöht. Ist ptr ein Zeiger auf einen int, wird ptr um 4 Bytes, also die Größe eines int, erhöht:
```
int * ptr = &a;
ptr++;
```
```
add     r0, r0, #4
```
Gleitkommazahlen wiederum benötigen spezielle Instruktionen, die in vielen modernen Prozessoren vorhanden sind und präzise wissenschaftliche Berechnungen ermöglichen. Beispielsweise wird der folgende C-Code:
```
float a = 1.23;
a += 1;
```
Durch den Compiler in folgende Assemblerbefehle umgesetzt:
```
...
vmov.f32        s2, #1.000000e+00
vadd.f32        s0, s0, s2
```

### Merke: Datentypen in der Assemblerprogrammierung und die Verantwortung des Programmierers

Im Speicher existieren Datentypen **nicht physisch** – alle Daten werden als Bitmuster abgelegt, deren Bedeutung allein durch die Interpretation im Programm, also durch die **Wahl der Instruktionen**, entsteht. Der Programmierer trägt daher die Verantwortung, passende Instruktionen auszuwählen, damit die Bitmuster korrekt entsprechend ihres Datentyps verarbeitet werden.

|-------------------------------------------|------------------------------------|---------------------------------|
|   [zurück](../Statemachine/Statemach.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](einfachdtypen.md)    |


