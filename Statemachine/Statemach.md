# A.3 Verknüpfungen von Basic Blocks
## 3.2.8 Kontrollstrukturen: Zustandsautomaten

**Zustandsautomaten** (State Machines) sind ein essenzielles Werkzeug in der Programmierung, da sie komplexe Abläufe systematisch und nachvollziehbar abbilden können. Ein Zustandsautomat besteht aus einer endlichen Anzahl von Zuständen, die durch Übergänge miteinander verbunden sind. Jeder Zustand kann bestimmte Aktionen ausführen, und die Übergänge zwischen den Zuständen erfolgen basierend auf vordefinierten Bedingungen oder Eingaben. Diese klare Strukturierung ermöglicht es Entwicklern, Abläufe und Entscheidungslogik in Software übersichtlich zu gestalten.

Zustandsautomaten finden breite Anwendung in verschiedenen Bereichen der Softwareentwicklung. In **Videospielen** werden sie beispielsweise zur Verwaltung von Spielerzuständen, Gegnerverhalten oder verschiedenen Spielphasen eingesetzt. In **Kommunikationsprotokollen** helfen Zustandsautomaten beim Handhaben von Verbindungsaufbau, Datenübertragung und Verbindungsabbau. Auch in **Benutzerschnittstellen** sind sie unverzichtbar, etwa bei der Navigation durch Menüs oder Dialogfenster. Darüber hinaus kommen sie etwa in der **Automobilindustrie** zur Steuerung von Motorfunktionen oder Fahrerassistenzsystemen zum Einsatz.

Die Vorteile von Zustandsautomaten sind vielfältig und tragen maßgeblich zu ihrer Beliebtheit bei Entwicklern bei. Ihre **Modularität** ermöglicht es, Systeme einfach zu erweitern und zu warten, da neue Zustände oder Übergänge hinzugefügt werden können, ohne die bestehende Logik zu beeinträchtigen. Dies führt zu einer **erhöhten Wartbarkeit** und erleichtert die Anpassung an sich ändernde Anforderungen. Zudem reduziert die systematische Strukturierung von Zustandsautomaten die Fehleranfälligkeit erheblich. Jeder Zustand und jeder Übergang ist explizit definiert, was das **Debugging** und die **Validierung** der Logik erleichtert und somit die **Fehlerreduktion** fördert.

Ein weiterer bedeutender Vorteil ist die **Nachvollziehbarkeit** komplexer Abläufe. Zustandsautomaten stellen Abläufe übersichtlich dar, was das Verständnis und die Kommunikation innerhalb von Entwicklungsteams verbessert. Jeder Zustand und Übergang ist klar dokumentiert, was die **Transparenz** und **Klarheit** der Softwarearchitektur erhöht. Darüber hinaus bieten Zustandsautomaten eine hohe **Wiederverwendbarkeit**, da sie in verschiedenen Projekten und Kontexten eingesetzt werden können. Dies spart Entwicklungszeit und sorgt für eine konsistente Implementierung von Abläufen.

Die **Testbarkeit** von Zustandsautomaten ist ebenfalls ein großer Pluspunkt. Durch die klare Trennung der Zustände und Übergänge können einzelne Komponenten isoliert und gezielt getestet werden, was die **Qualität der Software** insgesamt erhöht. Zudem sind Zustandsautomaten sehr **skalierbar** und eignen sich sowohl für einfache als auch für sehr komplexe Systeme. Ihre flexible Struktur passt sich den wachsenden Anforderungen an und unterstützt die **Skalierbarkeit** der Anwendung.

|-----------------------------------------------------------------------------------------------------------------------------------------|
|[Weiterführende Informationen zu Zustandsautomaten](https://www.youtube.com/watch?v=58N2N7zJGrQ&list=PLBlnK6fEyqRgp46KUv4ZY69yXmpwKOIev) |

### Beispiel einer simplen Statemachine in Assembler
Ein einfaches Beispiel für einen Zustandsautomaten verdeutlicht die praktische Umsetzung. Das folgende Beispiel umfasst vier Zustände (`state0` bis `state3`) und zeigt sowohl unbedingte als auch bedingte Zustandsübergänge:

```asm
.section .data
    current_state: .word state0  
    
.section .text

_start:

statemachine:
    ldr r0, =current_state   @ Lade die Adresse der aktuellen Zustandsvariable
    mov lr, pc               @ Speichere die Rücksprungadresse im Link-Register
    ldr pc, [r0]             @ Lade den nächsten Zustand in den Program Counter
    b  statemachine          @ Endlosschleife

state0:
    @ Aktionen für state0
    adr r0, state1           @ Setze den nächsten Zustand auf state1
    ldr r1, =current_state
    str r0, [r1]             @ Aktualisiere die Zustandsvariable
    bx lr                    @ Rückkehr zur Hauptschleife

state1:
    @ Aktionen für state1
    adr r0, state2           @ Setze den nächsten Zustand auf state2
    ldr r1, =current_state
    str r0, [r1]
    bx lr

state2:
    @ Aktionen für state2
    cmp r3, #0               @ Vergleiche Register r3 mit 0
    adreq r0, state1         @ Wenn gleich, setze nächsten Zustand auf state1
    adrne r0, state3         @ Wenn ungleich, setze nächsten Zustand auf state3
state2_nextstate:
    ldr r1, =current_state
    str r0, [r1]
    bx lr

state3:
    @ Aktionen für state3
    adr r0, state0           @ Setze den nächsten Zustand auf state0
    ldr r1, =current_state
    str r0, [r1]
    bx lr
```

#### Erläuterung des Beispiels
Zunächst wird die Zustandsvariable `current_state` in der Datensektion initialisiert und auf `state0` gesetzt. Die Hauptschleife der Zustandsmaschine lädt die Adresse der aktuellen Zustandsvariable, speichert die Rücksprungadresse im Link-Register (`lr`) und lädt den nächsten Zustand in den Program Counter (`pc`). Dadurch wird die entsprechende Zustandsroutine aufgerufen, die spezifische Aktionen ausführt und den nächsten Zustand bestimmt. Nach der Ausführung eines Zustands wird zur Hauptschleife zurückgesprungen, wodurch ein endloser Zyklus entsteht.

Jeder Zustand (`state0` bis `state3`) enthält Platzhalter für Aktionen und bestimmt den nächsten Zustand durch Adressierung und Aktualisierung der Zustandsvariable. Insbesondere `state2` demonstriert einen bedingten Übergang, bei dem basierend auf dem Vergleich des Registers `r3` mit `0` entschieden wird, ob der nächste Zustand `state1` oder `state3` sein soll. Schließlich kehrt `state3` zu `state0` zurück, wodurch der Zyklus geschlossen wird.

**Fazit:** Zustandsautomaten sind ein mächtiges Konzept zur Strukturierung und Verwaltung komplexer Abläufe in der Programmierung. Ihre Implementierung in Assembler bietet tiefe Einblicke in die Funktionsweise von Maschinen auf niedriger Ebene und erfordert ein gutes Verständnis der Architektur und der Assembler-Syntax. Dank ihrer Modularität, Fehlerreduktion, Nachvollziehbarkeit, Wiederverwendbarkeit, Testbarkeit und Skalierbarkeit sind Zustandsautomaten ein unverzichtbares Werkzeug für Entwickler, insbesondere in Anwendungen, die klare Zustandsübergänge und -verwaltung erfordern.


Zunächst wird die Zustandsvariable `current_state` im Datenbereich initialisiert und auf `state0` gesetzt. Die Hauptschleife der Zustandsmaschine lädt die Adresse der aktuellen Zustandsvariable und speichert die Rücksprungadresse zur Hauptschleife im `Link-Register (lr)` mittels `mov lr, pc`. Dabei ist wichtig zu beachten, dass der `Program Counter` in 32-Bit ARM-Architekturen auf die Adresse der aktuellen Instruktion plus `0x8` Bytes zeigt. Dies bedeutet, dass im `lr` die Adresse der Instruktion `b statemachine` gespeichert wird.

Anschließend wird der nächste Zustand aus der Zustandsvariable geladen und in den `Program Counter` geschrieben, wodurch die **entsprechende Zustandsroutine** aufgerufen wird. Diese Routine führt **spezifische Aktionen** aus und bestimmt den **nächsten Zustand**. Nach der Ausführung eines Zustands wird mit `bx lr` zur Adresse zurückgesprungen, die im `Link-Register` gespeichert ist, also zur `b statemachine`-Instruktion. Dadurch gelangt die Ausführung zurück zur Hauptschleife, und der Zyklus beginnt von vorne. Dieser Mechanismus stellt sicher, dass nach jedem Zustandsübergang die Hauptschleife erneut durchlaufen wird, wodurch eine Dauerschleife entsteht.

Jeder Zustand (`state0` bis `state3`) enthält Platzhalter für Aktionen und bestimmt den nächsten Zustand durch Adressierung und Aktualisierung der Zustandsvariable. `state2` demonstriert einen **bedingten Übergang**, bei dem basierend auf dem Vergleich des Registers `r3` mit `0` entschieden wird, ob der nächste Zustand `state1` oder `state3` sein soll. Schließlich kehrt `state3` zu `state0` zurück.

|--------------------------|------------------------------------|-----------------------------------------------|
|   [zurück](do_while.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](../Lookuptables/lookuptable.md)    |