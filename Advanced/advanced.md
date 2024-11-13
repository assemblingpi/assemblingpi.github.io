# B.1 Einführung
## 1.1 Kapitelübersicht

In diesem Abschnitt des Tutorials werden anwendungsbezogenere Themen behandelt. Anstelle des CPUlators wird der QEMU-Emulator verwendet, der die Emulation eines Raspberry Pi 2b ermöglicht. Dadurch wird es möglich, nahezu „bare-metal“ zu programmieren, ohne dass eine physische Raspberry Pi-Hardware erforderlich ist. „Bare-metal“ bedeutet in diesem Zusammenhang, dass Programme direkt auf der Hardware ohne Betriebssystem ausgeführt werden.

Für den kommenden Teil werden folgende Softwaretools benötigt:
- **WSL (Windows Subsystem for Linux)**
- **ARM-Assembler und Linker**
- **QEMU für die Emulation**
- **GDB zum Debuggen**

### Installation von WSL

Die Installation von WSL erfolgt nach den Anweisungen auf der [offiziellen Microsoft-Webseite](https://learn.microsoft.com/de-de/windows/wsl/install). Nach erfolgreicher Installation kann mit der Einrichtung der weiteren Werkzeuge fortgefahren werden.

### Installation der ARM Cross-Toolchain

Der benötigte ARM-Assembler ist Teil der GNU Arm Embedded Toolchain. Diese kann in WSL mit dem folgenden Befehl installiert werden:

```
sudo apt install binutils-arm-none-eabi gcc-arm-none-eabi
```

#### Überprüfung der Installation

Nach der Installation lässt sich die korrekte Einrichtung des Assemblers überprüfen, indem die Versionsnummer abgefragt wird:

```
arm-none-eabi-as --version
```

Die Ausgabe sollte in etwa wie folgt aussehen:

```
GNU assembler (2.38-3ubuntu1+15build1) 2.38
Copyright (C) 2022 Free Software Foundation, Inc.
This program is free software; you may redistribute it under the terms of
the GNU General Public License version 3 or later.
This program has absolutely no warranty.
This assembler was configured for a target of `arm-none-eabi'.
```

### Installation von QEMU

QEMU und die benötigten Zusatzpakete können ebenfalls über die Paketverwaltung installiert werden:

```
sudo apt install qemu qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils virt-manager
```

#### Überprüfung der Installation

Die korrekte Installation von QEMU lässt sich überprüfen, indem die Version des Emulators abgefragt wird:

```
qemu-system-x86_64 --version
```

Die Ausgabe sollte in etwa folgendermaßen aussehen:

```
QEMU emulator version 6.2.0 (Debian 1:6.2+dfsg-2ubuntu6.22)
Copyright (c) 2003-2021 Fabrice Bellard and the QEMU Project developers
```

### Installation von GDB-Multiarch

Das **gdb-multiarch**-Paket ist eine spezielle Version des GNU Debuggers, die mehrere Architekturen unterstützt, einschließlich ARM. Dies ermöglicht das Debuggen von Programmen, die für verschiedene Zielarchitekturen kompiliert wurden.

Um das Paket zu installieren, verwenden Sie folgenden Befehl:

```
sudo apt install gdb-multiarch
```
Die Ausgabe sollte in etwa folgendermaßen aussehen:

```
GNU gdb (Ubuntu 12.1-0ubuntu1~22.04.2) 12.1
Copyright (C) 2022 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
```
Mit diesen Schritten sind alle benötigten Werkzeuge installiert und das System bereit, um mit dem Tutorial fortzufahren.

|-------------------------------|------------------------------------|-----------------------------------------|
|   [zurück](../Proz/param.md)  |   [Hauptmenü](../ueberblick.md)    |   [weiter](../Systemprog/sysprog.md)    |