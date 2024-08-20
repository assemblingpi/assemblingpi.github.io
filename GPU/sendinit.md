## Senden der Initialisierungsnachricht
Um die Mail für den Versand über die Mailbox vorzubereiten, müssen drei Punkte beachtet werden:
```
1. Es muss sichergestellt werden, dass die Initialisierungsstruktur 16-Byte-aligned
   ist, sodass die Adresse nur die oberen 28 Bits belegt. Schließlich liegen in den 
   restlichen 4 Bits die Infos bezüglich des Kanals!
2. Die Adresse der Initialisierungsstruktur muss angepasst werden, bevor die Mail an 
   die GPU gesandt wird, da diese den Adressraum mit anderen Adressen sieht 
   als die CPU.
3. Dieser Adresswert wird dann bitweise mit 1 verknüpft, sodass Kanal 1 (Framebuffer)
   die unteren 4 Bits belegt.
```
**Nachdem diese Schritte abgeschlossen sind, kann dieser Wert als Mail über die Mailbox an die GPU gesendet werden. Die GPU gibt bei Erfolg den Wert 0 zurück, anderenfalls einen Wert verschieden von 0.**

### Warum die übergebene Adresse angepasst werden muss:

[Der Wert 0xC0000000 wird an dieser Stelle verwendet, um sicherzustellen, dass die GPU die Speicheradresse korrekt interpretiert, wenn der L2-Cache deaktiviert ist. 
In diesem Fall sieht die GPU den physischen Speicher nämlich ab der Adresse 0xC0000000. Diese Anpassung ist notwendig, damit die Speicheradresse, die von der CPU an die GPU gesendet wird, korrekt übersetzt wird, sodass die GPU den richtigen Speicherbereich ansteuern kann.](https://github-wiki-see.page/m/raspberrypi/firmware/wiki/Accessing-mailboxes)

