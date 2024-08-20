## Die Interruptvektortabelle

Eine Interruptvektortabelle ist eine spezielle Tabelle im Speicher, die eine Sammlung von Interruptvektoren enthält. Jeder Interruptvektor in dieser Tabelle ist eine Speicheradresse, die auf den Anfang einer Interrupt-Service-Routine (ISR) zeigt. 
Eine ISR ist ein spezielles Programm, das ausgeführt wird, wenn ein bestimmter Interrupt auftritt. 
Wenn ein bestimmter Interrupt auftritt, springt der Prozessor an die Stelle des jeweiligen Interruptvektor, um die passende ISR auszuführen. 

[Interrupt Handler](ihandler.md)