# B.2 Erweiterungen der CPU-Funktionalität
## 2.3.8 VFP und NEON: Vektoren und Skalare

In der ARMv7-Architektur behandelt NEON jedes der **Q-Register** oder **D-Register** als **Vektor**, der aus mehreren Elementen gleicher Größe und desselben Typs besteht. Ein Vektor ist eine Datenstruktur, die mehrere Datenwerte enthält, die parallel verarbeitet werden können, was eine effizientere Bearbeitung großer Datenmengen ermöglicht. Diese Vektoren können aus 1, 2, 4, 8 oder 16 Elementen bestehen, abhängig von der Größe jedes einzelnen Elements. Ein 128-Bit Q-Register kann bis zu 16 Elemente umfassen, wenn diese 8 Bit groß sind, oder nur zwei Elemente, wenn sie 64 Bit groß sind.

Ein **Skalar** ist ein einzelnes Element innerhalb eines solchen Vektors. NEON erlaubt es, auf diese Elemente auch so zuzugreifen und sie zu bearbeiten, als wären sie eigenständige Werte. Dies ist besonders nützlich, wenn eine Operation nur auf einem bestimmten Element eines Vektors ausgeführt werden soll.

Bei der ARMv7-Architektur, die auch die NEON-Erweiterung unterstützt, sind die Vektorregister für die SIMD-Verarbeitung verantwortlich. Hier sind die spezifischen Details zu den Vektorregistern in ARMv7:


### Lanes

Im Kontext von NEON (und SIMD-Architekturen im Allgemeinen) bezieht sich der Begriff **"Lanes"** auf die einzelnen Datenpfade innerhalb eines Vektorregisters, die parallel verarbeitet werden können. Jede Lane repräsentiert eine Stelle im Vektorregister, die eine Einheit von Daten (z.B. ein 8-Bit-, 16-Bit-, 32-Bit- oder 64-Bit-Wert) enthält. 

#### Was bedeutet das im Detail?

Ein NEON-Vektorregister kann in mehrere Lanes unterteilt werden, wobei jede Lane eine bestimmte Anzahl von Bits hat:

- **8-Bit Lanes**: Ein 128-Bit Q-Register hat 16 Lanes.
- **16-Bit Lanes**: Ein 128-Bit Q-Register hat 8 Lanes.
- **32-Bit Lanes**: Ein 128-Bit Q-Register hat 4 Lanes.
- **64-Bit Lanes**: Ein 128-Bit Q-Register hat 2 Lanes.

Jede Lane kann unabhängig von den anderen Lanes denselben oder unterschiedliche Datenwerte enthalten, und SIMD-Instruktionen (Single Instruction, Multiple Data) können auf jeder Lane parallel dieselbe Operation ausführen.
Im Kontext des NEON-Coprozessors in ARMv7 bezieht sich der Begriff "Lanes" also auf die logischen Einheiten innerhalb der NEON-Register, die unabhängig voneinander arithmetische Operationen durchführen können. Jede Lane arbeitet mit einer festen Bitbreite, und Überträge bleiben innerhalb dieser Lane begrenzt, ohne in die nächste überzulaufen.
Die Suffixe bei NEON-Operationen bestimmen die Lane-Größe und den Datentyp. Suffixe wie `.8`, `.16`, `.32` oder `.64` spezifizieren die Größe der Lanes.
Diese Struktur ermöglicht es, mehrere Operationen gleichzeitig durchzuführen, jedoch in festen und unabhängigen Bereichen der Register, wodurch parallele Verarbeitung effizient wird, aber die Übertragungen zwischen den Einheiten verhindert werden.

#### Beispiel: 32-Bit Lanes in einem Q-Register

Angenommen, man hat ein 128-Bit Q-Register (`Q0`). Wenn man dieses Register in 32-Bit Lanes unterteilet, hat man 4 Lanes (`Lane 0`, `Lane 1`, `Lane 2`, und `Lane 3`). Eine SIMD-Operation, wie z.B. eine Addition, würde auf allen vier 32-Bit-Werten gleichzeitig ausgeführt werden. Das heißt, die Addition wird parallel in jeder Lane durchgeführt, was zu einem erheblichen Geschwindigkeitsgewinn führt, wenn viele ähnliche Operationen ausgeführt werden müssen.

|------------------------|-------------------------------|--------------------------------|
| [zurück](neonregs.md)  | [Hauptmenü](../ueberblick.md) | [weiter](neonadr.md)           |


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