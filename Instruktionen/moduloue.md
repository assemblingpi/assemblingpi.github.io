## Übungsaufgabe: Modulo berechnen

Der Modulo-Operator gibt den Rest einer ganzzahligen Division zurück. Er berechnet, wie viel nach der maximalen Anzahl vollständiger Teilungen übrig bleibt. In ARM-Assembler gibt es keinen direkten Befehl für Modulo, aber es kann mit ganzzahliger Division und Multiplikation berechnet werden.

Gegeben ist der folgende Codeabschnitt:
```asm
.data

number_a: .word 17
number_b: .word 5

.text

.global _start
_start:
        
        ldr r0, =number_a
        ldr r0, [r0]            @ Lade number_a in r0
        ldr r1, =number_b
        ldr r1, [r1]            @ Lade number_b in r1

        // Code hier

```

### Aufgabenstellung:
1. Ergänze den Code so, dass `resultat = a mod b` berechnet wird.
2. Speichere das Ergebnis der Modulo-Berechnung in r0 und überprüfe es für verschiedene Werte auf Korrektheit.

### Hinweis:
Um den Modulo-Operator 𝑎 mod 𝑏 zu berechnen, wenn nur Integer-Multiplikation und Integer-Division zur Verfügung stehen, bietet sich folgende Methode an:
```
𝑞   = 𝑎 / 𝑏
𝑝   = 𝑞 × 𝑏
mod = a − p
```
