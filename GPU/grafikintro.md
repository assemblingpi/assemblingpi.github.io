# B.2 Erweiterungen der CPU-Funktionalität
## 2.4.1 Grafik & GPU: Einführung in die Computergrafik & RGB565-Format

Computergrafik ermöglicht es uns, Bilder, Animationen und komplexe visuelle Darstellungen in digitalen Medien zu erzeugen. Sie spielt eine zentrale Rolle in den Bereichen Unterhaltung, Design und Technik. Doch um ihre Mechanismen zu verstehen, müssen wir uns grundlegende Konzepte wie Pixel, Farbdarstellung und die Rolle der Hardware näher ansehen. In diesem Artikel betrachten wir zunächst diese Grundlagen und gehen anschließend auf das RGB565-Format ein, ein effizienteres Farbformat, das in vielen ressourcenbegrenzten Systemen eingesetzt wird.

### 1. Grundbegriffe der Computergrafik

Um den Prozess der Bilddarstellung zu verstehen, müssen wir uns mit einigen Kernkomponenten der Computergrafik auseinandersetzen:

- **Pixel:** Jedes digitale Bild besteht aus winzigen Punkten, den sogenannten Pixeln. Jeder Pixel hat eine bestimmte Farbe und trägt zur Gesamtbildkomposition bei. Die Bildauflösung beschreibt die Anzahl der Pixel in einem Bild (Breite x Höhe).
  
- **Framebuffer:** Der Framebuffer ist ein Speicherbereich, der die Farbinformationen aller Pixel eines Bildes enthält. Sobald diese Informationen in der GPU verarbeitet sind, werden die Bilder auf dem Bildschirm dargestellt.

- **GPU (Graphics Processing Unit):** Die GPU ist eine spezialisierte Recheneinheit zur Beschleunigung der Bildberechnung und -darstellung. Sie kann Millionen von Pixeln in Echtzeit verarbeiten, was besonders für flüssige Animationen und komplexe grafische Szenen entscheidend ist.

### 2. Farbmodell RGB: Grundlagen und Varianten

Farbdarstellung in der Computergrafik basiert typischerweise auf dem RGB-Modell (Rot, Grün, Blau). Die drei Grundfarben werden in verschiedenen Intensitäten gemischt, um eine Vielzahl von Farbtönen zu erzeugen. Unterschiedliche Formate innerhalb dieses Systems haben Auswirkungen auf die Farbtiefe und somit auf die Qualität und Effizienz der Darstellung.

#### RGB8 (24-Bit RGB)

Das RGB8-Format verwendet 8 Bits pro Farbkanal, was insgesamt 24 Bits pro Pixel ergibt. Dadurch können 16,7 Millionen verschiedene Farben dargestellt werden. RGB8 wird in Bereichen verwendet, die eine hohe Farbgenauigkeit erfordern, etwa in der Bildbearbeitung oder bei modernen Spielen.

#### RGB565 (16-Bit RGB)

RGB565 verwendet 16 Bits pro Pixel, was zu einer geringeren Farbtiefe im Vergleich zu RGB8 führt. Die Aufteilung der Bits ist asymmetrisch: 5 Bits für Rot, 6 Bits für Grün und 5 Bits für Blau. Diese Verteilung ist darauf ausgelegt, das menschliche Auge zu berücksichtigen, das Grüntöne besser unterscheiden kann als Rot- oder Blautöne. RGB565 kann insgesamt 65.536 Farben darstellen.

### 3. Warum RGB565? Speicher- und Leistungsoptimierung

RGB565 ist ein effizientes Farbformat, das mit 16 Bits pro Pixel weniger Speicher benötigt als das 24-Bit-RGB-Format. Es wird vor allem in Systemen mit begrenztem Speicher und Rechenleistung, wie mobilen Geräten und älteren Displays, eingesetzt. Ein Bild mit 320 x 240 Pixeln verbraucht im RGB565-Format nur 153.600 Bytes. Der Vorteil liegt im geringen Speicherverbrauch und der schnelleren Verarbeitung, während die reduzierte Farbtiefe die Darstellung von Farbnuancen einschränkt. Daher ist es weniger geeignet für moderne, hochauflösende Anwendungen, aber ideal für ressourcenarme Systeme.

### Tool zur Erkundung von RGB565 Farbcodes

Da wir im weiteren Verlauf des Tutorials das RGB565-Format verwenden werden, ist es sinnvoll, sich damit vertraut zu machen. Zur Auswahl von RGB-Farbwerten und zur Anzeige der entsprechenden 16-Bit-Codes können Sie das Tool auf [rgbcolorpicker.com/565](https://rgbcolorpicker.com/565) nutzen.


|--------------------------------------|-------------------------------|------------------------------|
| [zurück](../vfpuNeon/matrix_lsg.md)  | [Hauptmenü](../ueberblick.md) | [weiter](gpuintro.md)        |


|**2.4 Grafik & GPU**                                                       |
|---------------------------------------------------------------------------|
| [2.4.1 Einführung in die Computergrafik & RGB565-Format](grafikintro.md)  |
| [2.4.2 Was ist eine GPU?](gpuintro.md)                                    |
| [2.4.3 Die GPU des BCM2836 VideoCoreIV](gpubcm2836.md)                    |
| [2.4.4 Kommunikation mittels Mailbox](kommb.md)                           |
| [2.4.5 Der Framebuffer und seine Nutzung](framebuff.md)                   |
| [2.4.6 Der Framebuffer-Mailbox-Kanal](framemailb.md)                      |
| [2.4.7 Senden der Initialisierungsnachricht](sendinit.md)                 |