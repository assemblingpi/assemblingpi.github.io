# B.1 Einführung
## 1.9.5 Interrupts: Interrupts im Raspberry Pi 2B

### Struktur des Interrupt-Systems des BCM2836
Jeder der vier Prozessor-Kerne besitzt einen eigenen lokalen Interrupt-Controller, der speziell für die Verwaltung von lokalen Interrupts, wie Timer- und Mailbox-Interrupts, zuständig ist. Zusätzlich existiert ein globaler Interrupt-Controller, der dafür verantwortlich ist, Interrupts von Peripheriegeräten, wie der GPU, an die verschiedenen Kerne zu verteilen.

### Interrupt-Verarbeitung bei ARMv7-A Prozessoren
Wenn ein Interrupt auftritt, erzwingt der Prozessor einen Sprung zum entsprechenden Eintrag in der Vektortabelle. 
Je nach Quelle bzw. Ursache des Interrupts wird also eine spezifische Adresse in das Program Counter Register (PC) geladen, wodurch der Prozessor zum entsprechenden Interruptvektor verzweigt, der ihn wiederum zur ISR weiterleitet.

|-----------------------|------------------------------------|---------------------------|
|   [zurück](ictrl.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](armvekt.md)    |