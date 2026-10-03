Because **any logic function can be built using only NAND gates**. Since every circuit is made of NOT, AND, and OR (or equivalents), if NAND can make those three, it can make anything.

**NOT from NAND**  
Tie both inputs together:

```
NOT A = A NAND A
```

**AND from NAND**  
NAND followed by a NOT:

```
A AND B = NOT (A NAND B) = (A NAND B) NAND (A NAND B)
```

**OR from NAND**  
Invert each input first, then NAND them (De Morgan's law):

```
A OR B = (NOT A) NAND (NOT B) = (A NAND A) NAND (B NAND B)
```

With NOT, AND, and OR covered, you can build XOR, adders, the ALU, latches, RAM, and a full CPU from NAND alone.

**NAND truth table**

```
A  B | A NAND B
0  0 |   1
0  1 |   1
1  0 |   1
1  1 |   0
```

Output is 0 only when both inputs are 1.

**NOR is also universal**, for the same reason (by symmetry). AND, OR, and XOR are not universal; you can't build NOT from AND or OR alone.

**Why it matters in practice**

- Manufacturing: you only need to fabricate and optimize one gate type. NAND is also compact and fast in CMOS.