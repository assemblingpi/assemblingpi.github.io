# Einführung in die ARM-Architektur

## Load/Store-Architektur

Bei ARM-CPUs handelt es sich um eine Load/Store-Architektur, bei denen Speicherzugriffe nur mittels spezieller Befehle (LDR und STR) durchgeführt werden können und direkte Berechnungen auf Daten im Speicher nicht möglich sind (im Gegensatz zu x86 CPUs). Rechenoperationen finden demnach ausschließlich in den Registern (kleine Speichereinheiten) der CPU statt. Dies bedeutet, dass Daten für ihre Verarbeitung zuerst vom Speicher in Register geladen und nach der Berechnung wieder in den Speicher zurückgeschrieben werden müssen. 

Innerhalb der ARM-CPU gibt es verschiedene 32-Bit Register, die für spezifische Aufgaben verwendet werden:

- **General-Purpose Register**: Diese Register dienen allgemeinen Berechnungen und Datenoperationen und sind vielseitig einsetzbar.

- **Stack Pointer (SP)**: Verwaltet den Stack im Speicher und zeigt auf den aktuellen **Top of Stack**. Er wird verwendet, um temporäre Daten und Rücksprungadressen zu speichern.

- **Link Register (LR)**: Speichert die Rücksprungadresse bei Funktionsaufrufen, um nach der Ausführung der Funktion zur ursprünglichen Adresse zurückzukehren.

- **Frame Pointer (FP)**: Wird verwendet, um auf Parameter und lokale Variablen im Stack zuzugreifen.

- **Program Counter (PC)**: Beinhaltet die Adresse der nächsten auszuführenden Instruktion. Der PC wird nach jeder Instruktion automatisch erhöht, und bei Verzweigungen angepasst, um zur neuen Instruktionsadresse zu springen.

- **Statusregister**: Ein 32-Bit-Register, das Flags enthält, die den Status des Prozessors anzeigen, wie etwa den Zustand vorheriger Operationen.

[weiter](../ZahlensysBitsBytes/zbb.md)