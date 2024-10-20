# B.2 Erweiterungen der CPU-Funktionalität
## 2.4.6 Grafik & GPU: Der Framebuffer-Mailbox-Kanal

### Überblick über den Framebuffer-Kanal
Der Framebuffer-Kanal ist der Mailbox-Kanal 1 und stellt eine von mehreren Methoden dar, um von der GPU ein Framebuffer anzufordern.

### Die Framebuffer-Initialisierungsnachricht
Die einzige Nachricht, die über diesen Kanal gesendet wird, ist ein Zeiger auf eine Initialisierungsstruktur, während die empfangene Nachricht nur den Status des Erfolgs oder Fehlers zurückgibt. Die Struct sieht folgendermaßen aus:

```
+----+----+----+----+----+----+----+
| 31 |           ...          |  0 |
+----+----+----+----+----+----+----+
|          Physische Breite        |  <--- FrameBufferInfo
+----+----+----+----+----+----+---_+
|           Physische Höhe         |
+----+----+----+----+----+----+----+
|         Virtuelle Breite         |
+----+----+----+----+----+----+----+
|          Virtuelle Höhe          |
+----+----+----+----+----+----+----+
|             GPU_PITCH            |
+----+----+----+----+----+----+----+
|             Farbtiefe            |
+----+----+----+----+----+----+----+
|             X-Offset             |
+----+----+----+----+----+----+----+
|             Y-Offset             |
+----+----+----+----+----+----+----+
|            GPU - Zeiger          |
+----+----+----+----+----+----+----+
|            GPU - Größe           |
+----+----+----+----+----+----+----+
```

Das ist das Format der Datenstruktur, die wir an den Grafikprozessor übermitteln wollen. Diese Struct, die wir FrameBufferInfo nennen - wird verwendet, um die gewünschte Breite, Höhe, virtuelle Breite, virtuelle Höhe und Farbtiefe zu definieren. Die Werte für die Zeilenbreite (Pitch), den GPU-Zeiger und die Größe des Framebuffers werden von der GPU automatisch ausgefüllt. Daher sollten diese Werte zurückgesetzt werden, wenn ein neuer Framebuffer erstellt wird. Wenn die Anfrage an die GPU erfolgreich ist, enthält der GPU-Zeiger-Eintrag den Zeiger auf den Framebuffer.

|-------------------------|-------------------------------|-----------------------------|
| [zurück](framebuff.md)  | [Hauptmenü](../ueberblick.md) | [weiter](sendinit.md)       |


|**2.4 Grafik & GPU**                                                       |
|---------------------------------------------------------------------------|
| [2.4.1 Einführung in die Computergrafik & RGB565-Format](grafikintro.md)  |
| [2.4.2 Was ist eine GPU?](gpuintro.md)                                    |
| [2.4.3 Die GPU des BCM2836 VideoCoreIV](gpubcm2836.md)                    |
| [2.4.4 Kommunikation mittels Mailbox](kommb.md)                           |
| [2.4.5 Der Framebuffer und seine Nutzung](framebuff.md)                   |
| [2.4.6 Der Framebuffer-Mailbox-Kanal](framemailb.md)                      |
| [2.4.7 Senden der Initialisierungsnachricht](sendinit.md)                 |