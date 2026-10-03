
**Idea:** subtraction becomes addition: **A − B = A + (−B)**, so one adder does both.

**Negating a number (2 steps)**

1. Flip all bits.
2. Add 1.

Example, −5 in 8 bits:

```bash
 5  = 00000101
flip = 11111010
+1   = 11111011   (−5)
```

**Example: 9 − 5**

```bash
 9  = 00001001
−5  = 11111011
sum = 1 00000100  → drop the carry → 00000100 = 4 ✔
```

**Example: 5 − 9 (negative result)**

```bash
 5  = 00000101
−9  = 11110111
sum = 11111100  → leading 1 = negative
```

Negate to read it: flip → 00000011, +1 → 00000100 = 4, so the result is **−4** ✔

**Key facts**

- Leftmost bit is the sign (0 = positive, 1 = negative).
- 8-bit range: −128 to +127.
- Only one zero.
- Overflow: two positives giving a negative (or vice versa) means the result didn't fit.

**In hardware:** XOR each bit of B with a "subtract" signal (flips the bits) and wire that same signal to the adder's carry-in (adds 1).

