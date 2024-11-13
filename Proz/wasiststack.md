# A.5 Prozedurale Programmierung
## 5.2.2 Was ist eine Prozedur: Der Stack

Der **Stack** spielt eine zentrale Rolle bei der Verwaltung von Daten und dem Programmfluss, insbesondere bei der Ausführung von **Prozeduren**. Er fungiert als spezieller Bereich im Hauptspeicher, der hauptsächlich für **Rücksprungadressen**, **lokale Variablen** und andere **temporäre Daten** verwendet wird. Der Stack arbeitet nach dem **Last-In-First-Out (LIFO)**-Prinzip, was bedeutet, dass die zuletzt hinzugefügten Daten als erstes wieder entfernt werden. Diese Eigenschaft macht den Stack ideal für die Verwaltung von Prozeduraufrufen, Rücksprungadressen und lokalen Variablen.

### **Full Descending Stack in der ARM-Architektur**

ARM-Prozessoren implementieren einen **Full-Descending Stack**, bei dem der **Stack Pointer (SP)** auf die zuletzt belegte Speicheradresse zeigt und beim Hinzufügen neuer Daten in Richtung niedrigerer Adressen verschoben wird (Descending). **Full** bedeutet in diesem Kontext, dass bei einem PUSH-Befehl der Stackpointer erst reduziert und dann die neuen Daten gespeichert werden, und bei einem POP-Befehl die Daten zuerst entfernt und dann der Stackpointer erhöht wird, so dass an der aktuellen Adresse, die im `SP` hinterlegt ist immer der zuletzt abgelegte Wert liegt.

### Interaktion mit dem Stack: PUSH und POP

Die Interaktion mit dem Stack erfolgt hauptsächlich über die Befehle **`PUSH`** und **`POP`**, die es ermöglichen, Daten effizient zu speichern und wieder abzurufen:

- **PUSH:** Dieser Befehl legt Daten auf den Stack, indem er den Stack Pointer (`SP`) reduziert und die Daten an der neuen Position speichert.
  ```assembly
  PUSH {R4} 
  ```

- **POP:** Dieser Befehl entfernt Daten vom Stack, legt sie in ein Register und erhöht den Stack Pointer entsprechend.
  ```assembly
  POP {R4} 
  ```

Das **LIFO-Prinzip** stellt sicher, dass die zuletzt gespeicherten Daten als erstes wieder abgerufen werden, was entscheidend für die korrekte Rückkehr von Prozeduren ist.

### Vorteile der Stack-Nutzung

- **Modularität:** Der Stack ermöglicht es, Prozeduren unabhängig voneinander zu verwalten, wodurch der Code übersichtlicher und wartbarer wird.
- **Wiederverwendbarkeit:** Prozeduren können mehrfach aufgerufen werden, ohne dass ihre internen Zustände kollidieren, da jeder Aufruf seinen eigenen Speicherbereich auf dem Stack erhält.
- **Effiziente Speicherverwaltung:** Der Stack sorgt für eine dynamische und effiziente Nutzung des Speichers, indem er nur den benötigten Speicherplatz reserviert und nach der Prozedurausführung freigibt.

|----------------------------|------------------------------------|------------------------------------|
|   [zurück](wasistproz.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](wasiststackframe.md)    |


| **5.2 Was Was ist eine Prozedur**                                             |
|-------------------------------------------------------------------------------|
| [5.2.1 Intro](wasistproz.md)                                                  |
| [5.2.2 Der Stack](wasiststack.md)                                             |
| [5.2.3 Was ist der Stackframe einer Prozedur?](wasiststackframe.md)           |
| [5.2.4 Was ist der Calltree?](wasistcalltree.md)                              |
| [5.2.5 Prozeduren, Link Register und Stack](prozlrstack.md)                   |
| [5.2.6 Parameterübergabe und Rückgabewerte](param.md)                         |
| [5.2.7 Finde das geheime Passwort!](disasm_ue.md)                             |