# B.2 Erweiterungen der CPU-Funktionalität
## 2.3.1 VFP und NEON: Intro

In zahlreichen Berechnungen, besonders in Bereichen wie der Wissenschaft, Grafikverarbeitung oder dem maschinellen Lernen, stehen wir häufig vor der Herausforderung, mit extrem großen, kleinen oder äußerst präzisen Zahlen arbeiten zu müssen. Herkömmliche Ganzzahldarstellungen reichen in solchen Fällen nicht aus, da sie weder für das Rechnen mit Nachkommastellen, noch mit besonders große Zahlenwerten geeignet sind.

Um diese Herausforderung zu bewältigen, kommt die Floating-Point-Darstellung zum Einsatz. 

In vielen Prozessoren ohne spezielle Hardware für Floating-Point-Operationen wird zusätzliche Software benötigt, um diese Berechnungen auszuführen. Der Cortex-A7-Prozessor hingegen verfügt über eine integrierte Floating-Point-Einheit, die es erlaubt, solche Berechnungen effizient durchzuführen. Dadurch werden Anwendungen, die auf genaue und schnelle numerische Berechnungen angewiesen sind, beschleunigt.

Trotz der vielen Vorteile der Floating-Point-Darstellung gibt es auch einige Einschränkungen. Da Computer nur eine **begrenzte Anzahl von Bits** zur Verfügung haben, um Zahlen zu speichern, ist die Präzision zwangsläufig **beschränkt**. Im Gegensatz zu reellen Zahlen, die unendlich viele Nachkommastellen besitzen können, stellt die Floating-Point-Darstellung nur eine **Annäherung an die tatsächlichen reellen Werte** dar. Dies führt zu unvermeidbaren Rundungsfehlern, die insbesondere in sensiblen Anwendungen sorgfältig berücksichtigt werden müssen.

|--------------------------------|------------------------------------|----------------------------|
|   [zurück](../Timer/delay.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](bingleit.md)    |