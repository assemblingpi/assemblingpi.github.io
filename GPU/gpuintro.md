# B.2 Erweiterungen der CPU-Funktionalität
## 2.4.2 Grafik & GPU: Was ist eine GPU?

Eine Grafikprozessor-Einheit (GPU) ist ein spezialisierter Prozessor, der für die Verarbeitung und Darstellung von Grafiken und Bilddaten auf Computern und anderen Geräten entwickelt wurde. Sie spielt eine entscheidende Rolle in modernen Computern, Konsolen, Mobilgeräten und eingebetteten Systemen, indem sie komplexe grafische Operationen effizient ausführt.

## Funktion und Bedeutung

**Grafik-Rendering**: Die primäre Aufgabe einer GPU besteht darin, grafische Daten zu rendern. Dies umfasst die Berechnung und Darstellung von 2D- und 3D-Grafiken. Zum Beispiel werden die Bilddaten für Spiele, Benutzeroberflächen oder Videos durch die GPU verarbeitet und auf einem Bildschirm angezeigt.

**Parallele Verarbeitung**: Im Gegensatz zur zentralen Verarbeitungseinheit (CPU), die für allgemeine Berechnungen und die Ausführung von Programmen zuständig ist, ist die GPU für parallele Verarbeitung optimiert. Sie kann Tausende von Threads gleichzeitig ausführen, was sie ideal für grafikintensive Aufgaben wie die Bearbeitung von Texturen oder das Durchführen von Berechnungen in Echtzeit macht.

**Spezielles Hardware-Design**: GPUs sind mit spezieller Hardware ausgestattet, um die parallele Verarbeitung von Daten zu beschleunigen. Dies umfasst Millionen von kleinen Recheneinheiten (Shader-Kernen), die gleichzeitig an verschiedenen Teilen eines Bildes arbeiten können. Dies ermöglicht eine schnelle und effiziente Verarbeitung komplexer grafischer Inhalte.

## Anwendungsbereiche
GPUs sind vielseitige Komponenten, deren Einsatzbereiche weit über die herkömmliche Berechnung von Grafik- und Physikeffekten in modernen Computerspielen hinausgehen. Neben der Hauptverantwortung für die Echtzeit-Berechnung von detaillierten und flüssigen 3D-Darstellungen, die für ein immersives Spielerlebnis unerlässlich sind, finden GPUs auch Anwendung in der wissenschaftlichen Berechnung und Datenanalyse. Hierbei nutzen sie ihre Fähigkeit zur parallelen Verarbeitung, um große Datenmengen effizient zu bewältigen und komplexe Berechnungen zügig durchzuführen. Neben diesen prominenten Einsatzgebieten gibt es noch viele weitere Anwendungsbereiche, die von der Flexibilität und Leistungsfähigkeit von GPUs profitieren.

|--------------------------------------|-------------------------------|------------------------------|
| [zurück](grafikintro.md)             | [Hauptmenü](../ueberblick.md) | [weiter](gpubcm2836.md)      |


|**2.4 Grafik & GPU**                                                       |
|---------------------------------------------------------------------------|
| [2.4.1 Einführung in die Computergrafik & RGB565-Format](grafikintro.md)  |
| [2.4.2 Was ist eine GPU?](gpuintro.md)                                    |
| [2.4.3 Die GPU des BCM2836 VideoCoreIV](gpubcm2836.md)                    |
| [2.4.4 Kommunikation mittels Mailbox](kommb.md)                           |
| [2.4.5 Der Framebuffer und seine Nutzung](framebuff.md)                   |
| [2.4.6 Der Framebuffer-Mailbox-Kanal](framemailb.md)                      |
| [2.4.7 Senden der Initialisierungsnachricht](sendinit.md)                 |