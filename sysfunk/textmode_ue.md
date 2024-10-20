# B.3 Implementierung systemnaher Funktionen
## 3.1.5 Systemnahe Funktionen: Aufgabe zur Implementierung von Funktionen zur Verwaltung des Textmodus

### Hintergrund

Der Textmodus stellt eine Betriebsart dar, in der Zeichen in einem rasterbasierten Format auf dem Bildschirm ausgegeben werden. Dabei wird der Bildschirm in Zeilen und Spalten unterteilt, wobei jedes Feld ein einzelnes Zeichen oder Symbol anzeigen kann. Diese Methode der Darstellung ermöglicht eine effiziente und geordnete Ausgabe von Text, ähnlich der klassischen Textkonsolen-Anzeige, ohne die Nutzung komplexer Grafikelemente.

### Aufgabenbeschreibung

Implementieren Sie in der Datei `textmode.s` zwei Funktionen zur Verwaltung des Textmodus. Diese Funktionen sind essenziell für die Initialisierung und das Ansprechen der Textanzeige innerhalb des Modus.

#### 1. `textmode_init`

Ziel: Diese Funktion initialisiert den Textmodus und bereitet die grafische Oberfläche für die Textausgabe vor.

Vorgehensweise:
- Überprüfen Sie zunächst, ob der Textmodus bereits aktiviert ist, indem Sie den Wert der globalen Variable `textmode_state` abfragen.
- Ist der Modus nicht aktiv, setzen Sie `textmode_state` auf einen Wert, der den aktiven Zustand anzeigt (z. B. `0xF`).
- Rufen Sie anschließend die Funktion `canvas_init` auf, um die grafische Leinwand zu initialisieren.
- Verwenden Sie `fillscreen`, um den Bildschirm entweder zu löschen oder mit einer Hintergrundfarbe zu füllen.
- Sollte der Textmodus bereits aktiv sein, muss keine weitere Aktion ausgeführt werden.

#### 2. `textmode_get_tabentry`

Ziel: Diese Funktion berechnet die Speicheradresse eines bestimmten Zeichens innerhalb der Texttabelle, basierend auf einem gegebenen Index (`r1`) und einer Hintergrundfarbe (`r2`).

Vorgehensweise:
- Der übergebene Index muss innerhalb der Tabellengrenzen liegen. Diese Tabelle wird beispielsweise auf 80 Spalten und 60 Zeilen (maximal 4800 Einträge) beschränkt.
- Sollte der Index den Wert 0 haben, rufen Sie die Funktion `fillscreen` auf, um den Bildschirm mit der angegebenen Hintergrundfarbe zu löschen.
- Wenn der Index gültig ist, berechnen Sie die exakte Speicheradresse des Zeichens auf der Leinwand, basierend auf der Tabelle.

### Hinweise

- Verwenden Sie die globale Variable `canvas_base`, die die Basisadresse der Leinwand speichert, sowie `textmode_state`, um den aktuellen Status des Textmodus zu verwalten.
- Externe Funktionen:
- `canvas_init`: Initialisiert die grafische Leinwand.
- `fillscreen`: Löscht den Bildschirm oder füllt ihn mit einer Hintergrundfarbe.


|------------------------------|------------------------------------|-------------------------------|
|   [zurück](canvas_lsg.md)   |   [Hauptmenü](../ueberblick.md)    |   [weiter](textmode_lsg.md)   |


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
