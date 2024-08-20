## Debugging mit GDB: Ein Leitfaden

### Einleitung

Debugging ist ein essenzieller Skill für jeden Programmierer. Es verbessert die Fähigkeit, Probleme systematisch zu identifizieren und zu lösen, was zu robusteren und zuverlässigeren Programmen führt. In diesem Tutorial nutzen wir den GDB-Debugger, ein weit verbreitetes und mächtiges Tool, das Entwicklern hilft, Programme zu testen und Fehler zu finden.

### Warum GDB?

Der GNU Debugger (GDB) ist ein vielseitiges Werkzeug, das in vielen Entwicklungsumgebungen eingesetzt wird. Es unterstützt eine Vielzahl von Programmiersprachen und Plattformen, was es universell einsetzbar macht. Als Open-Source-Software ist GDB kostenlos und gut dokumentiert, was das Lernen und Anwenden erleichtert.

#### Vorteile von GDB:
- **Weit verbreitet:** GDB ist in vielen Entwicklungsumgebungen integriert.
- **Unterstützt viele Sprachen und Plattformen:** Es bietet breite Unterstützung, einschließlich Remote-Debugging mit QEMU.
- **Open-Source:** Kostenlos und gut dokumentiert.
- **Tiefer Einblick:** Entwicklern ermöglicht GDB, die innere Logik von Programmen zu verstehen und Fehlerursachen zu identifizieren.

### Features von GDB

GDB bietet zahlreiche nützliche Funktionen, darunter:
- **Schrittweises Ausführen:** Programme können Zeile für Zeile oder Anweisung für Anweisung ausgeführt werden.
- **Breakpoints und Watchpoints:** Programme können an bestimmten Stellen angehalten werden, um den aktuellen Zustand zu untersuchen.
- **Speicher- und Registeranalyse:** Anzeigen des Inhalts von Speichern und Registern zur Fehlersuche.

### GDB Cheatsheet: Die wichtigsten Befehle auf einen Blick

#### Breakpoints und Watchpoints setzen und verwalten
- **b main** - Setzt einen Breakpoint am Anfang des Programms.
- **b** - Setzt einen Breakpoint in der aktuellen Zeile.
- **b N** - Setzt einen Breakpoint in der Zeile N.
- **b +N** - Setzt einen Breakpoint N Zeilen nach der aktuellen Zeile.
- **b fn** - Setzt einen Breakpoint am Anfang der Funktion "fn".
- **d N** - Löscht den Breakpoint Nummer N.
- **info break** - Listet alle gesetzten Breakpoints auf.
- **watch $rx == 0xvalue** - Setzt einen Watchpoint, der das Programm anhält, sobald das Register x den definierten Wert enthält.

#### Programmablauf steuern
- **r** - Startet das Programm und läuft bis zum nächsten Breakpoint oder Fehler.
- **c** - Setzt die Ausführung des Programms bis zum nächsten Breakpoint oder Fehler fort.
- **f** - Läuft bis zur Beendigung der aktuellen Funktion.
- **s** - Führt die nächste Zeile des Programms aus.
- **s N** - Führt die nächsten N Zeilen des Programms aus.
- **n** - Führt die nächste Zeile des Programms aus, tritt aber nicht in Funktionen ein.
- **u N** - Läuft bis zu N Zeilen vor der aktuellen Zeile.

#### Speicher und Variablen untersuchen
- **p var** - Gibt den aktuellen Wert der Variablen "var" aus.
- **bt** - Gibt einen Stack-Trace aus.
- **u** - Geht eine Ebene im Stack nach oben.
- **d** - Geht eine Ebene im Stack nach unten.
- **info vector** - Gibt Informationen über Neon-Register aus.

#### Speicherauszüge anzeigen
- **x/nfu addr** - Zeigt den Speicher an der angegebenen Adresse an, mit optionalen Parametern für Anzahl, Format und Einheit.
  - **n** - Anzahl der anzuzeigenden Einheiten (Standard ist 1).
  - **f** - Anzeigeformat: `x` (Hexadezimal), `d` (Dezimal), `u` (Unsigned), `o` (Oktal), `t` (Binär), `a` (Adresse), `c` (Zeichen), `s` (Nullterminierter String), oder `i` (Maschinenbefehl).
  - **u** - Einheitengröße: `b` (Bytes), `h` (Halbwörter, 2 Bytes), `w` (Wörter, 4 Bytes), `g` (Riesenwörter, 8 Bytes).

Beispiele:
- **x/3uh 0x54320** - Zeigt drei Halbwörter (`h`) im Speicher an, formatiert als unsigned Dezimal (`u`), beginnend bei Adresse 0x54320.
- **x/4xw $sp** - Zeigt vier Wörter (`w`) im Speicher oberhalb des Stack-Pointers (`$sp`) in Hexadezimal (`x`) an.

#### GDB beenden
- **q** - Beendet GDB.
