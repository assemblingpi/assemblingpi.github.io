## UDIV und SDIV

Die Befehle UDIV und SDIV werden verwendet, um Divisionen durchzuführen, wobei UDIV für vorzeichenlose Division und SDIV für vorzeichenbehaftete Division verwendet wird.
**Achtung: In diesen beiden Instruktionen dürfen keine unmittelbaren Werte verwendet werden!**

### Syntax
```
UDIV <Zielregister>, <Dividendregister>, <Divisorregister>

SDIV <Zielregister>, <Dividendregister>, <Divisorregister>
```

- `UDIV (Unsigned Divide)`: Führt eine Division von zwei vorzeichenlosen (unsigned) 32-Bit-Werten durch. Das Ergebnis wird im `<Zielregister>` gespeichert.
- `SDIV (Signed Divide)`: Führt eine Division von zwei vorzeichenbehafteten (signed) 32-Bit-Werten durch. Das Ergebnis wird im `<Zielregister>` gespeichert.

**Hinweis:** Diese Instruktionen werden nicht von allen Prozessoren der ARMv7-Familie unterstützt und sind auch im CPUlator nicht ausführbar. Da jedoch in einem späteren Abschnitt des Tutorials auf einen Emulator umgestiegen wird, der die ARMv7-A-Variante unterstützt, wird an dieser Stelle bereits auf diese Befehle hingewiesen, da sie äußerst nützlich sind.

[weiter](arithlogintro.md)