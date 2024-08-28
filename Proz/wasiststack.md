## Was ist der Stack?

Der Stack ist von zentraler Bedeutung für die Realisierung von Prozeduren auf Maschinennaher Ebene. Er fungiert als spezieller Speicherbereich, der vor allem für lokale Variablen, Rücksprungadressen und andere temporäre Daten verwendet wird. Der Stack arbeitet nach dem LIFO-Prinzip (Last In, First Out), was bedeutet, dass die zuletzt eingefügten Daten zuerst wieder entfernt werden. 

Die Interaktion mit dem Stack erfolgt über die Befehle `PUSH` und `POP`:

**PUSH**: Dieser Befehl legt Daten auf den Stack und reduziert dabei den Stack-Pointer (`SP`), da
      der Stack in ARM abwärts, also zu niedrigeren Speicheradressen wächst.
      
**POP**:  Dieser Befehl entfernt Daten vom Stack, legt sie in ein Register und erhöht den Stack-
 Pointer entsprechend.

Das LIFO-Prinzip stellt sicher, dass die zuletzt gespeicherten Daten als erstes wieder abgerufen werden, was für die korrekte Verwaltung von Funktionsaufrufen und Rückkehradressen entscheidend ist.

**Full Descending Stack**

ARM-Prozessoren implementieren einen Full-Descending Stack. Das ist ein Stack, bei dem der Stackpointer (SP) auf die letzte belegte(!) Speicheradresse zeigt und beim Hinzufügen neuer Daten in Richtung niedrigerer Adressen verschoben wird. Das bedeutet, dass bei einem PUSH-Befehl der Stackpointer erst reduziert und dann die neuen Daten gespeichert werden, und bei einem POP-Befehl die Daten zuerst entfernt und dann der Stackpointer erhöht wird.

[weiter](wasiststackframe.md)