
How the tutorial is categorized? [[computer-from-scratch.excalidraw]]
Timestamps [[Chapters-breakdown]]

In order to follow the tutorial/lecture, I have installed `Digital`. My plan is to design an 8-bit computer. Here is the [[Installation Guide - Digital]]

Lets dive in. 
## Part 1 : Foundation
We start with the basic concept of binary. How the numbers are represented in base 2. How the decimail-binary conversion is done and vice-versa.
## Part 2 : Binary Arithematic

Addition is straight forward. For the subtraction, we introduce the concept of [[2's complement]]
With it, we can use the same adder circuit to perform the subtraction as well.
## Part 3 : Logic Gates

* AND Gate
* OR Gate
* XOR Gate - exclusive OR gate outputs 1 when its two inputs are **different**, and 0 when they're the same.
* NOT Gate -  Inverter
* NAND Gate - [[Why NAND gate is called Uiversal Gate?]]

## Part 4: Constructing a circuit from logic gates
* Selector Circuit [[selector-circuit.excalidraw]]
	```bash
	if S=0 then Output=A
	if S=1 then Output=B
	```

* Half Adder : [[half-and-full-adder-gates.excalidraw]]
	```bash
	Sum   = A XOR B
	Carry = A AND B
	```
	
*  Full Adder: [[half-and-full-adder-gates.excalidraw]]
	```bash
	Sum  = A XOR B XOR Cin
	Cout = (A AND B) OR (Cin AND (A XOR B))
	```
* Chaining the Full Adder: [[8-bit-adder.excalidraw]]
	If we look at the 8-bit adder, so far it only does addition. How to extend this capability to do the subtraction? If we remember from earlier, A-B is nothing but A+(-B) from [[2's complement]] section.
* Extending it for substraction:[[8-bit-adder-subtractor.excalidraw]]
	 1. Put an XOR between each input bit Bi and its full adder.
	2. Wire the second input of all 8 XORs to the same **SUB** line.
	3. Connect SUB to FA0's **Cin**, which was tied to 0 before.
## Part 5 : Sequential Logic
**Sequential logic** is digital logic whose output depends on the **current inputs and the past** (stored state), not just the inputs.
```bash
Combinational: Output = f(Inputs)
  Examples: adder, selector, XOR. Same inputs always give the same output.

Sequential: Output = f(Inputs, Stored state)
  Examples: latch, flip-flop, register, counter. Same inputs can give different outputs.
```

### Latch
![[Sequential-logic.png]]Pattern:
1. When C = 0 , the output doesn't change
2. When C=1, the output follows A
3. When C goes from 1 to 0, we lock in A's value

Now let's visualize these steps in Digital

![[Latch-concept.png]]
1. Here, we can see that, I set value of A from low to high and again low to high. Since C was low, the output didn't change.
2. At step 5, i turned on the value of C to high. Then value of A was turned high from low twice. If you look at output Y, it is following A.
3. Now at the end of step 11, the value of C was made low. A was again made low to high twice but the Y was low all the time and didn't follow the value of C.

