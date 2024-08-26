## Vektoren und Skalare

In der ARMv7-Architektur behandelt NEON jedes der **Q-Register** oder **D-Register** als **Vektor**, der aus mehreren Elementen gleicher Größe und desselben Typs besteht. Ein Vektor ist eine Datenstruktur, die mehrere Datenwerte enthält, die parallel verarbeitet werden können, was eine effizientere Bearbeitung großer Datenmengen ermöglicht. Diese Vektoren können aus 1, 2, 4, 8 oder 16 Elementen bestehen, abhängig von der Größe jedes einzelnen Elements. Ein 128-Bit Q-Register kann bis zu 16 Elemente umfassen, wenn diese 8 Bit groß sind, oder nur zwei Elemente, wenn sie 64 Bit groß sind.

Ein **Skalar** ist ein einzelnes Element innerhalb eines solchen Vektors. NEON erlaubt es, auf diese einzelnen Elemente zuzugreifen und sie zu bearbeiten, als wären sie eigenständige Werte. Dies ist besonders nützlich, wenn eine Operation nur auf einem bestimmten Element eines Vektors ausgeführt werden soll.

Bei der ARMv7-Architektur, die auch die NEON-Erweiterung unterstützt, sind die Vektorregister für die SIMD-Verarbeitung verantwortlich. Hier sind die spezifischen Details zu den Vektorregistern in ARMv7:


## Lanes

Im Kontext von NEON (und SIMD-Architekturen im Allgemeinen) bezieht sich der Begriff **"Lanes"** auf die einzelnen Datenpfade innerhalb eines Vektorregisters, die parallel verarbeitet werden können. Jede Lane repräsentiert eine Stelle im Vektorregister, die eine Einheit von Daten (z.B. ein 8-Bit-, 16-Bit-, 32-Bit- oder 64-Bit-Wert) enthält. 

### Was bedeutet das im Detail?

Ein NEON-Vektorregister kann in mehrere Lanes unterteilt werden, wobei jede Lane eine bestimmte Anzahl von Bits hat:

- **8-Bit Lanes**: Ein 128-Bit Q-Register hat 16 Lanes.
- **16-Bit Lanes**: Ein 128-Bit Q-Register hat 8 Lanes.
- **32-Bit Lanes**: Ein 128-Bit Q-Register hat 4 Lanes.
- **64-Bit Lanes**: Ein 128-Bit Q-Register hat 2 Lanes.

Jede Lane kann unabhängig von den anderen Lanes denselben oder unterschiedliche Datenwerte enthalten, und SIMD-Instruktionen (Single Instruction, Multiple Data) können auf jeder Lane parallel dieselbe Operation ausführen.

### Beispiel: 32-Bit Lanes in einem Q-Register

Angenommen, Sie haben ein 128-Bit Q-Register (`Q0`). Wenn Sie dieses Register in 32-Bit Lanes unterteilen, haben Sie 4 Lanes (`Lane 0`, `Lane 1`, `Lane 2`, und `Lane 3`). Eine SIMD-Operation, wie z.B. eine Addition, würde auf allen vier 32-Bit-Werten gleichzeitig ausgeführt werden. Das heißt, die Addition wird parallel in jeder Lane durchgeführt, was zu einem erheblichen Geschwindigkeitsgewinn führt, wenn viele ähnliche Operationen ausgeführt werden müssen.

NEON also provides integer arithmetic operations. How do the results from an
arithmetic operation such as addition or multiplication compare to one of the
logical operations? Logical operations are bit-by-bit, and the results stay in each
bit “column,” but arithmetic operations must expand to use more bits. Even an
example as 12+12=102 shows addition can have a carry that requires another bit
column. In order to provide multiple simultaneous parallel arithmetic operations,
the NEON coprocessor forces arithmetic operations to stay in fixed-sized “lanes”
and does not allow the results from one lane to carry (or overflow) into the next.
Although there can be 128 simultaneous 1-bit logical operations performed,
the number of simultaneous arithmetic operations is reduced by the lane size. In
a NEON arithmetic operation, the total number of bits involved is specified by
whether a D (64 bits) or a Q (128 bits) register is used. The lane size and type
are specified by the suffix.