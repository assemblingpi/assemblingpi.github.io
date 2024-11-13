# B.2 Erweiterungen der CPU-Funktionalität
## 2.3.6 VFP und NEON: Was ist NEON?
NEON ist eine SIMD-Erweiterung (Single Instruction, Multiple Data) innerhalb von ARM-Prozessoren, die entwickelt wurde, um die Effizienz bei datenintensiven Anwendungen wie Multimedia, Signalverarbeitung und 3D-Grafik zu steigern. Diese Technologie ermöglicht die parallele Ausführung von Operationen auf Vektordaten in 128-Bit-Registerblöcken, was zu einer signifikanten Leistungssteigerung führt.

Mit NEON können mehrere Datenelemente – entweder Integer oder Floating-Point – gleichzeitig verarbeitet werden. Dadurch reduziert sich der Rechenaufwand für Aufgaben, die traditionell sequenziell durchgeführt werden, erheblich. Dies macht NEON besonders wertvoll für Anwendungen, in denen große Datenmengen schnell verarbeitet werden müssen, wie zum Beispiel in der Signalverarbeitung und bei grafikintensiven Anwendungen.

Ein Beispiel verdeutlicht die Funktionsweise: Wenn zwei 128-Bit NEON-Register als Arrays von 16 Elementen interpretiert werden, wobei jedes Element 8 Bit groß ist, kann eine NEON-Instruktion wie **VADD.I8** diese beiden Register in einem Schritt verarbeiten. Das bedeutet, dass 16 Additionen von 8-Bit-Zahlen gleichzeitig ausgeführt werden. Jedes der 16 Elemente im ersten Register wird mit dem entsprechenden Element im zweiten Register addiert, und das Ergebnis wird in einem dritten Register gespeichert – alles innerhalb einer einzigen Instruktion.

Durch diese parallele Verarbeitung kann NEON Operationen, die normalerweise viele sequenzielle Instruktionen erfordern würden, erheblich beschleunigen. Diese Fähigkeit ist besonders nützlich in Bereichen, in denen große Datenmengen effizient verarbeitet werden müssen

Der gegebene Code aktiviert den NEON-Coprozessor und ist in `boot.s` zu ergänzen:
```
@ enable Neon-Coprozessor
mrc p15, 0, r0, c1, c1, 2
orr r0, r0, #(3<<10)			@ enable neon
bic r0, r0, #(3<<14)			@ clear nsasedis/nsd32dis
mcr p15, 0, r0, c1, c1, 2
ldr r0, =(0xF << 20)
mcr p15, 0, r0, c1, c0, 2
mov r3, #0x40000000 
vmsr FPEXC, r3
```

|-----------------------|-------------------------------|---------------------------------|
| [zurück](vfpconv.md)  | [Hauptmenü](../ueberblick.md) | [weiter](neonregs.md)           | 


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