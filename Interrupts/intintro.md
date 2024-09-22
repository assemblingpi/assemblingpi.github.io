## Was sind Interrupts?

Interrupts sind Signale, die den Prozessor darauf hinweisen, dass ein bestimmtes Ereignis eingetreten ist, das sofortige Aufmerksamkeit erfordert.
Dieser Mechanismus ermöglicht, auf externe oder interne Ereignisse zu reagieren, ohne dass der Prozessor ständig aktiv prüfen muss, ob neue Ereignisse aufgetreten sind. 
Wenn der Prozessor jedoch aktiv prüfen muss, ob ein bestimmtes Event (z. B. eine Datenübertragung oder ein Signal) aufgetreten ist, spricht man von Polling. Polling kostet die CPU wertvolle Zeit, da sie aktiv immer und immer wieder abfrägt, ob ein Signal aufgetreten ist.
Bei Interupts hingegen unterbricht der Prozessor nach Auftreten eines Interrupt-Events automatisch seine gegenwärtige Tätigkeit, um eine spezielle Routine auszuführen, die als Interrupt-Service-Routine (ISR) oder Interrupt Handler bezeichnet wird. 

|--------------------------------------|------------------------------------|-----------------------------|
|   [zurück](../privlmodi/mmupriv.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](ivektable.md)    |
