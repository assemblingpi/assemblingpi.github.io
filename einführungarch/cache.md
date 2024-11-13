# A.1 Einführung
## 1.2.4 Grundlagen der Computerarchitektur: Cache

Da der direkte Zugriff auf den Hauptspeicher jedoch vergleichsweise langsam ist, verwenden moderne CPUs einen schnellen Zwischenspeicher, den sogenannten **Cache**. Der Cache speichert häufig verwendete Daten, sodass sie ohne Verzögerung zur Verfügung stehen. Er verhindert so, dass der langsame Hauptspeicher zum Engpass bei der Datenverarbeitung wird. 

### Räumliche und zeitliche Lokalität

Die Effektivität eines Caches beruht auf zwei Prinzipien, die sich auf typische Datenzugriffsmuster beziehen: *räumliche* und *zeitliche Lokalität*.

- **Räumliche Lokalität** bedeutet, dass Daten, die nahe beieinander im Speicher liegen, oft gemeinsam genutzt werden. Wenn ein Programm z. B. ein Array durchläuft, sind die Elemente im Speicher nebeneinander angeordnet und es ist effizient, diese Blöcke auf einmal zu laden. Ein Cache kann somit Datenblöcke auf Vorrat speichern und die Leistung steigern, indem es nur wenige große Ladeoperationen statt vieler kleiner gibt.

- **Zeitliche Lokalität** beschreibt, dass kürzlich verwendete Daten oft wieder gebraucht werden. Beispielsweise greift eine Schleife in einem Programm häufig wiederholt auf dieselben Variablen zu. Der Cache kann hier durch das Speichern von kürzlich verwendeten Daten Zugriffe auf den Hauptspeicher minimieren und so die Leistung steigern.

### 1.2.2 Cache-Arten

Die Daten im Cache sind in kleine Einheiten, sogenannte *Cache-Lines*, organisiert. Abhängig davon, wie flexibel Speicherblöcke diesen Cache-Lines zugeordnet werden können, gibt es unterschiedliche Cache-Architekturen: *direct-mapped*, *fully associative* und *set-associative*. Die Flexibilität der Zuordnung beschreibt, wie viele Möglichkeiten ein Speicherblock hat, in einer bestimmten Cache-Line gespeichert zu werden. Ein Cache in einer CPU verwendet stets eine dieser Architekturen, die je nach Systemanforderung gewählt wird:

- **Direct-Mapped Cache**  
  Im *direct-mapped Cache* hat jeder Speicherblock eine fest zugeordnete Cache-Line, also nur einen einzigen Platz im Cache. Diese feste Zuordnung ermöglicht eine schnelle Überprüfung, ob ein Block im Cache liegt, erhöht jedoch das Risiko, dass sich Blöcke gegenseitig verdrängen, wenn sie denselben Platz beanspruchen.

- **Fully Associative Cache**  
  Der *fully associative Cache* ist maximal flexibel: Jeder Speicherblock kann in jede beliebige Cache-Line geladen werden. Diese Freiheit reduziert Konflikte, da der Block stets in eine freie Line geschrieben werden kann. Die Suche nach einem Block ist hier jedoch aufwendiger, da die CPU theoretisch den gesamten Cache durchsuchen muss.

- **Set-Associative Cache**  
  Der *set-associative Cache* stellt einen Kompromiss dar, indem er den Cache in *Sets* unterteilt und jedem Speicherblock ein festes Set zuordnet. Innerhalb dieses Sets kann der Block flexibel in eine von mehreren Lines abgelegt werden. Ein *4-way set-associative Cache* etwa bietet pro Set vier mögliche Cache-Lines. Das reduziert die Konflikte und vereinfacht die Suche, da die CPU nur das jeweilige Set durchsuchen muss.

Diese Cache-Typen bieten verschiedene Balancepunkte zwischen Effizienz und Flexibilität. Der *direct-mapped Cache* ist einfach und schnell, der *fully associative Cache* flexibel, aber komplex, und der *set-associative Cache* vereint Flexibilität und Effizienz und wird daher häufig verwendet.



### Cache-Größen und Cache-Line-Größen
Die Leistung und Effizienz des Caches hängen maßgeblich von seiner Größe und der Größe der Cache-Lines ab:

- **Cache-Größe:**
Die Cache-Größe bestimmt, wie viele Daten gespeichert werden können. Ein größerer Cache erhöht die Wahrscheinlichkeit, benötigte Daten schnell bereitzuhalten, ist jedoch teurer und verbraucht mehr Energie. Daher wird die Größe je nach Anforderung angepasst: Kleine Anwendungen kommen mit kleinem Cache aus, während datenintensive Anwendungen von einem größeren Cache profitieren.

- **Cache-Line-Größe:**
Die Cache-Line-Größe bestimmt, wie viele Bytes pro Eintrag im Cache gespeichert werden. Kleine Cache-Lines bieten mehr Kontrolle und minimieren unnötige Datenzugriffe, während größere Cache-Lines die räumliche Lokalität besser ausnutzen. Große Cache-Lines können jedoch Platz verschwenden, wenn Daten nicht benötigt werden. Die optimale Größe hängt vom Zugriffsmuster ab: Lokale Zugriffe profitieren von größeren Cache-Lines, verstreute Zugriffe von kleineren.


### Write Policy

Die *Write Policy* eines Caches bestimmt, wie Schreibzugriffe auf den Cache gehandhabt und mit dem Hauptspeicher synchronisiert werden. Diese Entscheidung beeinflusst sowohl die Leistung als auch die Datenkonsistenz:

- **Write-Through**  
Bei einer *Write-Through Policy* wird jeder Schreibzugriff auf den Cache sofort an den Hauptspeicher weitergegeben. Die Daten sind dadurch im Cache und im Hauptspeicher stets synchron, was die Konsistenz gewährleistet. Allerdings führt dies zu mehr Speicherzugriffen, was die Geschwindigkeit beeinträchtigen kann.

- **Write-Back**  
Bei der *Write-Back Policy* werden Änderungen zunächst nur im Cache gespeichert und erst dann in den Hauptspeicher geschrieben, wenn die Daten aus dem Cache verdrängt werden müssen. Diese Methode verringert die Anzahl der Schreibzugriffe und kann die Leistung erhöhen. Allerdings sind die Daten im Hauptspeicher nicht sofort aktuell, was zu Inkonsistenzen führen kann, wenn keine geeigneten Synchronisationsmechanismen vorhanden sind.

Die Wahl zwischen Write-Through und Write-Back hängt von den Anwendungsanforderungen ab. Write-Through wird oft verwendet, wenn Datenkonsistenz für andere Systemkomponenten wichtig ist, während Write-Back bei Anwendungen bevorzugt wird, die hohe Leistung erfordern.

### Cache-Replacement-Policy

Da der Cache kleiner ist als der Hauptspeicher, können nicht alle Daten gleichzeitig gespeichert werden. Wenn der Cache voll ist und neue Daten geladen werden müssen, bestimmt eine *Cache-Replacement-Policy*, welche Daten ersetzt werden sollen. Diese Policy ist entscheidend für die Cache-Effizienz und die Wahrscheinlichkeit eines Cache Hits. Je nach Systemanforderungen gibt es verschiedene Strategien:

- **Least Recently Used (LRU)**  
Die *Least Recently Used*-Strategie ersetzt den Speicherblock, auf den am längsten nicht zugegriffen wurde. Sie ist oft eine gute Wahl, erfordert jedoch, dass die Zugriffsreihenfolge kontinuierlich nachverfolgt wird.

- **First-In, First-Out (FIFO)**  
Bei der *First-In, First-Out*-Strategie wird der älteste Block im Cache ersetzt. Diese Methode ist einfacher zu implementieren als LRU, kann jedoch ineffizient sein, wenn ältere Daten noch benötigt werden.

- **Random Replacement**  
Die *Random Replacement*-Strategie ersetzt zufällig einen Speicherblock. Diese Methode ist besonders einfach umzusetzen, kann aber dazu führen, dass wichtige Daten verloren gehen.

- **Least Frequently Used (LFU)**  
Die *Least Frequently Used*-Strategie ersetzt den am seltensten verwendeten Block, was vor allem bei stabilen Zugriffsmustern sinnvoll ist. Allerdings ist der Verwaltungsaufwand hoch, da die Zugriffshäufigkeit verfolgt werden muss.

Die Wahl der Replacement-Policy hängt von der Cache-Architektur und den Systemanforderungen ab. Jede dieser Strategien zielt darauf ab, die Wahrscheinlichkeit zu erhöhen, dass die CPU beim nächsten Zugriff die benötigten Daten im Cache vorfindet.



|---------------------|-------------------------------|---------------------|
| [zurück](endilsg.md)| [Hauptmenü](../ueberblick.md) | [weiter](eaintro.md)| 


| **1.2 Grundlagen der Computerarchitektur**                                                |
|-------------------------------------------------------------------------------------------|
| [1.2.1 Die Ausführung von Programmen durch den Computer](../einführungarch/cpuintro.md)   |
| [1.2.2 Von-Neumann-Architektur](../einführungarch/archintro.md)                           |
| [1.2.3 Der Speicher](../einführungarch/memintro.md)                                       |
| [1.2.4 Cache](../einführungarch/cache.md)                                    |
| [1.2.5 Ein- und Ausgabegeräte](../einführungarch/eaintro.md)                              |
| [1.2.6 Der Systembus](../einführungarch/sbusintro.md)                                     |
| [1.2.7 Der Befehlszyklus](../einführungarch/archintro_pip.md)                             |