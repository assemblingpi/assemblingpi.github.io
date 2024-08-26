# VMOV: Move beteween arm register and neon register
Der Befehl VMOV in der ARM-Architektur steht für **Vector Move**. Er wird verwendet, um Daten zwischen verschiedenen Registern zu verschieben, insbesondere zwischen ARM- und NEON-/VFP-Registern. Dabei können verschiedene Datentypen und Registergrößen involviert sein, aber es findet keine Datenkonvertierung statt – die Bits werden einfach kopiert.

# VMOV: 32-Bit Datentransfer 
Dieser Befehl verschiebt Daten zwischen einem 32-Bit-ARM-Register und einem 32-Bit-NEON-Register.

**Syntax:**
```
vmov {<cond>} Rd, Sn  @ move Sn to Rd
vmov {<cond>} Sn, Rd  @ move Rd to Sn
```
**cond:** Bedingte Ausführung (optional)
**Rd:** ein ARM-Integer Register
**Sd:** eines der 32-Bit Single Precision Register

# VMOV: 64-Bit Datentransfer 
Dieser Befehl ermöglicht den Transfer von 64-Bit-Daten zwischen zwei 32-Bit-ARM-Registern und einem 64-Bit-NEON-Register.

```asm
vmov {<cond>} Dd, Rl, Rh  @ move Rh and Rl to Dd
vmov {<cond>} Rl, Rh, Dd  @ move Dd to Rl and Rh
```
**cond:** Bedingte Ausführung (optional)
**Rl:** ein ARM-Integer Register, das die niedrigwertigsten 32-Bit beinhaltet
**Rh:** ein ARM-Integer Register, das die höchstwertigsten 32-Bit beinhaltet
**Dd:** eines der 64-Bit Vector-Register

# VMOV: Datentransfer zwischen NEON-Skalar und Integer Register