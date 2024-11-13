# A.5 Prozedurale Programmierung
## 5.2.1 Was ist eine Prozedur: Intro

In der Assemblerprogrammierung sind Prozeduren grundlegende Bauelemente, die die Struktur und Organisation von Programmen erheblich verbessern. Sie ermöglichen die Wiederverwendbarkeit von Code und die effiziente Verwaltung von Daten und Kontrollfluss, wodurch komplexe Aufgaben übersichtlich und **modular** umgesetzt werden können.

### Prozeduren: Wiederverwendbare Codeblöcke
Eine Prozedur ist eine abgeschlossene Sammlung von Anweisungen, die eine spezifische Aufgabe ausführt und unabhängig vom restlichen Programmablauf funktioniert. Durch das Bündeln von Befehlen unter einem Namen können Prozeduren mehrfach aufgerufen werden, ohne den Code zu duplizieren. Dies fördert die Modularität und Wiederverwendbarkeit, was die Wartung und Fehlerbehebung erheblich vereinfacht, da Änderungen nur einmal in der Prozedur vorgenommen werden müssen.

Prozeduren mit Rückgabewerten werden oft als Funktionen bezeichnet und ermöglichen es, Ergebnisse von Berechnungen oder Operationen zurückzugeben, die im aufrufenden Kontext weiterverwendet werden können. Die aufgerufene Prozedur kann Ergebnisse über Register oder Speicher zurückgeben, sodass sie im aufrufenden Kontext weiterverwendet werden können. Dieses Konzept erweitert die Flexibilität von Prozeduren und ermöglicht eine einfache, klare Struktur für Berechnungen und Datenverarbeitung.

### Prozeduren als abgeschlossene Boxen
Stellen Sie sich eine Prozedur als eine geschlossene Box vor, die klar definierte Eingänge und Ausgänge hat. Über diese Eingänge erhält die Prozedur die Werte, die sie verarbeitet, und durch die Ausgänge gibt sie Ergebnisse zurück.  Innerhalb dieser Box wird bei jedem Aufruf ein neuer Stack-Frame erstellt, der die lokale Umgebung der Prozedur enthält. Es liegt jedoch in der Verantwortung des Programmierers, benötigte Register zu sichern und zu initialisieren. Auf diese Weise kann die Prozedur unabhängig und unbeeinflusst von äußeren Faktoren arbeiten und bleibt auf die ihr zugewiesene Aufgabe beschränkt.

Diese Isolation ist zentral für die Zuverlässigkeit des Codes. Die Prozedur agiert vollständig in ihrem eigenen Bereich und benötigt keine Informationen über den restlichen Programmablauf. Was vor ihrem Aufruf passiert ist, ist für sie irrelevant, abgesehen von den übergebenen Eingabewerten.

### Struktur und Steuerung des Programmflusses
Jede Prozedur besteht aus einem oder mehreren **Basic Blocks**. Der Programmfluss wird durch spezifische Aufruf- und Rückkehrbefehle gesteuert. Beim Aufruf springt der Programmfluss zum ersten Basic Block der Prozedur. Gleichzeitig wird die Rücksprungadresse gespeichert, sodass das Programm nach Abschluss der Prozedur an der richtigen Stelle fortgesetzt werden kann.

Nach der Ausführung sorgt eine Rückkehranweisung dafür, dass der Programmfluss zur aufrufenden Stelle zurückkehrt. Dazu wird der aktuelle Stack-Frame entfernt und die Rücksprungadresse wiederhergestellt. Dieses Prinzip ermöglicht einen kontrollierten, strukturierten Ablauf und trägt zur Klarheit und Vorhersehbarkeit des Codes bei.

### Die Verantwortung des Programmierers
Unsere Aufgabe als Programmierer besteht darin, die Illusion einer isolierten Box zu wahren. Konkret heißt das, dass alle Register und Speicherbereiche, die außerhalb der Prozedur wichtig sind, gesichert und nach der Ausführung wieder in ihren ursprünglichen Zustand zurückversetzt werden müssen. Auf diese Weise verhindern wir, dass die Prozedur unbeabsichtigte Seiteneffekte auf andere Teile des Programms hat und sichern die Integrität des Gesamtsystems.

|--------------------------|------------------------------------|-------------------------------|
|   [zurück](prozprog.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](wasiststack.md)    |


| **5.2 Was Was ist eine Prozedur**                                             |
|-------------------------------------------------------------------------------|
| [5.2.1 Intro](wasistproz.md)                                                  |
| [5.2.2 Der Stack](wasiststack.md)                                             |
| [5.2.3 Was ist der Stackframe einer Prozedur?](wasiststackframe.md)           |
| [5.2.4 Was ist der Calltree?](wasistcalltree.md)                              |
| [5.2.5 Prozeduren, Link Register und Stack](prozlrstack.md)                   |
| [5.2.6 Parameterübergabe und Rückgabewerte](param.md)                         |
| [5.2.7 Finde das geheime Passwort!](disasm_ue.md)                             |