## VMSR und VMRS: Steuerung und Statusübertragung zwischen ARM- und NEON/VFP-Statusregistern

**VMSR (Vector Move to Special Register)**
```asm
vmsr FPSCR, Rx
```
Der Befehl VMSR überträgt den Inhalt eines ARM-Registers in das FPSCR (Floating-Point Status and Control Register).
Dieser Befehl ist vonnöten um den Zustand von NEON- und Gleitkommaoperationen durch direkte Manipulation des FPSCR zu steuern. Da NEON- und Gleitkommaoperationen spezifische Anforderungen haben, die vom Statusregister gesteuert werden (z.B. Rundungsmodi, Ausnahmebedingungen), ermöglicht VMSR den Entwicklern, diese Modi direkt zu setzen.


**VMRS (Vector Move from Special Register)**
```asm
vmrs Rd, FPSCR
```
Der Befehl VMRS überträgt den aktuellen Status aus einem NEON- oder VFP-Statusregister (z.B. FPSCR) in ein ARM-Register.
Dies ist wichtig, um den aktuellen Status und die Ergebnisse von NEON- oder Gleitkommaoperationen zu lesen. Da das FPSCR Zustände wie Überlauf, Unterlauf, ungültige Operationen, usw. speichert, ermöglicht VMRS es, diesen Status auszulesen und entsprechende Entscheidungen im Programm zu treffen.

### Achtung: Bedingte Ausführung von NEON-Instruktionen hängt vom CPSR ab!
Wichtig zu beachten ist, dass die Bedingungsflags im FPSCR (wie N, Z, C, V) zuerst in das CPSR übertragen werden müssen, um sie für bedingte Ausführungen von NEON- und Gleitkomma-Instruktionen verwenden zu können!

**1. Übertragen der Flags vom FPSCR in ein ARM-Register**
Zuerst müssen die Flags aus dem FPSCR in ein ARM-Register übertragen werden. Dies geschieht mithilfe des Befehls VMRS:

```asm
vmrs r0, fpscr  @ Übertrage die FPSCR-Flags in das ARM-Register r0
```
**2. Übertragen der Flags vom ARM-Register in das CPSR**
Nachdem die Flags im ARM-Register gespeichert sind, können sie in das CPSR übertragen werden. Dies erfolgt mit dem Befehl MSR:

```asm
msr cpsr_f, r0  @ Übertrage die Flags vom ARM-Register r0 in das CPSR
```

**Der alte CPSR Inhalt muss selbstverständlich gespeichert werden, anderenfalls kommt es zu Problemen im weiteren Programmablauf!**