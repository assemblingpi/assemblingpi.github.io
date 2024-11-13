# B.2 Erweiterungen der CPU-Funktionalität
## 2.3.1 VFP und NEON: Intro

In vielen Berechnungen – etwa in Wissenschaft, Computergrafik oder maschinellem Lernen – stoßen herkömmliche Ganzzahldarstellungen an ihre Grenzen, da sie weder Nachkommastellen noch große Zahlenwerte zuverlässig verarbeiten können. Die Floating-Point-Darstellung bietet hier eine Lösung, indem sie präzise Berechnungen über große Wertebereiche ermöglicht. Etwa bei trigonometrischen Funktionen, wie sie für exakte Winkel- und Längenberechnungen in Grafikanwendungen notwendig sind, ist Floating-Point unverzichtbar. 

In vielen Prozessoren ohne spezielle Hardware für Floating-Point-Operationen wird zusätzliche Software benötigt, um diese Berechnungen auszuführen. Der Cortex-A7-Prozessor hingegen verfügt über eine integrierte Floating-Point-Einheit, die es erlaubt, solche Berechnungen effizient durchzuführen. Dadurch werden Anwendungen, die auf genaue und schnelle numerische Berechnungen angewiesen sind, beschleunigt.

Trotz der vielen Vorteile der Floating-Point-Darstellung gibt es auch einige Einschränkungen. Da Computer nur eine **begrenzte Anzahl von Bits** zur Verfügung haben, um Zahlen zu speichern, ist die Präzision zwangsläufig **beschränkt**. Im Gegensatz zu reellen Zahlen, die unendlich viele Nachkommastellen besitzen können, stellt die Floating-Point-Darstellung nur eine **Annäherung an die tatsächlichen reellen Werte** dar. Dies führt zu unvermeidbaren Rundungsfehlern, die insbesondere in sensiblen Anwendungen sorgfältig berücksichtigt werden müssen.

|--------------------------------|------------------------------------|----------------------------|
|   [zurück](../Timer/delay_lsg.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](bingleit.md)    |


|**2.3 VFP und NEON**                                                                                               |
|-------------------------------------------------------------------------------------------------------------------|
| [2.3.1 Intro](floatingintro.md)                                                                                   |
| [2.3.2 Gleitkommazahlen](bingleit.md)                                                                             |
| [2.3.3 Floating Point Format nach IEEE 754](floatingnums.md)                                                      |
| [2.3.4 VFP (Vector Floating Point) in der ARM-Architektur](vfp_intro.md)                                          |
| [2.3.5 VFP Data Conversion Befehle](vfpconv.md)                                                                   |
| [2.3.6 Was ist NEON?](neonintro.md)                                                                               |
| [2.3.7 Überblick über die ARMv7 NEON-Register](neonregs.md)                                                       |
| [2.3.8 Vektoren und Skalare](scalvekt.md)                                                                         |
| [2.3.9 Registeradressierung in NEON](neonadr.md)                                                                  |
| [2.3.10 Das NEON und Floatingpoint Status Register](neonstat.md)                                                  |
| [2.3.11 Steuerung und Statusübertragung zwischen ARM- und NEON/VFP-Statusregistern (VMSR und VMRS)](neonctrl.md)  |
| [2.3.12 NEON Instruktionen](neoninstr.md)                                                                         |
| [2.3.13 Datentransfer](vmov.md)                                                                                   |
| [2.3.14 NEON Load/Store Instruktionen](neonldstr.md)                                                              |
| [2.3.15 Arithmetische und logische NEON-Operationen](varithlog.md)                                                |
| [2.3.16 VTRN (Vector Transpose) Instruktionen](vtrn.md)                                                           |
| [2.3.17 Implementierung von Trigonometrischen Funktionen](trigon_ue.md)                                           |
| [2.3.18 Implementierung einer 4x4-Matrixmultiplikationsfunktion mit NEON](matrix_ue.md)                           |