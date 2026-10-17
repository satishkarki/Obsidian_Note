
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

This is called positive latch.

Lets look at the behaviour of positive latch and negative latch side by side:
![[positive-vs-negative-latch.png]]

```bash
Positive latch:  C = 1 → transparent (Y follows A)
                 C = 0 → hold

Negative latch:  C = 0 → transparent (Y follows A)
                 C = 1 → hold
```
### Flip-Flop [[latches-and-flip-flops.excalidraw]]
 ![[Flip-Flop.png]]

The difference between these two D Flip flops are the position of NOT gate, which decides which clock edge triggers them.

A latch is transparent for a whole clock _level_. A flip-flop samples its input only at one _instant_, the clock edge, and holds that value until the next edge. That makes it much more predictable.

**Left circuit (A, C → Y): negative-edge triggered**

- Latch 1 (master): select = C, so it's transparent when **C = 1**.
- Latch 2 (slave): select = NOT C, so it's transparent when **C = 0**.
- Y updates when C falls from 1 to 0.

**Right circuit (A1, C1 → Y1): positive-edge triggered**

- Latch 1 (master): select = NOT C1, so it's transparent when **C1 = 0**.
- Latch 2 (slave): select = C1, so it's transparent when **C1 = 1**.
- Y1 updates when C1 rises from 0 to 1.

## Part 6 : Capturing the output of ALU

Now that we have covered the foundation of sequential logic. We will now build the register to store the output of ALU.

Let's head to digital and create our 8-bit register.

![[Register-8bit.png]]

Isn't it fascinating ? Now if we slightly modify our circuit to remove the A inputs and rewire the output of D-FlipFlop back to the adder, we get the Program Counter. Here is how it looks:

![[Program-Counter.png]]



## Part 7 : RAM Contextualization

Just to grasp the concept, lets assume we have a RAM with four address and each address hold 4bit data. The logic diagram lets us visualize the read and write operation on it.


| Address | Address | Value |
| ------- | ------- | ----- |
| 00      | 0       | 0111  |
| 01      | 1       | 0100  |
| 10      | 2       | 0000  |
| 11      | 3       | 1010  |

![[4bit-RAM-RW.png]]


## Part 8 : Connecting RAM and ALU

Now lets connect the above RAM to a 4bit ALU via a bus
![[Connecting-RAM-ALU-via-BUS.png]]

## Part 9: Seven Segment Display

Its kind of hard to read the binary value. So lets extend the above diagram so that we can hook it up to a seven segment display.

Lets look at the pin out of the display first. [[7-segment-display.excalidraw]]

Now lets build this on digital .
![[seven-segment-display.png]]


Now lets combine this display to our 4-bit adder circuit. Here is how it looks now as a complete 4bit circuit with one display. As for the simplicity we have only used one display, any addition resulting in two digits will not be captured on the output. Also, I went on and added a display register to hold output value.

![[Complete-circuit-4bitadder-with-display.png]]

Now lets tie everything together as an abstraction for a 8bit computer [[computer-overview.excalidraw]]

## Part 10 :  Instruction Register
Up until now we have been pushing multiple buttons in `digital` to perform addition and substraction. Lets automate the process.

For our computer overview from above, lets consider 8 bit instruction register

| Short name | Full name | Opcode | Description     |
| ---------- | --------- | ------ | --------------- |
| LDA        | LOAD -A   | 0001   | Reg A= RAM[arg] |
So what it means is - for our 8bit instruction register, lower 4 bits are called operand or arg. The upper 4 bits are called opcode.  With 4 bit opcode, we get 2^4 = 16 different instructions.

So in order to perform the LDA instruction, here is what needs to happen in our circuit:
* Assumption: IR has the right command loaded
* IR OUT =1
* MAR IN = 1
* IR OUT = MAR IN = 0
* RAM OUT = 1
* REGISTER A = 1
* RAM OUT = REGISTER A =0

## Part 11 : Clock
 Let's try to analyze the above LDA instruction in terms of clock cycle [[clock-timing.excalidraw]]
The reason for the introduction of clock at this phase is to synchronize all the operation in our 8bit computer. Let's look at the modified diagram of the abstraction view including the clock signal.

Now here how our updated computer overview looks after adding the clock [[computer-overview-with-clock.excalidraw]]

## Part 12 : Program Counter
  
Lets start with the assumption- the first address of RAM i.e 0000 in our case is not an address to a value but the first instruction for our Instruction Register (IR)

With this assumption let's introduce the clock cycle for PC in our LDA instruction [[clock-timing-with-PC-Cycle.excalidraw]]

Here is how the abstraction will look like now [[computer-overview-with-PC-Counter.excalidraw]]

## Part 13 : Designing the ISA


We have closely looked at how the `LDA` instruction works above. Based on that intuition, here are the other instruction that we designed:

| Short Name | Full Name      | Opcode | Description                              | Argument                  |
|------------|----------------|--------|------------------------------------------|---------------------------|
| LDA        | Load-A         | 0001   | Reg A = RAM[arg]                         | Data address in RAM       |
| ADD        | Addition       | 0010   | Reg A = Reg A + RAM[arg]                 | Data address in RAM       |
| SUB        | Subtraction    | 0011   | Reg A = Reg A - RAM[arg]                 | Data address in RAM       |
| STA        | Store A        | 0100   | RAM[arg] = Reg A                         | Data address in RAM       |
| LDI        | Load Immediate | 0101   | Reg A = arg                              | Literal                   |
| JMP        | Jump           | 0110   | Program Counter = arg                    | Instruction address in RAM |
| JC         | Jump if Carry  | 0111   | If Carry Flag = 1, PC = arg, else no-op  | Instruction address in RAM |
| JZ         | Jump if Zero   | 1000   | If Zero Flag = 1, PC = arg, else no-op   | Instruction address in RAM |
| OUT        | Display        | 1110   | OUT = Reg A = Display                    | None                      |
| HLT        | Halt           | 1111   | Stops computer                           | None                      |
One thing to notice is that the opcode is something we decided upon and it doesn't have to be the same as above. Since we decided on 4 bits for our opcode, our design can have 2^4 =16 instructions.

We have added some components to our computer overview diagram for the instructions we designed above. Let's look at our updated Computer Overview diagram [[computer-overview-v13.excalidraw]]


The last instruction in our table is `HLT` and this is how it is implemented in our computer overview diagram [[clock-halt.excalidraw]]

## Part 14: Control Brain

Going back to our `digital` circuit, we have been manually punching in the values and flags to peform additon/substraction.

![[punch-in-value.png]]

How we can automate this part? - On our abstraction of computer overview, up until we left out the control brain part. This is the crux of our tiny computer. It ties together all the parts. 

Let's create this control brain. Before that, lets look at our modified computer overview [[computer-overview-with-control  brain.excalidraw]]    

This control brain is nothing but a control ROM. And we have already create an encoder for our 1U seven-segment display. We will use the same concept to built this ROM.But before that, lets look at all the timing diagram of each CPU instructions.

## Part 15: Timing of each CPU instructions

