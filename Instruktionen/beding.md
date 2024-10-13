# 2. Basic Blocks implementieren
## 2.3.7 Datenverarbeitung: Bedingungscodes 

In ARM-Assembler können Bedingungscodes verwendet werden, um Anweisungen basierend auf dem Zustand der Flags im CPSR (Current Program Status Register) auszuführen. Diese Bedingungscodes stellen Kombinationen der Flags dar und unterscheiden sich in zwei Gruppen, jene, die  vorzeichenbehaftete Bedingungen betreffen und diejenigen, die vorzeichenlose Bedingungen betreffen. Hier ist eine Übersicht der wichtigsten Bedingungscodes:

### Unsigned Bedingungen (Vorzeichenlos):
- **HS (Higher or Same)**: Wird gesetzt, wenn ein Übertrag aufgetreten ist (C == 1). Das bedeutet, dass der Vergleich größer oder gleich ist bei vorzeichenlosen Werten
- **LO (Lower)**: Kein Übertrag (C == 0), bedeutet, dass der Vergleich kleiner ist bei vorzeichenlosen Werten
- **HI (Higher)**: Höher bei vorzeichenlosen Werten (C == 1 und Z == 0)
- **LS (Lower or Same)**: Niedriger oder gleich bei vorzeichenlosen Werten (C == 0 oder Z == 1)

### Signed Bedingungen (Vorzeichenbehaftet):
- **MI (Minus)**: Negativ (N == 1)
- **PL (Plus)**: Positiv oder Null (N == 0)
- **GT (Greater Than)**: Größer bei vorzeichenbehafteten Werten (Z == 0 und N == V)
- **LT (Less Than)**: Kleiner bei vorzeichenbehafteten Werten (N != V)
- **GE (Greater or Equal)**: Größer oder gleich bei vorzeichenbehafteten Werten (N == V)
- **LE (Less or Equal)**: Kleiner oder gleich bei vorzeichenbehafteten Werten (Z == 1 oder N != V)
- **VS (Overflow Set)**: Überlauf (V == 1)
- **VC (Overflow Clear)**: Kein Überlauf (V == 0)


### Allgemeine Bedingungen:
- **EQ (Equal)**: Gleich (Z == 1)
- **NE (Not Equal)**: Ungleich (Z == 0)
- **AL (Always)**: Immer ausführen (keine Bedingung)

Diese Bedingungscodes sind essenziell, um bedingte Instruktionen in ARM-Assembler umzusetzen.

#### Die folgende Tabelle zeigt den Zusammenhang von Bedingungen und Flags:

| Abkürzung   |  Bedeutung              |   Flags             |
|-------------|-------------------------|---------------------|
| EQ          | Equal                   |   Z=1               |
| NE          | Not equal               |   Z=0               |
| CS/HS       | Unsigned higher or same |   C=1               |
| CC/LO       | Unsigned lower          |   C=0               |
| MI          | Minus                   |   N=1               |
| PL          | Positive or Zero        |   N=0               |
| VS          | Overflow                |   V=1               |
| VC          | No overflow             |   V=0               |
| HI          | Unsigned higher         |   C=1 & Z=0         |
| LS          | Unsigned lower or same  |   C=0 or Z=1        |
| GE          | Greater or equal        |   N=V               |
| LT          | Less than               |   N!=V              |
| GT          | Greater than            |   Z=0 & N=V         |
| LE          | Less than or equal      |   Z=1 or N!=V       |
| AL          | Always                  |        -            |

|----------------------|------------------------------------|----------------------------|
|   [zurück](comp.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](bedinstr.md)    |
