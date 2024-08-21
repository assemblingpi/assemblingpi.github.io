
## Operanden und Adressierungsarten

Die meisten Anweisungen in Assembler benötigen Operanden, um Daten zu verarbeiten. Eine Operandenadresse gibt dabei den Speicherort der Daten an. Das gilt nicht nur für Daten im Speicher, sondern auch für Register in der CPU. Obwohl Register keine physischen Adressen wie der Arbeitsspeicher haben, besitzen sie eindeutige Kennungen (z.B. `R0`, `R1`), die die CPU verwendet, um auf die darin gespeicherten Daten zuzugreifen. Diese Registeradressen ermöglichen es der CPU, gezielt auf die benötigten Daten in den Registern zuzugreifen.

Einige Anweisungen benötigen keinen Operanden, während andere ein, zwei oder drei Operanden erfordern können.
Wenn eine Anweisung zwei Operanden benötigt, ist der erste Operand das Zielregister und der zweite Operand ist die Quelle. Die Quelle enthält entweder die zu übertragenden Daten (bei direkter Adressierung) oder die Adresse der Daten (in einem Register oder Speicher). 

### Adressierungsarten
Die Adressierungsart gibt an, wie die CPU auf Daten zugreift. Sie legt fest, wie die in einem Befehl angegebene Adresse verwendet wird, um den Speicherort zu bestimmen, auf den zugegriffen werden soll. Dies kann direkt, über Register oder durch Berechnung mit Offsets geschehen.

#### Immediate Adressierung:
```
opcode Rd, #imm 
```

Die Quelle ist ein Wert, der direkt im Befehl eingebettet ist. Einen solchen Wert nennt man auch immediate oder direkter Wert.
   

#### Register-Adressierung:
```
opcode Rd, Rn
```

Die Quelle ist ein Registerwert.
  

#### Register-indirekte Adressierung:
```
opcode Rd, [Rn]
```

Die Quelle liegt im Speicher und die Adresse des Speicherorts befindet sich in einem Register.
     

#### Register-indirekte Adressierung mit Offset:
```
opcode Rd, [Rn, #offset]
```

Die Quelle liegt ebenfalls im Speicher. Ein Offset wird zur Basisadresse, die im einem Register liegt hinzugefügt, um die Zieladresse zu bestimmen.
    

#### Pre-Indexed Adressierung:
```
opcode Rd, [Rn, #offset]!
```

Die Quelle liegt ebenfalls im Speicher. Der Offset wird zur Basisadresse hinzugefügt, bevor auf den Speicher zugegriffen wird. Der Wert im Register Rn wird dadurch auch aktualisiert
     

#### Post-Indexed Adressierung:
```
     opcode Rd, [Rn], #offset
```

Der Speicherzugriff erfolgt zuerst auf die Basisadresse, danach wird der Offset hinzugefügt, um nachträglich das Basisregister zu aktualisieren.
     

#### Register-indirekte Adressierung mit zwei Registern:
```
opcode Rd, [Rn, Rm]
```

Zwei Register (`Rn` und `Rm`) werden verwendet, um die Speicheradresse zu berechnen. Die Basisadresse liegt in `Rn`, und der Wert in `Rm` wird als offset hinzugefügt.
     
[weiter](../CPUlator/erste.md)