# A.4 Datentypen 
## 4.2.2 Einfache Datentypen: ASCII

ASCII steht für "American Standard Code for Information Interchange". Es handelt sich um einen Zeichensatz, der in den 1960er Jahren entwickelt wurde, um eine einheitliche Methode zur Darstellung von Textzeichen und Steuerzeichen in Computern und Kommunikationssystemen bereitzustellen.

### Was ist ASCII?
ASCII definiert eine Zuordnung zwischen Zahlen (Dezimalwerten von 0 bis 127) und Zeichen, einschließlich Buchstaben, Ziffern, Interpunktionszeichen und Steuerzeichen wie Zeilenumbruch oder Tabulator. Jeder dieser Zeichen wird durch einen spezifischen 7-Bit-Binärwert repräsentiert, was es Computern ermöglicht, diese Zeichen intern zu speichern und zu verarbeiten.

### Wofür braucht man ASCII?
ASCII bildet die Grundlage für die Darstellung von einfachem Text in Computern, Dateiformaten und Netzwerkprotokollen, was sicherstellt, dass Text von verschiedenen Systemen korrekt erkannt und interpretiert wird. Es wurde entwickelt, um die Kommunikation zwischen verschiedenen Geräten und Systemen zu standardisieren, insbesondere in frühen Netzwerken und beim Datenaustausch zwischen Computern. In der Programmierung wird ASCII häufig genutzt, um Zeichen zu manipulieren, zu sortieren oder zu vergleichen, da die Zeichen durch feste numerische Werte eindeutig identifizierbar sind.

### Warum ist ASCII wichtig?
ASCII war eine entscheidende Innovation, weil es eine einfache und standardisierte Methode bot, um Text in einer Form darzustellen, die von jedem Computer verstanden werden kann. Dies war besonders wichtig in einer Zeit, in der verschiedene Computersysteme oft inkompatibel miteinander waren. Auch heute bildet ASCII die Grundlage für viele moderne Zeichensätze, obwohl es durch umfangreichere Systeme wie Unicode ergänzt wurde, die eine breitere Palette von Zeichen aus verschiedenen Sprachen und Symbolen unterstützen.


### ASCII - Tabelle

Die folgende Tabelle dient als Referenz, um schnell nachzusehen, welches Ascii-Zeichen welchem Wert entspricht:

| Zeichen | Dezimal | Hexadezimal |
|---------|---------|-------------|
| NUL     | 0       | 0x00        |
| SOH     | 1       | 0x01        |
| STX     | 2       | 0x02        |
| ETX     | 3       | 0x03        |
| EOT     | 4       | 0x04        |
| ENQ     | 5       | 0x05        |
| ACK     | 6       | 0x06        |
| BEL     | 7       | 0x07        |
| BS      | 8       | 0x08        |
| TAB     | 9       | 0x09        |
| LF      | 10      | 0x0A        |
| VT      | 11      | 0x0B        |
| FF      | 12      | 0x0C        |
| CR      | 13      | 0x0D        |
| SO      | 14      | 0x0E        |
| SI      | 15      | 0x0F        |
| DLE     | 16      | 0x10        |
| DC1     | 17      | 0x11        |
| DC2     | 18      | 0x12        |
| DC3     | 19      | 0x13        |
| DC4     | 20      | 0x14        |
| NAK     | 21      | 0x15        |
| SYN     | 22      | 0x16        |
| ETB     | 23      | 0x17        |
| CAN     | 24      | 0x18        |
| EM      | 25      | 0x19        |
| SUB     | 26      | 0x1A        |
| ESC     | 27      | 0x1B        |
| FS      | 28      | 0x1C        |
| GS      | 29      | 0x1D        |
| RS      | 30      | 0x1E        |
| US      | 31      | 0x1F        |
| SPACE   | 32      | 0x20        |
| !       | 33      | 0x21        |
| "       | 34      | 0x22        |
| #       | 35      | 0x23        |
| $       | 36      | 0x24        |
| %       | 37      | 0x25        |
| &       | 38      | 0x26        |
| '       | 39      | 0x27        |
| (       | 40      | 0x28        |
| )       | 41      | 0x29        |
| *       | 42      | 0x2A        |
| +       | 43      | 0x2B        |
| ,       | 44      | 0x2C        |
| -       | 45      | 0x2D        |
| .       | 46      | 0x2E        |
| /       | 47      | 0x2F        |
| 0       | 48      | 0x30        |
| 1       | 49      | 0x31        |
| 2       | 50      | 0x32        |
| 3       | 51      | 0x33        |
| 4       | 52      | 0x34        |
| 5       | 53      | 0x35        |
| 6       | 54      | 0x36        |
| 7       | 55      | 0x37        |
| 8       | 56      | 0x38        |
| 9       | 57      | 0x39        |
| :       | 58      | 0x3A        |
| ;       | 59      | 0x3B        |
| <       | 60      | 0x3C        |
| =       | 61      | 0x3D        |
| >       | 62      | 0x3E        |
| ?       | 63      | 0x3F        |
| @       | 64      | 0x40        |
| A       | 65      | 0x41        |
| B       | 66      | 0x42        |
| C       | 67      | 0x43        |
| D       | 68      | 0x44        |
| E       | 69      | 0x45        |
| F       | 70      | 0x46        |
| G       | 71      | 0x47        |
| H       | 72      | 0x48        |
| I       | 73      | 0x49        |
| J       | 74      | 0x4A        |
| K       | 75      | 0x4B        |
| L       | 76      | 0x4C        |
| M       | 77      | 0x4D        |
| N       | 78      | 0x4E        |
| O       | 79      | 0x4F        |
| P       | 80      | 0x50        |
| Q       | 81      | 0x51        |
| R       | 82      | 0x52        |
| S       | 83      | 0x53        |
| T       | 84      | 0x54        |
| U       | 85      | 0x55        |
| V       | 86      | 0x56        |
| W       | 87      | 0x57        |
| X       | 88      | 0x58        |
| Y       | 89      | 0x59        |
| Z       | 90      | 0x5A        |
| [       | 91      | 0x5B        |
| \       | 92      | 0x5C        |
| ]       | 93      | 0x5D        |
| ^       | 94      | 0x5E        |
| _       | 95      | 0x5F        |
| `       | 96      | 0x60        |
| a       | 97      | 0x61        |
| b       | 98      | 0x62        |
| c       | 99      | 0x63        |
| d       | 100     | 0x64        |
| e       | 101     | 0x65        |
| f       | 102     | 0x66        |
| g       | 103     | 0x67        |
| h       | 104     | 0x68        |
| i       | 105     | 0x69        |
| j       | 106     | 0x6A        |
| k       | 107     | 0x6B        |
| l       | 108     | 0x6C        |
| m       | 109     | 0x6D        |
| n       | 110     | 0x6E        |
| o       | 111     | 0x6F        |
| p       | 112     | 0x70        |
| q       | 113     | 0x71        |
| r       | 114     | 0x72        |
| s       | 115     | 0x73        |
| t       | 116     | 0x74        |
| u       | 117     | 0x75        |
| v       | 118     | 0x76        |
| w       | 119     | 0x77        |
| x       | 120     | 0x78        |
| y       | 121     | 0x79        |
| z       | 122     | 0x7A        |
| {       | 123     | 0x7B        |
| \|      | 124     | 0x7C        |
| }       | 125     | 0x7D        |
| ~       | 126     | 0x7E        |
| DEL     | 127     | 0x7F        |

|-------------------------------|------------------------------------|----------------------------------|
|   [zurück](einfachdtypen.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](komplexedtypen.md)    |


| **4.2 Einfache Datentypen**                                           |
|-----------------------------------------------------------------------|
| [4.2.1 Intro](einfachdtypen.md)                                       |
| [4.2.2 ASCII](ascii.md)                                               |