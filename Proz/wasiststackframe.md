# A.5 Prozedurale Programmierung
## 5.2.3 Was ist eine Prozedur: Was ist der Stackframe einer Prozedur?

Ein Stackframe ist der Speicherbereich auf dem Stack, der einer Funktion bei ihrem Aufruf zugewiesen wird. Er enthält die **Rücksprungadresse** und **lokale Variablen**. Zudem dient der Stackframe auch zum **Speichern von Registern**, die innerhalb der Funktion verändert werden, um den Zustand der aufrufenden Funktion wiederherstellen zu können. Der Stackframe wird beim Funktionsaufruf erstellt und beim Verlassen der Funktion wieder freigegeben. Er ermöglicht es, Daten und Rückkehradressen einer Funktion zu verwalten, ohne andere Funktionen zu beeinflussen.

### Bestandteile eines Stackframes
**Return-Adresse:** Die Adresse, zu der der Programmfluss nach Beendigung der Funktion zurückkehrt.

**Frame Pointer (FP):** Der vorherige Wert des Frame Pointers, der es ermöglicht, auf lokale Variablen und Parameter der aufrufenden Funktion zuzugreifen.

**Lokale Variablen:** Speicherbereich für Variablen, die nur innerhalb der Funktion gültig sind.

**Speicher für Register:** Falls nötig, werden Registerinhalte gesichert, um den Zustand der aufrufenden Funktion wiederherzustellen.

### Erstellung und Zerstörung eines Stackframes

**Prolog**: Der Stackframe wird zu Beginn der Funktion erstellt. Der Frame Pointer wird auf den "Boden" des aktuellen Stackframes gesetzt, damit man auf die lokalen Parameter und Variablen der Funktion am Stack über einen **Offset zum Framepointer** zugreifen kann.

#### Beispiel für die Erstellung eines Stackframes:
```asm
funktion:
     PUSH {r11}     @ 1. alten Frame-Pointer sichern
     MOV r11, sp    @ 2. Stackpointer im Framepointer sichern
```
Da bei `(1)` der Framepointer auf dem Stack gesichert wurde, zeigt der SP nach `(1)` auf den **alten Framepointer** und dementsprechend beinhaltet der FP bei `(2)` nun auch einen Zeiger auf seinen alten Wert. Dadurch, dass der Framepointer nun auf den Boden des neuen Stackframes zeigt, kann man über ein Offset zum Framepointer auf die Parameter und lokalen Variablen zugreifen.

**Epilog**: Am Ende der Funktion wird der Stackframe aufgelöst. Der Stack Pointer wird zurückgesetzt, so dass er wieder auf den alten Framepointer zeigt und der Framepointer wird wiederhergestellt, um den Zustand der aufrufenden Funktion wiederherzustellen.

#### Beispiel für die Zerstörung eines Stack-Frames:
```asm
     MOV sp, r11    @ Stack-Pointer auf Frame-Pointer zurücksetzen
     POP {r11}      @ alten Framepointer wiederherstellen
     BX lr          @ Rückkehr zur aufrufenden Funktion
```
Der Stackframe sorgt dafür, dass jede Funktion ihre eigenen Daten verwalten kann, ohne die Daten anderer Funktionen zu beeinträchtigen.

|-----------------------------|------------------------------------|----------------------------------|
|   [zurück](wasiststack.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](wasistcalltree.md)    |


| **5.2 Was Was ist eine Prozedur**                                             |
|-------------------------------------------------------------------------------|
| [5.2.1 Intro](wasistproz.md)                                                  |
| [5.2.2 Der Stack](wasiststack.md)                                             |
| [5.2.3 Was ist der Stackframe einer Prozedur?](wasiststackframe.md)           |
| [5.2.4 Was ist der Calltree?](wasistcalltree.md)                              |
| [5.2.5 Prozeduren, Link Register und Stack](prozlrstack.md)                   |
| [5.2.6 Parameterübergabe und Rückgabewerte](param.md)                         |
| [5.2.7 Finde das geheime Passwort!](disasm_ue.md)                             |