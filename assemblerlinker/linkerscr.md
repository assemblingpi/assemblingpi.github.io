# Grundkonzepte von Linker-Skripten

Die verschiedenen Eingabedateien, die wir mithilfe des Linkers zu einer Ausgabedatei umwandeln möchten nennt man auch Objektdatei. 
Alle Objektdatein und die Ausgabedatei enthalten unter anderem eine Liste von Sektionen. Jede Sektion hat einen Namen und eine Größe. Diese Sektionen sind Bereiche im Speicher, die für verschiedene Daten oder Code vorgesehen sind.

Jede Objektdatei enthält eine Symboltabelle mit definierten und undefinierten Symbolen, die Namen und Adressen haben. Beim Assemblieren  eines Programms werden Symbole für alle definierten Funktionen und Variablen erstellt. Undefinierte Funktionen oder Variablen werden als undefinierte Symbole aufgeführt.


## Linker-Skript-Format

Ein Linker-Skript ist eine Textdatei und besteht aus einer Reihe von Befehlen. Jeder Befehl ist entweder ein Schlüsselwort, möglicherweise gefolgt von Argumenten, oder eine Zuweisung an ein Symbol. Befehle können durch Semikolons getrennt werden. Leerzeichen werden meist ignoriert.

Beispiel für ein solches Linker-Skript:
```ld
SECTIONS{
  . = 0x10000;
  .text : { *(.text) }
  . = 0x8000000;
  .data : { *(.data) }
  .bss : { *(.bss) }
}
```
Der Befehl SECTIONS wird mit dem Schlüsselwort SECTIONS eingeleitet, gefolgt von einer Reihe von Symbolzuweisungen und Beschreibungen der Ausgabesektionen in geschweiften Klammern. Die erste Zeile setzt den Wert des speziellen Symbols . (Punkt), welches der Lokationszähler ist. Wenn die Adresse einer Ausgabesektion nicht anders angegeben ist, wird diese Adresse vom aktuellen Wert des Lokationszählers gesetzt. Der Lokationszähler wird dann um die Größe der jeweiligen Ausgabesektion erhöht. Zu Beginn des SECTIONS-Befehls hat der Lokationszähler den Wert 0.. 
Da `. = 0x8000000`; die Adresse für .data festlegt wird der Lokationszähler um die Größe von `.data`erhöht.

Die Zeilen .text, .data und .bss definieren Ausgabesektionen für Code, initialisierte Daten und uninitialisierte Daten. Innerhalb der geschweiften Klammern werden mit *(.text), *(.data) und *(.bss) die jeweiligen Eingabesektionen aufgelistet, die in die entsprechende Ausgabesektion platziert werden sollen. Das * ist ein Platzhalter, der alle Eingabesektionen mit dem entsprechenden Namen einschließt.

Der Linker verwendet die angegebenen Adressen (0x10000 für .text und 0x8000000 für .data) als Startadressen für die Platzierung der jeweiligen Ausgabesektionen in der Ausgabedatei.

Der Linker stellt zudem sicher, dass jede Ausgabesektion die erforderliche Ausrichtung hat, indem er den Lokationszähler gegebenenfalls erhöht. 

Setzen des Einstiegspunkts
Die erste Anweisung, die in einem Programm ausgeführt wird, wird als Einstiegspunkt (Entry) bezeichnet. Man kann das ENTRY-Kommando im Linker-Skript verwenden, um den Einstiegspunkt festzulegen. Das Argument ist der Name eines Symbols (sprich eines globalen labels):
```
ENTRY(symbol)
```


Output-Section LMA (Load Memory Address)
Jeder Abschnitt hat eine virtuelle Adresse (VMA), das heißt die Adresse an der Code und Daten sich bei Ausführung befinden und eine Ladeadresse (LMA). Die Adressausdruck im Output-Abschnitt legt die VMA fest. Der Linker setzt normalerweise die LMA gleich der VMA. Dies kann mit dem AT-Schlüsselwort geändert werden. Der Ausdruck, der auf das AT-Schlüsselwort folgt, spezifiziert die Ladeadresse des Abschnitts. Diese Funktion ist nützlich, um ein ROM-Image zu erstellen.

Beispiel:
```ld
SECTIONS {
  .text 0x1000 : { *(.text) _etext = . ; }
  .mdata 0x2000 :
    AT ( ADDR (.text) + SIZEOF (.text) )
    { _data = . ; *(.data); _edata = . ; }
  .bss 0x3000 :
    { _bstart = . ; *(.bss) *(COMMON) ; _bend = . ; }
}
```
Laufzeit-Initialisierungscode
Zur Laufzeit muss der initialisierte Datenteil vom ROM-Image an seine Ausführungsadresse kopiert werden. Der uninitialisierte Datenteil (BSS) muss auf Null gesetzt werden.

[Weitere Informationen zu Linkern, inklusive detailierter Beschreibung aller Linkerskript-Befehle]( https://sourceware.org/binutils/docs/ld/index.html)

