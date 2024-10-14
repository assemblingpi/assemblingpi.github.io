# B.1 Einführung
## 1.6.3 Priviligierungslevel: Betriebsmodi

- **User-Modus (usr):** Unprivilegierter Modus, geeignet für die meisten Anwendungsprogramme.
- **FIQ-Modus (fiq):** Privilegierter Modus, der bei einem schnellen Interrupt betreten wird.
- **IRQ-Modus (irq):** Privilegierter Modus, der bei einem normalen Interrupt aktiviert wird.
- **Supervisor-Modus (svc):** Privilegierter Modus, der für die meisten Kernel-Funktionen verwendet wird. Dieser Modus wird beim Zurücksetzen des Systems oder bei der Ausführung einer Supervisor Call (SVC) Instruktion betreten.
- **Monitor-Modus (mon):** Ein sicherer Modus, der den Wechsel zwischen sicheren und nicht sicheren Zuständen ermöglicht und auch für
die Bearbeitung von FIQs, IRQs und externen Abbrüchen verwendet werden kann. Dieser Modus wird durch die Ausführung einer Secure Monitor Call (SMC) Instruktion betreten.
- **Abort-Modus (abt):** Privilegierter Modus, der als Folge einer Daten- oder Prefetch-Abbruch-Ausnahme betreten wird
- **Undefined-Modus (und):** Privilegierter Modus, der bei einem fehlerhaften Befehl betreten wird.
- **Hypervisor-Modus (hyp):** Privilegierter Modus zur Verwaltung von Virtualisierung, in dem der Hypervisor mehrere virtuelle Maschinen steuert.
- **System-Modus (sys):** Privilegierter Modus, der für Anwendungsprogramme geeignet ist, die privilegierten Zugriff benötigen.

|-------------------------|------------------------------------|---------------------------|
|   [zurück](privlev.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](cpsrmod.md)    |