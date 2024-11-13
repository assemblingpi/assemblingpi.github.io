# B.1 Einführung
## 1.7 Boot: Einrichtung des Projekts und Deklaration der Hauptdateien
Zu Beginn wird ein Projektordner erstellt, in dem die folgenden Dateien angelegt werden: `boot.s`, `kmain.s`, `build.sh` und `link.lds`. Diese Dateien dienen als Grundlage für das Booten und den Aufbau der Systemstruktur. Jede dieser Dateien übernimmt spezifische Aufgaben, wie den Start des Systems, die Initialisierung der Hauptfunktionen und das Verlinken der nötigen Programmabschnitte. 

**Für alle weiteren Programmteile, die im Laufe dieses Tutorials erstellt werden, gilt ebenso folgendes Vorgehen: Zu jeder neuen Komponente wird eine Quelldatei mit der Endung .s angelegt. Zusätzlich wird in der build.sh-Datei jeweils eine Passage hinzugefügt, um diese Datei zu assemblieren, sowie eine weitere Passage, um sie in den Linkprozess einzubinden. Erst bei den Floating-Point und SIMD Programmen gibt es in dieser Hinsicht etwas weiteres zu beachten!**

### Link.lds
Zuerst legen wir das Linkerscript `link.lds` an:

```
ENTRY(start)

MEMORY
{
    ram :   ORIGIN = 0x00008000, LENGTH = 0x8000000  /* 128 MB RAM */  
}

SECTIONS
{ 
    . = 0x8000;                   
    .text : AT(0x8000) { KEEP*(.text) } > ram 
    . = ALIGN(16);
    .data : { *(.data) } > ram
    .bss  : { *(.bss)  } > ram
}
```
### BOOT.s
Dann legen wir das `boot.s`-Sourcefile an:

-  `.global label`:
Wird verwendet, um ein Label (z. B. eine Funktion oder Variable) in der aktuellen Datei global sichtbar zu machen, sodass es auch in anderen Dateien genutzt werden kann. Es definiert ein Label und gibt seine Verfügbarkeit für externe Module frei.

- `.extern label`:
Wird verwendet, um ein Label anzugeben, das in einer anderen Datei definiert ist. Es zeigt an, dass das Label extern existiert und in der aktuellen Datei verwendet werden soll.

In diesem Fall werden start und KMain global bzw. extern deklariert:


```
.global start
.extern KMain
```
Folgende **Equs** sind Konstantendefinitionen, die den Code leserlicher und damit wartbarer machen: 
```
@ equs für Betriebsmodi
.equ   MODE_MASK,		0x1F
.equ   MODE_USR,		0x10
.equ   MODE_IRQ,		0x12
.equ   MODE_SVC,		0x13
.equ   STACK_IRQ,		0x7000
```

Der Code deaktiviert zunächst die Interrupts und setzt die Register zurück. Anschließend wird der aktive Core geprüft, um sicherzustellen, dass es sich um Core 0 handelt. Wenn dies nicht der Fall ist, wird in eine Dauerschleife (sleep) gewechselt.
```  
.section .text
start:
	@ interrupts ausmaskieren
	cpsid if 
	@ register initialisieren
	mov r0,  #0
	mov r1,  #0
	mov r2,  #0
	mov r3,  #0
	mov r4,  #0
	mov r5,  #0
	mov r6,  #0
	mov r7,  #0
	mov r8,  #0
	mov r9,  #0
	mov r10, #0
	mov r11, #0
	mov r12, #0
	
@ wenn der aktive core != core0 -> sleep
which_core:
	@ mpidr = multiprocessor affinity register enthaelt info bzgl corenr.
	mrc p15, #0, r0, c0, c0, #5
	and r0, r0, #3
	cmp r0, #0
	@ sleep
	bne		.
	
@ pruefe das privigierungslevel des aktiven cores
check_pl:
	mrs r0, cpsr
	mov r1, #MODE_MASK
	and r2, r0, r1
	cmp r2, #MODE_SVC
	bne sleep
	
	@ setze vector-adresse
	// to be implemented
	
	@ Wechsel in den Interruptmodus
	// to be implemented
	
	@ Stack für Interruptmodus aufsetzen
	// to be implemented

	@ Wechsle zurück in den Supervisor Modus
	// to be implemented
	
	@ enable Neon-Coprozessor
	// to be implemented
	
	@ Setze Stack-Basis-Adresse
	mov sp, #0x80000
	
	@ enable interrupts
	cpsie i
	
	@ set up textmode
	// to be implemented
	
kernel_entry:
	@ Reset Register
	mov r0,  #0
	mov r1,  #0
	mov r2,  #0
	mov r3,  #0
	mov r4,  #0
	mov r5,  #0
	mov r6,  #0
	mov r7,  #0
	mov r8,  #0
	mov r9,  #0
	mov r10, #0
	mov r11, #0
	mov r12, #0
	bl k_uart0_init
	bl  KMain
	b  .		@ wenn main verlassen wird -> hier Dauerschleife
							
sleep:
	b sleep						
```

### KMAIN.s
Die Datei kmain.s definiert die Hauptfunktion KMain:
```
.global KMain
.section .text
KMAIN: 
	b   .
```
Diese Funktion führt im Moment keine Aktionen aus und verweilt in einer Dauerschleife.

### build.sh
Im letzten Schritt werden die Kommandos für den Assembler und den Linker in einem Bash-Skript (build.sh) gespeichert, um sie nicht jedes Mal manuell in der Kommandozeile eingeben zu müssen. Das Skript kann leicht erweitert werden, falls Anpassungen erforderlich sind.

Die Dateien `boot.s` und `kmain.s` werden hiermit für den ARMv7-A Prozessor (Cortex-A7) assembliert und als `boot.o` bzw. `kmain.o` gespeichert:
```
arm-none-eabi-as -march=armv7-a -mfloat-abi=hard  -mcpu=cortex-a7 -c boots -g -o boot.o
arm-none-eabi-as -march=armv7-a -mfloat-abi=hard  -mcpu=cortex-a7 -c kmain.s -g -o kmain.o
```


Dieser Befehl linkt die Objektdateien mithilfe des Linker-Skripts link.lds zu einer ausführbaren Datei kernel7.elf:
```
arm-none-eabi-ld -g  boot.o kmain.o -T link.lds -o kernel7.elf 
```
### Emulation und Debugging
Um das Programm zu emulieren und zu debuggen, müssen zwei WSL-Konsolen (Windows Subsystem for Linux) gleichzeitig geöffnet und jeweils in den Projektordner navigiert werden.

#### Starten der Emulation mit QEMU
In der ersten Konsole wird QEMU mit folgendem Befehl gestartet:
```
qemu-system-arm -S -s -m 1024 -M raspi2b -monitor stdio -kernel kernel7.elf -smp 4,cores=1
```
Dieser Befehl startet eine Emulation des Raspberry Pi 2 mit 1 GB RAM und einem ARM-Prozessor. Die Option -S hält den Prozessor an, bis der Debugger verbunden ist, und -s öffnet den Standard-Debug-Port auf 1234. Die Option -kernel gibt die zu ladende Kernel-Datei (kernel7.elf) an. Der Parameter -smp 4,cores=1 setzt die Anzahl der CPU-Kerne auf einen.

#### Verbinden mit GDB für Remote-Debugging
In der zweiten Konsole wird GDB verwendet, um sich mit dem laufenden QEMU-Prozess zu verbinden. Hier die nötigen Schritte:
1. Starten von GDB:
```
gdb-multiarch
```
2. Die Zielarchitektur auf ARMv7 festlegen:
```
set architecture armv7
```
3. Das Kernel-Abbild mit Debuginformationen laden:
```
file kernel7.elf
```
4. Verbindung mit dem QEMU-Debugger herstellen:
```
target remote localhost:1234
```
#### GDB Benutzeroberfläche und Registeranzeige
Um die GDB-Benutzeroberfläche für den TUI-Modus (Text User Interface) zu aktivieren, wird folgender Befehl verwendet:
```
tui enable
```
Um die CPU-Register anzuzeigen, kann man das folgende Layout aktivieren:
```
layout regs
```
Mit dem Befehl `s` kann durch das Programm schrittweise (Befehl für Befehl) steppen. Für weiterführende GDB-Befehle wird empfohlen, die entsprechende [Seite](../debuggdb/debuggdb.md) im Tutorial erneut aufzusuchen.

|--------------------------------------|------------------------------------|--------------------------------|
|   [zurück](../privlmodi/mmupriv.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](../UART/uart.md)    |
