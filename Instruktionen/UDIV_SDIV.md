## UDIV und SDIV

Die Befehle UDIV und SDIV werden verwendet, um Divisionen durchzuführen, wobei UDIV für vorzeichenlose Division und SDIV für vorzeichenbehaftete Division verwendet wird.
Achtung: Diese beiden Instruktionen werden keine unmittelbaren Werte verwendet!

### Syntax
```
UDIV <Zielregister>, <Dividendregister>, <Divisorregister>

SDIV <Zielregister>, <Dividendregister>, <Divisorregister>
```

- `UDIV (Unsigned Divide)`: Führt eine Division von zwei vorzeichenlosen (unsigned) 32-Bit-Werten durch. Das Ergebnis wird im `<Zielregister>` gespeichert.
- `SDIV (Signed Divide)`: Führt eine Division von zwei vorzeichenbehafteten (signed) 32-Bit-Werten durch. Das Ergebnis wird im `<Zielregister>` gespeichert.

[weiter](arithlogintro.md)