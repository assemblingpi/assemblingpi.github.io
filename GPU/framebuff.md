# B.2 Erweiterungen der CPU-Funktionalität
## 2.4.5 Grafik & GPU: Der Framebuffer und seine Nutzung

Ein Framebuffer ist ein spezieller Speicherbereich, der sowohl von der CPU als auch von der GPU verwendet wird, um Bilddaten zu speichern, die auf einem Bildschirm angezeigt werden sollen. Der Framebuffer enthält also eine Bitmap, die eine Videoanzeige steuert. Man kann sich den Framebuffer wie eine Leinwand vorstellen, auf der das Bild „gemalt“ wird, welches dann vom Monitor angezeigt wird. 

### Was ist für unser Tutorial relevant?
In diesem Tutorial wird die GPU verwendet, um den Framebuffer zu erstellen und dessen Adresse im physikalischen Speicher zu ermitteln. Diese Adresse wird über den Mailbox-Mechanismus an die ARM-CPU übermittelt, wodurch die CPU Bilddaten direkt in den Framebuffer schreiben und die Anzeige auf dem Bildschirm steuern kann. 
Die GPU überprüft diese Adresse regelmäßig und aktualisiert die Pixel auf dem Bildschirm entsprechend.

|---------------------|-------------------------------|-------------------------------|
| [zurück](kommb.md)  | [Hauptmenü](../ueberblick.md) | [weiter](framemailb.md)       |


|**2.4 Grafik & GPU**                                                       |
|---------------------------------------------------------------------------|
| [2.4.1 Einführung in die Computergrafik & RGB565-Format](grafikintro.md)  |
| [2.4.2 Was ist eine GPU?](gpuintro.md)                                    |
| [2.4.3 Die GPU des BCM2836 VideoCoreIV](gpubcm2836.md)                    |
| [2.4.4 Kommunikation mittels Mailbox](kommb.md)                           |
| [2.4.5 Der Framebuffer und seine Nutzung](framebuff.md)                   |
| [2.4.6 Der Framebuffer-Mailbox-Kanal](framemailb.md)                      |
| [2.4.7 Senden der Initialisierungsnachricht](sendinit.md)                 |