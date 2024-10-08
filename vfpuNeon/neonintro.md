## NEON: Was ist das?
NEON ist eine SIMD-Erweiterung (Single Instruction, Multiple Data) innerhalb von ARM-Prozessoren, die entwickelt wurde, um die Effizienz bei datenintensiven Anwendungen wie Multimedia, Signalverarbeitung und 3D-Grafik zu steigern. Diese Technologie ermöglicht die parallele Ausführung von Operationen auf Vektordaten in 128-Bit-Registerblöcken, was zu einer signifikanten Leistungssteigerung führt.

Mit NEON können mehrere Datenelemente – entweder Integer oder Floating-Point – gleichzeitig verarbeitet werden. Dadurch reduziert sich der Rechenaufwand für Aufgaben, die traditionell sequenziell durchgeführt werden, erheblich. Dies macht NEON besonders wertvoll für Anwendungen, in denen große Datenmengen schnell verarbeitet werden müssen, wie zum Beispiel in der Signalverarbeitung und bei grafikintensiven Anwendungen.

Ein Beispiel verdeutlicht die Funktionsweise: Wenn zwei 128-Bit NEON-Register als Arrays von 16 Elementen interpretiert werden, wobei jedes Element 8 Bit groß ist, kann eine NEON-Instruktion wie **VADD.I8** diese beiden Register in einem Schritt verarbeiten. Das bedeutet, dass 16 Additionen von 8-Bit-Zahlen gleichzeitig ausgeführt werden. Jedes der 16 Elemente im ersten Register wird mit dem entsprechenden Element im zweiten Register addiert, und das Ergebnis wird in einem dritten Register gespeichert – alles innerhalb einer einzigen Instruktion.

Durch diese parallele Verarbeitung kann NEON Operationen, die normalerweise viele sequenzielle Instruktionen erfordern würden, erheblich beschleunigen. Diese Fähigkeit ist besonders nützlich in Bereichen, in denen große Datenmengen effizient verarbeitet werden müssen

Der gegebene Code aktiviert den NEON-Coprozessor und ist in `boot.s` zu ergänzen:
```
    @ enable Neon-Coprozessor
    mrc p15, 0, r0, c1, c1, 2
	orr r0, r0, #(3<<10)          @ enable neon
	bic r0, r0, #(3<<14)          @ clear nsasedis/nsd32dis
	mcr p15, 0, r0, c1, c1, 2
	ldr r0, =(0xF << 20)
	mcr p15, 0, r0, c1, c0, 2
	mov r3, #0x40000000 
	vmsr FPEXC, r3
```

|-----------------------|-------------------------------|---------------------------------|
| [zurück](vfpconv.md)  | [Hauptmenü](../ueberblick.md) | [weiter](neonregs.md)           | 