## Aufgabenstellung: Implementiere `kscan` für formatiertes Einlesen

Entwickeln Sie die Assembly-Funktion `kscan`, die Benutzereingaben entsprechend einem vorgegebenen Formatstring verarbeitet. Die Funktion soll in der Lage sein, Eingaben für die Platzhalter `%d` (Dezimalzahl) und `%s` (Zeichenkette) einzulesen und die Werte an die entsprechenden Speicheradressen zu speichern. 

Die Übergabe der Parameter erfolgt wie folgt: Die Adresse des nullterminierten Formatstrings wird im Register `r1` übergeben. Die weiteren Argumente, die die Zieladressen für die eingelesenen Werte sind, werden über den Stack übergeben, wobei die zuerst verwendeten Argumente zuletzt gepusht werden müssen. 

Beim Aufruf der Funktion `kscan` soll der Rückgabewert die Anzahl der erfolgreich eingelesenen und zugewiesenen Werte sein. Tritt ein Fehler auf, beispielsweise durch einen ungültigen Formatbezeichner oder fehlende Argumente, soll die Funktion `-1` zurückgeben und eine entsprechende Fehlermeldung ausgeben.

|-----------------------------|------------------------------------|-----------------------------|
|   [zurück](kprintf_lsg.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](kscan_lsg.md)    |
