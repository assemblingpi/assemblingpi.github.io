## Der Framebuffer und seine Nutzung
Ein Framebuffer ist ein spezieller Speicherbereich, der sowohl von der CPU als auch von der GPU verwendet wird, um Bilddaten zu speichern, die auf einem Bildschirm angezeigt werden sollen. Der Framebuffer enthält also eine Bitmap, die eine Videoanzeige steuert. Man kann sich den Framebuffer wie eine Leinwand vorstellen, auf der das Bild „gemalt“ wird, welches dann vom Monitor angezeigt wird. 

### Was ist für unser Tutorial relevant?
In diesem Tutorial wird die GPU verwendet, um den Framebuffer zu erstellen und dessen Adresse im physikalischen Speicher zu ermitteln. Diese Adresse wird über den Mailbox-Mechanismus an die ARM-CPU übermittelt, wodurch die CPU Bilddaten direkt in den Framebuffer schreiben und die Anzeige auf dem Bildschirm steuern kann. 
Die GPU überprüft diese Adresse regelmäßig und aktualisiert die Pixel auf dem Bildschirm entsprechend.

|---------------------|-------------------------------|-------------------------------|
| [zurück](kommb.md)  | [Hauptmenü](../ueberblick.md) | [weiter](framemailb.md)       |