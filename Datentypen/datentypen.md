# Datentypen 
Programme arbeiten mit Informationen ganz unterschiedlicher Art. Diese Informationen können **Texte** sein, die vom Benutzer eingegeben oder angezeigt werden, **Ganzzahlen** für einfache mathematische Berechnungen, **Gleitkommazahlen** für präzisere, wissenschaftliche Berechnungen oder **Zeiger**, um auf bestimmte Speicheradressen zuzugreifen. Für derart unterschiedliche Arten von Informationen braucht es verschiedene Datentypen, um diese sachgerecht zu bearbeiten. Jeder Datentyp erfüllt dabei spezifische Aufgaben und ist für bestimmte Arten von Operationen ausgelegt.

In der Assemblerprogrammierung sind Datentypen grundlegend für die korrekte Verarbeitung und Speicherung von Daten. Sie definieren die Größe und Art der Daten, die von den Anweisungen verarbeitet werden, und beeinflussen somit direkt die Effizienz und Struktur des Programmflusses und der Datenverarbeitung. 

Auf Maschinenebene beziehen sich Datentypen auf die Art und Weise, wie Daten im Speicher und in Registern des Prozessors interpretiert werden. Es gibt **keinen physischen Unterschied** zwischen einem 32-Bit-Wert, der als Ganzzahl, Gleitkommazahl oder Zeiger verwendet wird – letztlich sind sie alle nur **Bitmuster**. Der Unterschied liegt einzig in der **Interpretation** dieser Werte im Rahmen eines Programms, das heißt darin, welche Operationen an ihnen durchgeführt werden.

Der Programmierer muss also sicherstellen, dass das Programm die Werte im Speicher als den richtigen Datentyp behandelt. Beispielsweise muss eine Zeichenkette korrekt als solche interpretiert werden, damit entsprechende Operationen richtig ausgeführt werden. 

[Einfache Datentypen](einfachdtypen.md)

[Komplexe Datentypen](komplexedtypen.md)














