
## Bedingungscodes 
In der ARM-Architektur können Bedingungscodes verwendet werden, um Anweisungen basierend auf dem Zustand der Flags im CPSR (Current Program Status Register) auszuführen. Diese Bedingungscodes unterscheiden sich, je nachdem, ob die Vergleiche mit vorzeichenbehafteten (signed) oder vorzeichenlosen (unsigned) Werten durchgeführt werden. Hier ist eine Übersicht der wichtigsten Bedingungscodes:

### Unsigned Bedingungen (Vorzeichenlos):
- **HS (Higher or Same)**: Wird gesetzt, wenn ein Übertrag aufgetreten ist (C == 1). Das bedeutet, dass der Vergleich größer oder gleich ist bei vorzeichenlosen Werten.
- **LO (Lower)**: Kein Übertrag (C == 0), bedeutet, dass der Vergleich kleiner ist bei vorzeichenlosen Werten.
- **HI (Higher)**: Höher bei vorzeichenlosen Werten (C == 1 und Z == 0).
- **LS (Lower or Same)**: Niedriger oder gleich bei vorzeichenlosen Werten (C == 0 oder Z == 1).

### Signed Bedingungen (Vorzeichenbehaftet):
- **GT (Greater Than)**: Größer bei vorzeichenbehafteten Werten (Z == 0 und N == V).
- **LT (Less Than)**: Kleiner bei vorzeichenbehafteten Werten (N != V).
- **GE (Greater or Equal)**: Größer oder gleich bei vorzeichenbehafteten Werten (N == V).
- **LE (Less or Equal)**: Kleiner oder gleich bei vorzeichenbehafteten Werten (Z == 1 oder N != V).

### Allgemeine Bedingungen:
- **EQ (Equal)**: Gleich (Z == 1).
- **NE (Not Equal)**: Ungleich (Z == 0).
- **MI (Minus)**: Negativ (N == 1).
- **PL (Plus)**: Positiv oder Null (N == 0).
- **VS (Overflow Set)**: Überlauf (V == 1).
- **VC (Overflow Clear)**: Kein Überlauf (V == 0).
- **AL (Always)**: Immer ausführen (keine Bedingung).

Diese Bedingungscodes sind essenziell, um bedingte Instruktionen in ARM-Assembler umzusetzen.

[weiter](bedinstr.md)