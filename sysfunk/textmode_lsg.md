# B.3 Implementierung systemnaher Funktionen
## 3.1.5 Systemnahe Funktionen: Lösung: Implementierung von Funktionen zur Verwaltung des Textmodus

### Deklarationen und Definitionen

```assembly
.global textmode_get_tabentry
.global textmode_init
.extern canvas_base
.extern textmode_state

.equ tab_length, 80 * 60       
.equ row_length, 80     
.equ row_offset, 8

.text
```

In diesem Abschnitt werden globale und externe Symbole sowie Konstanten definiert, um den Textmodus zu verwalten. Die globalen Symbole umfassen `textmode_get_tabentry` zur Berechnung der Speicheradresse eines Zeichens in der Texttabelle und `textmode_init` zur Initialisierung des Textmodus. Externe Variablen sind `canvas_base`, die die Basisadresse der Leinwand speichert, und `textmode_state`, die den Status des Textmodus angibt. Konstanten wie `tab_length` (4800 Einträge), `row_length` (80 Zeichen pro Zeile) und `row_offset` (8 Bytes pro Zeile) dienen der Speicheradressierung im Textmodus.

### Funktion textmode_init
```assembly
textmode_init:
    push {lr}
    ldr  r0, =textmode_state
    ldr  r1, [r0]
    cmp  r1, #0
    bne  textmode_init_end
    mov  r1, #0xf
    str  r1, [r0]
    bl   canvas_init
    ldr  r1, =0x0
    bl   fillscreen
textmode_init_end:
    pop  {pc}
```

Die Funktion `textmode_init` initialisiert den Textmodus nur, wenn er noch nicht aktiv ist. Zunächst wird überprüft, ob der Modus bereits aktiviert ist, indem der Wert der Variable `textmode_state` abgefragt wird. Falls der Modus noch inaktiv ist, wird er aktiviert, indem `textmode_state` auf `0xF` gesetzt wird. Anschließend wird die Funktion `canvas_init` aufgerufen, um die Leinwand für die Textanzeige vorzubereiten. Danach wird der Bildschirm mithilfe der Funktion `fillscreen` gelöscht oder mit einer Standard-Hintergrundfarbe gefüllt. Wenn der Modus bereits aktiv ist, wird die Initialisierung übersprungen und die Funktion beendet.

#### Funktion textmode_get_tabentry

```assembly
textmode_get_tabentry:
    push {lr}
    push {r11}
    mov  r11, sp
    sub  sp, sp, #8
    str  r2, [r11, #-4]
    ldr  r2, =canvas_base
    ldr  r2, [r2]
    str  r2, [r11, #-8]
```

Die Funktion `textmode_get_tabentry` beginnt mit der Sicherung des Link-Registers und des Frame-Pointers `r11`, um später zur aufrufenden Funktion zurückkehren zu können und einen neuen Stack-Frame zu erstellen. Anschließend wird der Stack-Pointer um 8 Bytes verringert, um Platz für lokale Variablen zu reservieren. Die Hintergrundfarbe, die in `r2` übergeben wird, wird auf dem Stack gespeichert. Danach wird die Basisadresse der Leinwand aus der externen Variable `canvas_base` geladen und ebenfalls auf dem Stack abgelegt. Diese Schritte bereiten die weiteren Berechnungen der Zeichenposition im Textmodus vor.

#### Berechnung des Tab-Index

```assembly
calc_tabindex:
    mov  r3, #tab_length
    udiv r0, r1, r3
    mul  r2, r0, r3
    sub  r0, r1, r2
    cmp  r0, #0
    bne  calc_columnrowindex
    push {r0, r1}
    ldr  r1, [r11, #-4]
    bl   fillscreen
    pop  {r0, r1}
```

Die Funktion `calc_tabindex` berechnet den genauen Tabellenindex eines Zeichens auf dem Bildschirm und prüft, ob der Index die maximal darstellbare Anzahl an Zeichen überschreitet, um den Bildschirm bei Bedarf zurückzusetzen.

Zuerst wird die Gesamtlänge der Tabelle, in das Register `r3` geladen. Anschließend wird der Index in `r1` durch diese Länge geteilt, um herauszufinden, ob die Kapazität des Bildschirms überschritten wurde. Das Ergebnis der Division wird in `r0` gespeichert, um die Anzahl der vollständigen Bildschirmfüllungen zu ermitteln. Mit der Multiplikation des Ergebnisses der Division und der Tabellenlänge wird die Position innerhalb eines Bildschirmzyklus berechnet. Diese Berechnung (Modulo) zeigt den verbleibenden Index innerhalb des aktuellen Bildschirms an.

Wenn der Modulo-Wert 0 ist, bedeutet dies, dass der Index genau ein Vielfaches der maximalen Anzeigemöglichkeit ist, also alle verfügbaren Zeichen auf dem Bildschirm gefüllt wurden und die Darstellung von vorne beginnen soll. In diesem Fall werden die Register `r0` und `r1` gesichert, die Hintergrundfarbe wird aus dem Stack in `r1` geladen und die Funktion `fillscreen` aufgerufen, um den Bildschirm zu löschen. Danach werden die Register wiederhergestellt, und die Funktion setzt den Ablauf fort.

Wenn der Modulo-Wert ungleich 0 ist, bleibt die Position innerhalb der darstellbaren Grenzen des Bildschirms, und es wird zur Berechnung des Spalten- und Zeilenindexes gesprungen. 

#### Berechnung von Spalten- und Reihenindex

```assembly
calc_columnrowindex:
    mov  r3, #row_length
    udiv r1, r0, r3
    mul  r2, r1, r3
    sub  r0, r0, r2
```

Die Funktion `calc_columnrowindex` berechnet den genauen Spalten- und Reihenindex eines Zeichens, basierend auf dem zuvor berechneten Tabellenindex `r0`.

Zuerst wird die Anzahl der Zeichen pro Zeile, die in diesem Fall 80 beträgt, in das Register `r3` geladen. Dann wird der Tabellenindex `r0` durch diese Anzahl geteilt, um den Reihenindex zu ermitteln, der in `r1` gespeichert wird. Dieser Wert gibt an, in welcher Zeile das Zeichen sich befindet.

Anschließend wird der Reihenindex `r1` mit der Anzahl der Zeichen pro Zeile multipliziert, um die Gesamtanzahl der Zeichen in den vorangegangenen Zeilen zu berechnen. Dieser Wert wird von `r0` subtrahiert, um den Spaltenindex zu erhalten, also die genaue Position des Zeichens innerhalb der Zeile. Damit ist sowohl die Zeilenposition (Reihenindex) als auch die Position innerhalb der Zeile (Spaltenindex) ermittelt.


#### Berechnung der Elementadresse

```assembly
calculate_element_addr:
    mov r2, #row_offset
    mul r2, r2, r3
    mul r1, r1, r2
    add r0, r1, r0
    ldr r2, [r11, #-8]
    add r0, r2, r0, lsl #4
```

Die Funktion `calculate_element_addr` berechnet die Speicheradresse eines Zeichens im 8x8-Pixel-Raster nach folgender Formel:

**Elementadresse = Basisadresse + (Spaltenindex + (Reihengröße × Reihenindex)) × Elementgröße**

Der **`row_offset`** von 8 Bytes beschreibt den vertikalen Speicherabstand zwischen zwei **Tabellenreihen** in der **Tabelle**. Da jede Tabellenreihe 8 Pixel hoch ist, stellt dieser Offset sicher, dass der Speicher korrekt von einer Tabellenreihe zur nächsten springt.

Der **Reihenindex** wird mit dem **`row_offset`** und der **Reihengröße** in `r3` (Anzahl der Zeichen pro Zeile) multipliziert, um den Speicherabstand vom Tabellenanfang bis zur gewünschten Zeile zu berechnen, gemessen **in Elementen**.

Durch das Addieren des **Spaltenindex** zum **Reihenoffset** erhält man den **Gesamtoffset in Elementen**, der sowohl die vertikale als auch die horizontale Position des Zeichens beschreibt.

Da jedes Zeichen im 8x8-Pixel-Raster 16 Bytes belegt, wird dieser Gesamtoffset um 4 Bits (Multiplikation mit 16) verschoben und zur **Basisadresse** addiert, um die exakte **Speicheradresse** des Zeichens zu bestimmen.

#### Funktionsende

```assembly
    mov sp, r11
    pop {r11}
    pop {pc}
```

Am Ende der Funktion wird der Stack-Frame aufgelöst, der Stack-Pointer wiederhergestellt, und die Funktion beendet, indem zur aufrufenden Funktion zurückgesprungen wird.

|------------------------------|------------------------------------|---------------------------|
|   [zurück](textmode_ue.md)   |   [Hauptmenü](../ueberblick.md)    |   [weiter](text_ue.md)    |


|**3.1 Systemnahe Funktionen**                                                                  |
|-----------------------------------------------------------------------------------------------|
| [3.1.1 Implementierung systemnaher Funktionen](sysfunkintro.md)                               |
| [3.1.2 Implementierung von Speicherfunktionen in ARM-Assembly](memue.md)                      |
| [3.1.3 Implementierung von Zahlendarstellungsfunktionen](format_ue.md)                        |
| [3.1.4 Grundlegende Grafikbibliothek](canvas_ue.md)                                           |
| [3.1.5 Implementierung von Funktionen zur Verwaltung des Textmodus](textmode_ue.md)           |
| [3.1.6 Textdarstellung via Textmode](text_ue.md)                                              |
| [3.1.7 Implementierung einer `kwrite`-Funktion](kwrite_ue.md)                                 |
| [3.1.8 Implementierung einer Eingabefunktion](kread_ue.md)                                    |
| [3.1.9 Implementierung einer formatierenden Ausgabefunktion in ARM-Assembly](kprintf_ue.md)   |
| [3.1.10 Implementiere `kscan` für formatiertes Einlesen](kscan_ue.md)                         |