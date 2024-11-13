# B.1 Einführung
## 1.3.2 Assembler und Linker: Grundkonzepte von Linker-Scripten

Der Linker ist ein Programm das bei der Erstellung einer ausführbaren Datei eine zentrale Rolle einnimmt. Ein Linkerscript funkiert dabei als Anleitung für dieses Programm.

### Objektdateien und die Rolle des Linkers
Wenn Quellcode durch einen Compiler oder Assembler übersetzt wird, entsteht als Zwischenergebnis eine sogenannte Objektdatei. Diese enthält den maschinencode, der jedoch noch nicht ausführbar ist. Das liegt daran, dass der Code in verschiedene Abschnitte unterteilt ist und bestimmte Speicheradressen oder Symbole (z.B. Funktionen oder Variablen) noch unbestimmt bleiben. Diese unbestimmten Verweise werden als "unaufgelöste Symbole" bezeichnet, da ihre exakten Positionen erst beim finalen Zusammenfügen der Dateien festgelegt werden.

Hier kommt der Linker ins Spiel. Seine Hauptaufgabe besteht darin, mehrere Objektdateien zu einer ausführbaren Datei zu verbinden. Dabei löst der Linker die unaufgelösten Symbole auf, indem er die endgültigen Speicheradressen zuweist. Das Ergebnis dieses Prozesses ist eine Datei, in der alle Adressverweise korrekt gesetzt sind. Zudem legt der Linker eine Einstiegsadresse fest, die dem Betriebssystem oder Bootloader als Startpunkt dient, um das Programm auszuführen.

Alle Objektdateien sowie die finale Ausgabedatei bestehen unter anderem aus einer Liste von Sektionen. Jede dieser Sektionen hat einen eindeutigen Namen und eine bestimmte Größe und repräsentiert einen Bereich im Speicher, der für bestimmte Daten oder Programmcode vorgesehen ist. Zu den typischen Sektionen gehören etwa der Bereich für ausführbaren Code und der für globale Daten. Zudem besitzen Objektdateien eine Symboltabelle, die sowohl definierte als auch undefinierte Symbole, wie Funktionen oder Variablen, aufführt. Beim Assemblieren werden Symbole für alle definierten Elemente erzeugt, während Verweise auf externe Funktionen oder Variablen als undefinierte Symbole markiert bleiben. Diese werden erst beim Linken aufgelöst, wenn der genaue Speicherort bekannt ist.

Mit dem tool `readelf`lassen sich sowohl die Sektionen, als auch die Symboltabelle einer ausführbaren Datei anzeigen:

`readelf -S <dateiname>` zeigt die Sektionen der Datei an:
```
Section Headers:
  [Nr] Name              Type            Addr     Off    Size   ES Flg Lk Inf Al
  [ 0]                   NULL            00000000 000000 000000 00      0   0  0
  [ 1] .text             PROGBITS        00008000 008000 0019c4 00  AX  0   0 32
  [ 2] .data             PROGBITS        00010000 010000 010694 00  WA  0   0 65536
  [ 3] .ARM.attributes   ARM_ATTRIBUTES  00000000 020694 00002d 00      0   0  1
  [ 4] .heap             NOBITS          00000000 100000 100000 00   A  0   0 1048576
  [ 5] .debug_line       PROGBITS        00000000 1206c1 000cd2 00      0   0  1
  [ 6] .debug_info       PROGBITS        00000000 121393 000344 00      0   0  1
  [ 7] .debug_abbrev     PROGBITS        00000000 1216d7 0001b8 00      0   0  1
  [ 8] .debug_aranges    PROGBITS        00000000 121890 0002c0 00      0   0  8
  [ 9] .debug_str        PROGBITS        00000000 121b50 000292 01  MS  0   0  1
  [10] .symtab           SYMTAB          00000000 121de4 002970 10     11 610  4
  [11] .strtab           STRTAB          00000000 124754 001775 00      0   0  1
  [12] .shstrtab         STRTAB          00000000 125ec9 00007d 00      0   0  1

```

**[Nr]**: Die laufende Nummer der Sektion, beginnend bei 0.

**Name**: Der Name der Sektion (z.B. `.text`, `.data`).

**Type**: Der Sektionstyp, der die Art der Daten angibt (z.B. `PROGBITS` für ausführbaren Code oder `NOBITS` für uninitialisierte Daten).

**Addr**: Die Adresse im Speicher, an der die Sektion geladen wird.

**Off**: Der Offset der Sektion innerhalb der Datei, d.h. die Position der Sektion im Dateistream.

**Size**: Die Größe der Sektion in Bytes.

**ES**: Die Größe eines Eintrags innerhalb der Sektion (für Sektionen mit Tabelleneinträgen relevant).

**Flg**: Flags, die verschiedene Eigenschaften der Sektion angeben (z.B. `ALLOC`, `EXEC`).

**Lk**: Der Index einer verlinkten Sektion, falls zutreffend (z.B. Verweis auf die Symboltabelle).

**Inf**: Zusätzliche Information, die je nach Sektionstyp variiert (z.B. Anzahl von Einträgen in einer Tabelle).

**Al**: Die Ausrichtungsanforderung der Sektion im Speicher (z.B. 4-Byte-Ausrichtung).


`readelf -s <dateiname>` zeigt die Symboltabelle an:
```
Symbol table '.symtab' contains 663 entries:
   Num:    Value  Size Type    Bind   Vis      Ndx Name
     0: 00000000     0 NOTYPE  LOCAL  DEFAULT  UND
     1: 00008000     0 SECTION LOCAL  DEFAULT    1 .text
     2: 00010000     0 SECTION LOCAL  DEFAULT    2 .data
     3: 00000000     0 SECTION LOCAL  DEFAULT    3 .ARM.attributes
     4: 00000000     0 SECTION LOCAL  DEFAULT    4 .heap
     5: 00000000     0 SECTION LOCAL  DEFAULT    5 .debug_line
     6: 00000000     0 SECTION LOCAL  DEFAULT    6 .debug_info
    ...
    15: 00007000     0 NOTYPE  LOCAL  DEFAULT  ABS STACK_IRQ
    16: 00008000     0 NOTYPE  LOCAL  DEFAULT    1 $a
    17: 00008038     0 NOTYPE  LOCAL  DEFAULT    1 which_core
    18: 00008048     0 NOTYPE  LOCAL  DEFAULT    1 check_pl
    19: 000080fc     0 NOTYPE  LOCAL  DEFAULT    1 sleep
    20: 000080bc     0 NOTYPE  LOCAL  DEFAULT    1 kernel_entry
    21: 00008100     0 NOTYPE  LOCAL  DEFAULT    1 $d
    22: 00000000     0 FILE    LOCAL  DEFAULT  ABS vektor.o
    23: 00008120     0 NOTYPE  LOCAL  DEFAULT    1 $a
    ...
    31: 0000815c     0 NOTYPE  LOCAL  DEFAULT    1 fiq_handler
    32: 00008140     0 NOTYPE  LOCAL  DEFAULT    1 $d
    ...
```

**Num:** Symbolnummer in der Tabelle, eindeutiger Index.

**Value:** Adresse oder Wert des Symbols (z.B. Speicherposition).

**Size:** Größe des Symbols in Bytes.

**Type:** Symboltyp, z.B. Funktion (FUNC), Variable (OBJECT).

**Bind:** Bindung, z.B. GLOBAL (in anderen Dateien verfügbar), LOCAL (nur in dieser Datei).

**Vis:** Sichtbarkeit, z.B. DEFAULT (standard), HIDDEN (versteckt).

**Ndx:** gibt den Status oder Kontext des Symbols an

**Name:** Name des Symbols, z.B. Funktions- oder Variablenname.



### Linker-Skript: Aufbau und Schlüsselkonzepte

Ein **Linker-Skript** gibt dem Linker vor, wie Code und Daten eines Programms im Speicher angeordnet werden.

#### Anatomie eines Linker-Skripts

Ein Linker-Skript besteht aus mehreren Hauptbestandteilen:

**Speicherlayout**: Definiert, welcher Speicherbereich wo verfügbar ist.

**Sektionen**: Bestimmt, welche Teile des Programms in welchen Speicherbereich geladen werden sollen.

**Optionen**: Befehle zur Spezifikation der Architektur, des Einstiegspunkts und anderer Parameter, falls nötig.



Diese Struktur sorgt dafür, dass der Linker das Programm korrekt im Speicher anordnet und bestimmte Symbole oder Optionen an festgelegten Stellen platziert werden können.

#### Speicherlayout
Um Speicherplatz für ein Programm zu reservieren, muss der Linker wissen, wie viel Speicher verfügbar ist und an welchen Adressen er sich befindet. Dafür dient die **MEMORY**-Definition im Linkerskript.

Die Syntax für MEMORY sieht folgendermaßen aus:

```
MEMORY
  {
    name [(attr)] : ORIGIN = origin, LENGTH = len
    …
  }
```

Dabei bedeuten:

- **name**: Der Name für diesen Speicherbereich, z.B. "flash" oder "ram". Der Name ist frei wählbar und dient nur zur Identifikation.
- **(attr)**: Optionale Attribute wie Schreibbarkeit (w), Lesbarkeit (r) oder Ausführbarkeit (x). Flash-Speicher ist oft (rx), RAM typischerweise (rwx). Diese Attribute beschreiben die Eigenschaften des Speichers, setzen sie aber nicht fest.
- **origin**: Die Startadresse des Speicherbereichs.
- **len**: Die Größe des Speicherbereichs in Bytes.

#### SECTIONS: Definition von Programm-Speicherbereichen

Der Befehl **SECTIONS** ist das Herzstück eines Linker-Skripts. Er gibt vor, welche Teile des Programms (die sogenannten **Sektionen**) in welche Speicherbereiche geladen werden sollen. Sektionen im Speicher definieren zusammenhängende Bereiche, in denen Code und Daten organisiert werden. Symbole werden in denselben Abschnitt gelegt, wenn sie zusammen initialisiert werden oder sich im gleichen Speicherbereich befinden sollen. 

Im Linker-Skript können diese Sektionen wie folgt zugeordnet werden:

```ld
SECTIONS {
  .text : { *(.text) }
  .data : { *(.data) }
  .bss  : { *(.bss) }
}
```

In diesem Beispiel werden alle `.text`-Sektionen des Programms zusammengeführt und an der gleichen Speicheradresse platziert. Das Gleiche gilt für die `.data`- und `.bss`-Sektionen. Das `*`-Symbol fungiert als Platzhalter und sorgt dafür, dass alle entsprechenden Sektionen aus den Eingabedateien (z.B. Objektdateien) in den definierten Bereich der Ausgabedatei übernommen werden.

#### Adresszuweisung und Speicherlayout

Neben der Zuweisung der Sektionen können im Linker-Skript auch explizit Speicheradressen festgelegt werden. Dadurch wird definiert, wo die Sektionen physisch im Speicher abgelegt werden. Ein Beispiel dafür:

```ld
SECTIONS {
  . = 0x10000;
  .text : { *(.text) }
  . = 0x8000000;
  .data : { *(.data) }
  .bss : { *(.bss) }
}
```

Hier wird die `.text`-Sektion ab der Adresse `0x10000` und die `.data`-Sektion ab der Adresse `0x8000000` im Speicher platziert. Durch die Zuweisung von Adressen können Entwickler sicherstellen, dass der Code und die Daten an den richtigen Speicherstellen abgelegt werden, insbesondere bei komplexen Systemen mit unterschiedlichen Speicherbereichen.

#### ENTRY: Festlegung des Einstiegspunkts

Der **Einstiegspunkt** eines Programms ist die Adresse, an der die Ausführung beginnt, typischerweise die erste Funktion, die nach dem Start des Programms aufgerufen wird. Der Einstiegspunkt wird im Linker-Skript mit dem **ENTRY**-Befehl definiert:

```ld
ENTRY(main)
```

In diesem Beispiel wird die Ausführung des Programms bei der Funktion `main` begonnen. Der Einstiegspunkt ist besonders wichtig für die Initialisierung des Systems und den Start des Programmablaufs.

#### Eingabe- und Ausgabesektionen

In einem Linker-Skript spielen **Eingabe- und Ausgabesektionen** eine zentrale Rolle, wenn es darum geht, wie Code und Daten in der finalen ausführbaren Datei organisiert werden. **Eingabesektionen** sind Bereiche in Objektdateien, die spezifische Inhalte wie ausführbaren Code (z.B. in `.text`) oder Daten (z.B. in `.data`) enthalten. Diese Sektionen werden vom Linker verarbeitet und in **Ausgabesektionen** überführt, welche die Struktur der fertigen Datei bestimmen.

Eine Ausgabesektion, wie etwa `.text`, definiert, wo im Speicher die entsprechenden **Eingabesektionen** aus den Objektdateien abgelegt werden. Der Ausdruck `* (.text)` im Linker-Skript weist den Linker an, alle `.text`-Sektionen der Eingabedateien in die Ausgabesektion `.text` zu packen. Das Sternchen (`*`) fungiert dabei als Platzhalter, der sicherstellt, dass alle passenden Eingabesektionen zusammengeführt werden.

Die Ausrichtung der **Ausgabesektionen** wird von der größten Ausrichtungsanforderung der enthaltenen **Eingabesektionen** bestimmt. Dies ist wichtig, um sicherzustellen, dass alle Daten korrekt im Speicher platziert und zugänglich sind. Beispielsweise könnte eine `.text`-Ausgabesektion so organisiert werden, dass sie den ausführbaren Code bündelt, während `.data` den Bereich für initialisierte Daten repräsentiert und `.bss` für nicht initialisierte Daten steht.

#### Der Lagezähler und seine Rolle im Linker-Skript
Im Linker-Skript ist der Lagezähler `.` eine zentrale Variable, die die aktuelle Speicheradresse repräsentiert und dabei hilft, Eingabesektionen korrekt in Ausgabesektionen zu platzieren. Zu Beginn hat dieser Zähler den Wert 0, wird jedoch bei jeder neuen Sektion um deren Größe erhöht. Der Lagezähler bestimmt also, wo die nächste Sektion im Speicher platziert wird, sofern keine expliziten Adressen festgelegt wurden. 

Der Lagezähler kann jedoch nicht nur die Platzierung von Sektionen steuern, sondern auch dafür sorgen, dass der Code auf bestimmte Speichergrenzen ausgerichtet ist. Besonders bei Architekturen wie ARM ist es wichtig, dass der Code auf 2- oder 4-Byte-Grenzen ausgerichtet ist. Hierfür bietet das Linker-Skript die Funktion `ALIGN`, mit der der Lagezähler angepasst werden kann, um das nötige **Padding** einzufügen und so die Speichergrenzen korrekt zu setzen.

Ein Beispiel für die Ausrichtung der `.text`-Sektion auf 4 Bytes sieht wie folgt aus:

```c
SECTIONS
{
  .isr_vector : {
    exceptions.o (.isr_vector) /* 1023 Bytes */
  }

  .text : {
    . = ALIGN(4);
    *(.text)
    *(.text*)
  }
}
```

Alternativ können die Speicheradressen auch explizit im Skript festgelegt werden. In diesem Fall würde man beispielsweise die Sektion `.isr_vector` auf Adresse `0x08000000` und die `.text`-Sektion auf `0x08000400` setzen:

```c
SECTIONS
{
  . = 0x08000000;
  .isr_vector : {
    exceptions.o (.isr_vector)
  }

  . = 0x08000400;
  .text : {
    . = ALIGN(4);
    *(.text)
    *(.text*)
  }
}
```

#### Ausgabesektionen mit Speicherregionen verwalten

Eine weitere Methode zur Platzierung von Ausgabesektionen ist die Nutzung von **Speicherregionen**. Statt jeder Sektion direkt eine Adresse zuzuweisen, definiert man Speicherregionen, die den verfügbaren Speicher des Systems beschreiben:

```c
MEMORY
{
  FLASH (rx)      : ORIGIN = 0x08000000, LENGTH = 1024K
  SRAM (xrw)      : ORIGIN = 0x20000000, LENGTH = 256K
  SDRAM (xrw)     : ORIGIN = 0x90000000, LENGTH = 8M
}
```

Jede Region hat Berechtigungen wie **r** (lesen), **w** (schreiben) oder **x** (ausführen). Diese Attribute werden mit denen der Sektionen abgeglichen. So würden `.text` und `.rodata` in dem **FLASH**-Speicher landen, da sie nicht schreibbar sind. 

### LMA und VMA: Lade- und virtuelle Adressen in Linker-Skripten

In der Speicherverwaltung von Programmen spielen zwei Schlüsselbegriffe eine wichtige Rolle: die **Load Memory Address (LMA)** und die **Virtual Memory Address (VMA)**. Diese Begriffe helfen zu definieren, wie und wo im Speicher bestimmte Sektionen des Programms abgelegt und ausgeführt werden.

- **LMA** ist die **Ladeadresse**: Dies ist die physische Speicheradresse, an die der Code tatsächlich geladen wird. Sie gibt an, wo der Code oder die Daten im Speicher abgelegt werden, zum Beispiel beim Laden von Flash-Speicher in den RAM.
- **VMA** ist die **virtuelle Speicheradresse**, die zur Laufzeit verwendet wird, um auf den Speicher zuzugreifen. Sie gibt an, wo der Code ausgeführt wird oder Daten im Arbeitsspeicher referenziert werden. Diese Adresse wird vom Programm und dem Prozessor während der Ausführung genutzt.

In den meisten einfachen Fällen sind LMA und VMA identisch – das heißt, der Code wird an der gleichen Adresse geladen und ausgeführt. In komplexeren Szenarien können LMA und VMA jedoch unterschiedlich sein. Ein häufiges Beispiel dafür ist, wenn ein Programm von einem nicht flüchtigen Speicher (wie Flash) in den Arbeitsspeicher (RAM) geladen wird, aber an einer anderen virtuellen Adresse ausgeführt werden muss.

**Beispiel: Unterschiedliche LMA und VMA**

Mit einem Linker-Skript kann man festlegen, ob LMA und VMA gleich oder unterschiedlich sein sollen. Dies geschieht durch das `AT`-Schlüsselwort, das die Ladeadresse (LMA) explizit festlegt. 

Ein Beispiel für ein Linker-Skript, das LMA und VMA definiert:

```ld
SECTIONS {
  .text 0x08000000 : AT(0x08000000) { *(.text) }
  .data 0x20000000 : AT(0x08001000) { *(.data) }
}
```

In diesem Beispiel wird der `.text`-Abschnitt sowohl bei der LMA `0x08000000` als auch bei der VMA `0x08000000` platziert, d.h., er wird geladen und zur Laufzeit an derselben Adresse ausgeführt. Der `.data`-Abschnitt wird jedoch bei der LMA `0x08001000` geladen, während er zur Laufzeit bei der VMA `0x20000000` ausgeführt wird.

[Weitere Informationen zu Linkern, inklusive detailierter Beschreibung aller Linkerskript-Befehle]( https://sourceware.org/binutils/docs/ld/index.html)

|---------------------------------|------------------------------------|----------------------------------------|
|   [zurück](wasistasmlinker.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](../debuggdb/debuggdb.md)    |


|**1.3 Assembler und Linker**                               |
|-----------------------------------------------------------|
| [1.3.1 Intro](wasistasmlinker.md)                         |
| [1.3.2 Grundkonzepte von Linker-Scripten](linkerscr.md)   |