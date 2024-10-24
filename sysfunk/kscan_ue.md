# B.3 Implementierung systemnaher Funktionen
## 3.1.10 Systemnahe Funktionen: Aufgabe Implementiere `kscan` für formatiertes Einlesen

Entwickeln Sie die Assembly-Funktion `kscan`, die Benutzereingaben entsprechend einem vorgegebenen Formatstring verarbeitet. Die Funktion soll in der Lage sein, Eingaben für die Platzhalter `%d` (Dezimalzahl) und `%s` (Zeichenkette) einzulesen und die Werte an die entsprechenden Speicheradressen zu speichern. 

Die Übergabe der Parameter erfolgt wie folgt: Die Adresse des nullterminierten Formatstrings wird im Register `r1` übergeben. Die weiteren Argumente, die die Zieladressen für die eingelesenen Werte sind, werden über den Stack übergeben, wobei die zuerst verwendeten Argumente zuletzt gepusht werden müssen. 

Beim Aufruf der Funktion `kscan` soll der Rückgabewert die Anzahl der erfolgreich eingelesenen und zugewiesenen Werte sein. Tritt ein Fehler auf, beispielsweise durch einen ungültigen Formatbezeichner, soll die Funktion einen negativen Wert zurückgeben.

|-----------------------------|------------------------------------|-----------------------------|
|   [zurück](kprintf_lsg.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](kscan_lsg.md)    |


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