## Was ist der Stackframe einer Prozedur?
Ein Stackframe ist der Speicherbereich auf dem Stack, der einer Funktion bei ihrem Aufruf zugewiesen wird. Er enthält die Rücksprungadresse, lokale Variablen und eventuell gespeicherte Register. Der Stackframe wird beim Funktionsaufruf erstellt und beim Verlassen der Funktion wieder freigegeben. Er ermöglicht es, Daten und Rückkehradressen einer Funktion zu verwalten, ohne andere Funktionen zu beeinflussen.

### Bestandteile eines Stackframes
**Return-Adresse:** Die Adresse, zu der der Programmfluss nach Beendigung der Funktion zurückkehrt.

**Frame Pointer (FP):** Der vorherige Wert des Frame Pointers, der es ermöglicht, auf lokale Variablen und Parameter der aufrufenden Funktion zuzugreifen.

**Lokale Variablen:** Speicherbereich für Variablen, die nur innerhalb der Funktion gültig sind.
Speicher für Register: Falls nötig, werden Registerinhalte gesichert, um den Zustand der aufrufenden Funktion wiederherzustellen.

### Erstellung und Zerstörung eines Stackframes

**Prolog**: Der Stackframe wird zu Beginn der Funktion erstellt. Der Frame Pointer wird auf den 
"Boden" des aktuellen Stackframes gesetzt, damit man auf die lokalen Parameter und 
Variablen der Funktion am Stack über einen Offset zum Framepointer zugreifen kann.

#### Beispiel für die Erstellung eines Stackframes:
```asm
funktion:
     PUSH {r11}     @ 1. alten Frame-Pointer sichern
     MOV r11, sp    @ 2. Stackpointer im Framepointer sichern
```
Da bei (1) der Framepointer auf dem Stack gesichert wurde, zeigt der SP nach (1) auf den alten Framepointer und dementsprechend beinhaltet der FP bei (2) nun auch einen Zeiger auf seinen alten Wert. Dadurch, dass der Framepointer nun auf den Boden des neuen Stackframes zeigt, kann man über ein Offset zum Framepointer auf die Parameter und lokalen Variablen zugreifen.

**Epilog**: Am Ende der Funktion wird der Stackframe aufgelöst. Der Stack Pointer wird 
zurückgesetzt, und der Frame Pointer wird wiederhergestellt, um den Zustand der 
aufrufenden Funktion wiederherzustellen.

#### Beispiel für die Zerstörung eines Stack-Frames:
```asm
     MOV sp, r11    @ Stack-Pointer auf Frame-Pointer zurücksetzen
     POP {r11}      @ alten Framepointer wiederherstellen
     BX lr          @ Rückkehr zur aufrufenden Funktion
```
Der Stackframe sorgt dafür, dass jede Funktion ihre eigenen Daten verwalten kann, ohne die Daten anderer Funktionen zu beeinträchtigen.

[weiter](wasistcalltree.md)