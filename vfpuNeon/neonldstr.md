# B.2 Erweiterungen der CPU-Funktionalität
## 2.3.14 VFP und NEON: NEON Load/Store Instruktionen

Die Befehle **VLD** (Vector Load) und **VST** (Vector Store) sind grundlegende Instruktionen für den Datenaustausch mit den NEON-Registern. Die spezifischen Varianten **VLDM** und **VSTM** stehen für "Vector Load Multiple" und "Vector Store Multiple" und werden verwendet, um mehrere Register gleichzeitig zu laden oder zu speichern.


### **VLDM (Vector Load Multiple)**
Mit dem VLDM-Befehl lassen sich mehrere Datenblöcke gleichzeitig aus dem Speicher in verschiedene NEON-Register laden. Statt die Register einzeln zu befüllen, können mehrere Register in einem Schritt geladen werden. So lädt zum Beispiel `VLDM R1, {D0-D3}` die NEON-Register D0 bis D3 mit Daten, die aus dem Speicherbereich stammen, dessen Startadresse im ARM-Register `R1` angegeben ist.

### **VSTM (Vector Store Multiple)**
Der VSTM-Befehl dient dazu, Daten aus mehreren NEON-Registern auf einmal in den Speicher zu schreiben. Statt die Inhalte einzelner Register nacheinander zu speichern, können mehrere Register auf einen Schlag gesichert werden. Beispielsweise speichert der Befehl `VSTM R1, {D0-D3}` die Daten aus den Registern D0 bis D3 in den Speicherbereich, der durch die im Register `R1` hinterlegte Adresse bestimmt wird.

### VLD und VST: Interleaving und De-Interleaving
Die NEON-Einheit ermöglicht es auch, Daten in einer spezifischen Anordnung zu laden und zu speichern. 

#### **VLD (Vector Load)**  
Der **VLD**-Befehl lädt Daten direkt aus dem Speicher in die NEON-Register. 

- **Syntax:** `vldX.size {D0-Dn},[Rs]`  
  - `X` legt das Interleave-Muster fest und definiert den Abstand der Elemente innerhalb der Struktur (1, 2, 3, 4).
  - `size` gibt die Größe der Daten an (8, 16, 32 oder 64 Bit).
  - `D0-Dn` spezifiziert die Register, in die die Daten geladen werden.
  - `[Rs]` stellt die Speicheradresse dar, von der die Daten abgerufen werden.

#### **VST (Vector Store)**  
Der **VST**-Befehl hingegen speichert die Daten aus den NEON-Registern wieder zurück in den Speicher. Nachdem die Daten in den Registern verarbeitet wurden, ermöglicht VST das Ablegen der Ergebnisse in einem bestimmten Speicherbereich.

- **Syntax:** `vstX.size {D0-Dn},[Rd]`  
  - `X` steht auch hier für das Interleave-Muster und bestimmt den Datenabstand (1, 2, 3, 4).
  - `size` spezifiziert die Größe der zu speichernden Daten (8, 16, 32 oder 64 Bit).
  - `D0-Dn` gibt die Register an, deren Inhalt gespeichert wird.
  - `[Rd]` bezeichnet die Speicheradresse, an die die Daten geschrieben werden.

### Funktionsweise von Interleaving und De-Interleaving
`Interleaving` und `De-Interleaving` bestimmen, wie Daten beim Laden und Speichern zwischen dem Speicher und den NEON-Registern angeordnet werden.

#### De-Interleaving beim Laden
Beim Laden von Daten aus dem Speicher in NEON-Register können die Daten entflechtet (de-interleaved) werden. Das bedeutet, dass Daten, die im Speicher "verschachtelt" abgelegt sind, beim Laden auf mehrere Register verteilt und sauber getrennt werden. Diese Technik wird häufig genutzt, um komplexe Datensätze effizient zu verarbeiten.

#### Interleaving beim Speichern
Umgekehrt können beim Speichern von Daten aus den NEON-Registern die Inhalte verschachtelt (interleaved) abgelegt werden. 
Beim Speichern von Daten aus den NEON-Registern können die Daten verschachtelt (interleaved) werden. Hierbei werden die Registerdaten in einem verschachtelten Muster kombiniert und im Speicher gespeichert, wie es in manchen Multimedia-Anwendungen üblich ist.

#### Interleave-Muster und Befehle
- **VLD1:** Lädt ein bis vier Register aus dem Speicher ohne De-Interleaving. Für nicht-verschachtelte Daten geeignet.
- **VLD2:** Lädt zwei oder vier Register und teilt die Daten nach geraden und ungeraden Elementen auf. Ideal für die Trennung von Stereo-Audiodaten in linke und rechte Kanäle.
- **VLD3:** Lädt Daten in drei Register und trennt diese. Nützlich für das Aufteilen von RGB-Pixeldaten in ihre Farbkanäle.
- **VLD4:** Lädt Daten in vier Register und teilt sie entsprechend auf, ideal für ARGB-Bilddaten.

Bei den Store-Befehlen (VST) hängt die Anzahl der benötigten Register ebenfalls von der Art des Interleaving ab:

- **VST1:** Speichert Daten linear im Speicher ohne Verschachtelung. Mehrere Register werden nacheinander abgelegt.
- **VST2:** Führt ein 2-faches Interleaving durch, bei dem zwei Register abwechselnd in den Speicher geschrieben werden.
- **VST3:** Speichert Daten mit einem 3-fachen Interleaving, benötigt drei Register.
- **VST4:** Speichert Daten mit einem 4-fachen Interleaving, entsprechend werden vier Register benötigt.

#### Beispiel 

In einem Assemblercode wird eine 128-Bit-Variable namens `mydata` definiert:

```asm
mydata: .quad 0x0102030405060708, 0x090a0b0c0d0e0f10
```

Da ARM-Prozessoren normalerweise im Little-Endian-Format arbeiten, wird die Reihenfolge der Bytes im Speicher umgedreht. Das bedeutet, dass die Daten wie folgt im Speicher abgelegt werden:

- `0x08 0x07 0x06 0x05 0x04 0x03 0x02 0x01` (für das erste Quadword)
- `0x10 0x0f 0x0e 0x0d 0x0c 0x0b 0x0a 0x09` (für das zweite Quadword)

#### Beispiel 1: Einfaches Laden ohne De-Interleaving

```asm
vld1.8 {d0}, [r0]
```

dann wird das Register `d0` direkt mit den ersten 8 Bytes aus dem Speicher gefüllt. Das Register `d0` enthält danach:

|d0[7] |d0[6] |d0[5] |d0[4] |d0[3] |d0[2] |d0[1] |d0[0] |
|------|------|------|------|------|------|------|------|
| 0x08 | 0x07 | 0x06 | 0x05 | 0x04 | 0x03 | 0x02 | 0x01 |

Das bedeutet, die Bytes werden in der Reihenfolge, wie sie im Speicher liegen, nacheinander in das Register geladen.

#### Beispiel 2: Laden mit De-Interleaving

```asm
vld2.8 {d0, d1}, [r0]
```

dann passiert Folgendes:

- Das erste Byte (`0x08`) wird in `d0[7]` geladen.
- Das zweite Byte (`0x07`) wird in `d1[7]` geladen.
- Das dritte Byte (`0x06`) wird in `d0[6]` geladen.
- Das vierte Byte (`0x05`) wird in `d1[6]` geladen.
- Und so weiter.

Das Ergebnis sieht dann so aus:

**Register `d0`:**

|d0[7] |d0[6] |d0[5] |d0[4] |d0[3] |d0[2] |d0[1] |d0[0] |
|------|------|------|------|------|------|------|------|
| 0x08 | 0x06 | 0x04 | 0x02 | 0x10 | 0x0e | 0x0c | 0x0a |

**Register `d1`:**

|d1[7] |d1[6] |d1[5] |d1[4] |d1[3] |d1[2] |d1[1] |d1[0] |
|------|------|------|------|------|------|------|------|
| 0x07 | 0x05 | 0x03 | 0x01 | 0x0f | 0x0d | 0x0b | 0x09 |

### Erklärung des De-Interleaving

Beim De-Interleaving werden die Daten nicht einfach nacheinander in ein einzelnes Register geladen. Stattdessen werden die Daten auf mehrere Register verteilt:

- **VLD2:** Lädt abwechselnd Daten in zwei Register.
- **VLD3:** Würde die Daten in drei Register verteilen (z. B. `d0`, `d1`, `d2`).
- **VLD4:** Verteilt die Daten auf vier Register.

Im obigen Beispiel hat das De-Interleaving also zur Folge, dass die Daten von `mydata` abwechselnd in `d0` und `d1` geladen werden. Das erste Byte geht in `d0`, das zweite in `d1`, das dritte wieder in `d0` usw. So werden die Daten effektiv verteilt, was bei bestimmten Berechnungen oder Datenverarbeitungen nützlich sein kann.


### Interleaving und einfaches Speichern von Daten mit NEON

#### Beispiel
Die Vektorregister `D0` und `D1` sind mit folgenden Werten initialisiert:

- **D0:** `0x08 0x07 0x06 0x05 0x04 0x03 0x02 0x01` 
- **D1:** `0x10 0x0f 0x0e 0x0d 0x0c 0x0b 0x0a 0x09` 

### Beispiel 1: Einfaches Speichern ohne Interleaving
Wenn wir die Daten aus diesen Registern ohne Interleaving in den Speicher schreiben wollen, verwenden wir den folgenden Befehl:

```asm
vst1.8 {d0, d1}, [r0]
```
Dies speichert die Daten nacheinander im Speicher:

- Zuerst werden alle 8 Bytes aus `d0` im Speicher abgelegt.
- Danach folgen alle 8 Bytes aus `d1`.

Im Speicher sehen die Daten dann so aus:

- `0x08 0x07 0x06 0x05 0x04 0x03 0x02 0x01` (Daten aus `d0`)
- `0x10 0x0f 0x0e 0x0d 0x0c 0x0b 0x0a 0x09` (Daten aus `d1`)

### Beispiel 2: Speichern mit Interleaving

Beim Interleaving werden die Daten aus mehreren Registern abwechselnd in den Speicher geschrieben. Wenn wir die Daten aus den Registern `d0` und `d1` interleaved speichern möchten, verwenden wir:

```asm
vst2.8 {d0, d1}, [r0]
```

In diesem Fall passiert Folgendes:

- Das erste Byte von `d0[7]` wird in den Speicher geschrieben.
- Dann wird das erste Byte von `d1[7]` geschrieben.
- Danach folgen `d0[6]` und `d1[6]`, und so weiter.

Im Speicher sieht das Ergebnis dann so aus:

| Position  | 0  | 1  | 2  | 3  | 4  | 5  | 6  | 7  | 8  | 9  | 10 | 11 | 12 | 13 | 14 | 15 |
|-----------|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
| Wert      | 08 | 10 | 07 | 0f | 06 | 0e | 05 | 0d | 04 | 0c | 03 | 0b | 02 | 0a | 01 | 09 |

### Erklärung des Interleaving

Beim Interleaving werden die Daten nicht einfach nacheinander in den Speicher geschrieben. Stattdessen werden die Bytes aus mehreren Registern abwechselnd (interleaved) gespeichert:

- **VST2:** Speichert abwechselnd Daten aus zwei Registern.
- **VST3:** Würde die Daten abwechselnd aus drei Registern speichern (z. B. `d0`, `d1`, `d2`).
- **VST4:** Speichert die Daten abwechselnd aus vier Registern.

|--------------------|-------------------------------|-----------------------------|
| [zurück](vmov.md)  | [Hauptmenü](../ueberblick.md) | [weiter](varithlog.md)      |


|**2.3 VFP und NEON**                                                                                               |
|-------------------------------------------------------------------------------------------------------------------|
| [2.3.1 Intro](floatingintro.md)                                                                                   |
| [2.3.2 Gleitkommazahlen](bingleit.md)                                                                             |
| [2.3.3 Floating Point Format nach IEEE 754](floatingnums.md)                                                      |
| [2.3.4 VFP (Vector Floating Point) in der ARM-Architektur](vfp_intro.md)                                          |
| [2.3.5 VFP Data Conversion Befehle](vfpconv.md)                                                                   |
| [2.3.6 Was ist NEON?](neonintro.md)                                                                               |
| [2.3.7 Überblick über die ARMv7 NEON-Register](neonregs.md)                                                       |
| [2.3.8 Vektoren und Skalare](scalvekt.md)                                                                         |
| [2.3.9 Registeradressierung in NEON](neonadr.md)                                                                  |
| [2.3.10 Das NEON und Floatingpoint Status Register](neonstat.md)                                                  |
| [2.3.11 Steuerung und Statusübertragung zwischen ARM- und NEON/VFP-Statusregistern (VMSR und VMRS)](neonctrl.md)  |
| [2.3.12 NEON Instruktionen](neoninstr.md)                                                                         |
| [2.3.13 Datentransfer](vmov.md)                                                                                   |
| [2.3.14 NEON Load/Store Instruktionen](neonldstr.md)                                                              |
| [2.3.15 Arithmetische und logische NEON-Operationen](varithlog.md)                                                |
| [2.3.16 VTRN (Vector Transpose) Instruktionen](vtrn.md)                                                           |
| [2.3.17 Implementierung von Trigonometrischen Funktionen](trigon_ue.md)                                           |
| [2.3.18 Implementierung einer 4x4-Matrixmultiplikationsfunktion mit NEON](matrix_ue.md)                           |