# B.1 Einführung
## 1.6.4 Priviligierungslevel: Statusregister und Betriebsmodi

Das **CPSR (Statusregister)** ist ein zentrales Register, das den aktuellen Zustand, wie etwa die ALU-Flags und den aktuellen Betriebsmodus des Prozessors speichert. Die Flags werden automatisch indirekt durch arithmetische und logische Operationen oder Vergleichegesetzt. Die **Bits 4 bis 0** des Statusregisters bestimmen, in welchem Modus der Prozessor arbeitet. 
Unpriviligierte Modi haben im Gegensatz zu priviligierten Modi keinen direkten(!) Zugriff auf das CPSR zur Änderung der ALU-Flags oder der Betriebsmodi. Dieser Schutzmechanismus verhindert, dass Anwendungsprogramme den kritischen Systemstatus beeinflussen können, was die Sicherheit des Systems gewährleistet.

|  Mode:  | Encoding: |  Priv.-Level:  |      
|---------|-----------|----------------|
|usr:     |  10000    |     PL0        |         
|fiq:     |  10001    |     PL1        |    
|irq:     |  10010    |     PL1        |    
|svc:     |  10011    |     PL1        |    
|mon:     |  10110    |     PL1        |    
|abt:     |  10111    |     PL1        |    
|hyp:     |  11010    |     PL2        |    
|und:     |  11011    |     PL1        |    
|sys:     |  11111    |     PL1        |    

Ein Wechsel von einem **niedrigeren zu einem höheren Privilegierungslevel**, wie vom User-Modus zum Supervisor-Modus, erfolgt durch Systemaufrufe (Syscalls) und Interrupts. Dieser Mechanismus stellt sicher, dass der Wechsel in einen privilegierten Modus **nur unter kontrollierten Bedingungen** stattfindet. Umgekehrt können privilegierte Modi den Prozessor jedoch in einen weniger privilegierten Modus zurückversetzen, um die Kontrolle an weniger kritische Programme zu übergeben.

Um in der Systemprogrammierung auf einem ARM-Prozessor direkt auf Steuer- und Statusregister zugreifen zu können, verwendet man spezielle Instruktionen wie MRS und MSR.

### MSR: Move to Special Register
#### Syntax:
```
MSR  <Destination>, <Source>
```
### MRS: Move from Special Register
#### Syntax: 
```
MRS  <Destination>, <Source>
```
Beim Starten des Prozessors wollen wir überprüfen, ob er sich im Supervisormode befindet und wenn ihn "Schlafen legen", also in eine Dauerschleife führen, sollte das nicht der Fall sein.

**EQUS für die benötigten Konstanten:**
```asm
.equ    MODE_MASK,      0x1F
.equ    MODE_USR,       0x10
.equ    MODE_IRQ,       0x12
.equ    MODE_SVC,       0x13

...
```

**Überprüfen des Betriebsmodus:**
```asm
        MRS r0, cpsr  
        MOV r1, #MODE_MASK
        AND r2, r0, r1
        CMP r2, #MODE_SVC
        BNE sleep
        ...
sleep:
        B sleep
```

|-------------------------|------------------------------------|---------------------------|
|   [zurück](betrmod.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](irqpriv.md)    |


|**1.6 Priviligierungslevel**                                                       |
|-----------------------------------------------------------------------------------|
| [1.6.1 Priviligierungslevel und Prozessormodi](privmodiintro.md)                  |
| [1.6.2 Privilegierungslevel (PL0, PL1, PL2)](privlev.md)                          |
| [1.6.3 Betriebsmodi](betrmod.md)                                                  |
| [1.6.4 Statusregister und Betriebsmodi](cpsrmod.md)                               |
| [1.6.5 Interrupts und Priviligierungslevel](irqpriv.md)                           |
| [1.6.6 Rolle der MMU bei den Priviligierungslevel des Prozessors](mmupriv.md)     |