# B.3 Implementierung systemnaher Funktionen
## 3.1.9 Systemnahe Funktionen: Aufgabe Implementierung einer formatierenden Ausgabefunktion in ARM-Assembly

### Aufgabenstellung:

Eine Funktion `kprintf` soll in ARM-Assembler implementiert werden, die ähnlich wie die printf-Funktion aus der C-Standardbibliothek arbeitet. Die Funktion verarbeitet einen übergebenen Formatstring, der Platzhalter für verschiedene Datentypen enthält, und fügt die zugehörigen Argumente entsprechend ein. Die Ausgabe erfolgt auf ein definiertes Ausgabemedium wie UART oder ein Display.

### Was ist ein Formatstring?
Ein Formatstring ist eine Zeichenkette, die neben normalen Zeichen auch spezielle Platzhalter enthält, die durch %-Zeichen gekennzeichnet sind. Diese Platzhalter geben an, dass an dieser Stelle ein bestimmter Datentyp (z. B. eine Zahl oder eine Zeichenkette) eingefügt werden soll.

Ein Beispiel für einen Formatstring:
```
"Fehlercode %d: %s"
```
Hier steht:
- **%d** für eine Ganzzahl (Fehlercode),
- **%s** für eine Zeichenkette (Fehlerbeschreibung).
Wenn die Funktion diesen Formatstring mit den Argumenten 404 und "Datei nicht gefunden" verarbeitet, wird die Ausgabe:
```
"Fehlercode 404: Datei nicht gefunden"
```

### Anforderungen:

#### Verarbeitung des Formatstrings:

Die Funktion soll den übergebenen Formatstring Zeichen für Zeichen durchlaufen.
Normale Zeichen sollen direkt in einen Ausgabepuffer kopiert werden.
Bei Auftreten eines `%`-Zeichens soll ein Formatierungsspezifikator erkannt und verarbeitet werden.
Unterstützte Formatierungsspezifikatoren:

- **%d oder %i:** Vorzeichenbehaftete Dezimalzahl
- **%u:** Vorzeichenlose Dezimalzahl
- **%f:** Gleitkommazahl
- **%x:** Hexadezimale Zahl
- **%s:** Zeichenkette


Optional: Unterstützung von Feldbreitenangaben nach dem **%**-Zeichen (z. B. %5d) 

#### Argumentverarbeitung:

Die Funktion soll eine variable Anzahl von Argumenten verarbeiten, abhängig von den im Formatstring enthaltenen Spezifikatoren.
Die Adresse des Formatstrings soll in `r1` an kprintf übergeben werden und das Ausgabemedium über `r2` übergeben werden. 
Die zusätzlichen Argumente werden auf dem Stack übergeben.
Ein Parameterzähler soll verwendet werden, um die Position der nächsten Argumente auf dem Stack zu bestimmen.

#### Ausgabepuffer:

Ein Puffer von ausreichender Größe (z. B. 1024 Bytes) soll verwendet werden, um die Ausgabezeichenkette aufzubauen.
Es soll geprüft werden, dass der Puffer nicht überläuft.

#### Stack-Management:

Wichtige Register müssen zu Beginn der Funktion gesichert und am Ende wiederhergestellt werden.
Lokale Variablen sollen über feste Offsets vom Frame-Pointer verwaltet werden.
Die Funktion soll den Stack korrekt handhaben, um mit einer variablen Anzahl von Argumenten umzugehen.

#### Ausgabe:

Nach vollständiger Verarbeitung soll der Inhalt des Ausgabepuffers auf das definierte Ausgabemedium ausgegeben werden.
Die Funktion `kwrite` soll für die tatsächliche Ausgabe verwendet werden, wobei sie den Ausgabetyp, die Pufferadresse und die Anzahl der zu schreibenden Bytes erhält.

#### Returnwert:

Der Rückgabewert der Funktion printf ist ein Wert in `r0`, der die Anzahl der erfolgreich gedruckten Zeichen angibt. Wenn ein Fehler auftritt, ist der Rückgabewert `-1`.
Zu den gedruckten Zeichen zählen alle sichtbaren Zeichen sowie spezielle Zeichen wie Leerzeichen und Zeilenumbrüche `/n`.

### Hinweise:

- Verwenden Sie externe Funktionen, wo notwendig, z. B. für die Konvertierung von Zahlen in Zeichenketten (`num_2_dec`, `num2hexascii`).
- Erleichtern sie sich das Leben und kommentieren Sie Ihren Code ausreichend, um die Funktionsweise zu erläutern und dadurch die Wartbarkeit zu erhöhen.


|---------------------------|------------------------------------|-------------------------------|
|   [zurück](kread_lsg.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](kprintf_lsg.md)    |


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