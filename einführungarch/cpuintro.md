# A.1 Einführung
## 1.2.1 Grundlagen der Computerarchitektur: Die Ausführung von Programmen durch den Computer

Ein Computer führt Programme aus, indem er deren Anweisungen verarbeitet und Daten manipuliert. Im Zentrum dieses Prozesses steht die **CPU (Central Processing Unit)**, die als Mechanismus fungiert, der Programme umsetzt. Die CPU beginnt, indem sie Anweisungen aus dem Hauptspeicher lädt, der vereinfacht betrachtet wie ein lineares Array von Daten und Instruktionen organisiert ist. Diese Anweisungen werden im **Program Counter (PC)**, einem speziellen Register, verfolgt, das die Adresse der nächsten auszuführenden Anweisung enthält. Die **Kontrolleinheit** dekodiert die geladenen Anweisungen und bestimmt, welche Aktionen als nächstes ausgeführt werden sollen, sei es die fortlaufende Ausführung oder eine Verzweigung zu einem anderen Teil des Programms.

Während die Kontrolleinheit die Reihenfolge der Ausführung steuert, nutzt die **arithmetisch-logische Einheit (ALU)** die Daten, die in den **Registern**, schnellen Zwischenspeichern innerhalb der CPU, gespeichert sind. Diese Register ermöglichen der ALU einen schnellen Zugriff auf die benötigten Informationen, um arithmetische Berechnungen und logische Vergleiche effizient durchzuführen. Die Ergebnisse dieser Operationen können den weiteren Programmfluss beeinflussen, indem sie Bedingungen für Verzweigungen festlegen.

Der **Hauptspeicher** speichert sowohl den Programmcode als auch die zu verarbeitenden Daten. Nachdem die ALU die Daten verarbeitet hat, werden die Ergebnisse entweder in den Registern gehalten oder zurück in den Speicher geschrieben, um für zukünftige Anweisungen verfügbar zu sein. Dieses kontinuierliche Laden, Dekodieren, Verarbeiten und Speichern gewährleistet eine reibungslose und effiziente Ausführung der Programme.

Zusätzlich ermöglichen **Eingabe-/Ausgabegeräte (I/O)** dem Computer, mit externen Quellen und Zielen zu kommunizieren. Daten von diesen Geräten werden über den **Systembus** zur CPU übertragen, während verarbeitete Daten an die Ausgabegeräte gesendet werden. Der Systembus fungiert als Kommunikationskanal, der den Datenaustausch zwischen der CPU, dem Speicher und den I/O-Geräten ermöglicht, wodurch die Kontrolleinheit Anweisungen und Daten effizient zwischen den Komponenten hin- und herschieben kann.

|----------------------------------|--------------------------|-----------------------|
| [zurück](../wasistprog/disasm.md)| [Hauptmenü](../index.md) | [weiter](archintro.md)| 


| **1.2 Grundlagen der Computerarchitektur**                                                |
|-------------------------------------------------------------------------------------------|
| [1.2.1 Die Ausführung von Programmen durch den Computer](../einführungarch/cpuintro.md)   |
| [1.2.2 Von-Neumann-Architektur](../einführungarch/archintro.md)                           |
| [1.2.3 Der Speicher](../einführungarch/memintro.md)                                       |
| [1.2.4 Cache](../einführungarch/cache.md)                                    |
| [1.2.5 Ein- und Ausgabegeräte](../einführungarch/eaintro.md)                              |
| [1.2.6 Der Systembus](../einführungarch/sbusintro.md)                                     |
| [1.2.7 Der Befehlszyklus](../einführungarch/archintro_pip.md)                             |