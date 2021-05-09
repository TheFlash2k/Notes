# Notes Made by: [A51F221B](https://github.com/A51F221B)
### Reference Book : [KIP R. Irvine Assembly Language for x86 Processor Sixth Edition](http://index-of.es/Programming/Assembly/Assembly%20Language%20for%20x86%20Processors%206th%20Ed.pdf)
### Course : *CS232*- Computer Organization and Assembly Langauge


---


## Virtual Machine Concept in Assembly Langauges:
Computer Hardware and Software are related to each other through a concept called `Virtual Machine`.
To Understand this concept consider a program written in High level language such as c++ or java.Now we already know that Computer executes instructions in machine code, so  to convert this high level code into machine code we can use interpretation (line by line conversion to machine code) or Compilation of the entire program to machine code.
In terms of virtual machine we can say that first the program gets is converted from high level code to assembly code by Virtual Machine 1 and then that Assembly is converted to Machine code by Virtual Machine 2.

---

## Levels of instructions
### level 1:
level 1 can be the Digital Logic Hardware.
### level 2:
`Instruction Set Architecture` comes in level 2 where machine language is executed by a program embedded in computer microprocessor called microprogram.
### level 3:
This is where the `Assembly Langauge` operates which uses mnemonics such as ADD,SUB,MOV which are translated to machine language.
### level 4:
This is where most of us work today i.e. High Level Langauges operate here which get translated by their repective compilers.

--- 

## Binary Integers:
A computer simply uses `0s` and `1s` at the lowest level representing `off` and `on` state. Bits are numbered sequentially starting at zero on the right side and increasing toward the left. The bit on the left is called the `most significant bit (MSB)`, and the bit on the right is the `least significant bit (LSB)`.

| System |Base| Possible Digits |
| ------ |---|-----|
| Binary | 2 | 0 1 |
| Octal  | 8 | 0 1 2 3 4 5 6 7 |
| Decimal| 10| 0 1 2 3 4 5 6 7 8 9 |
| Hexadecimal | 16 | 0 1 2 3 4 5 6 7 8 9 A B C D E F |

Things to remember about `Binary Integers`:

<ul>
	<li>Binary Integers can be signed or unsigned</li>
	<li>Signed Integers can be either positive or negitive</li>
	<li>An unsigned integer is by default positive</li>
	<li>Zero is considered positive</li>
	<li>Dots can be inserted after every four binary integers for readibility</li>
</ul>

--- 

## Translating Unsigned Binary Integers to Decimal:
Weighted positional notation represents a convenient way to calculate the decimal value of an unsigned binary integer having n digits:
`dec  (D n1  2 n1 )  (D n2  2 n2 )    (D 1  2 1 )  (D 0  2 0 )`
D indicates a binary digit. For example, binary 00001001 is equal to 9. We calculate this value
by leaving out terms equal to zero:
`(1  2 3 )  (1  2 0 )  9`

## Translating Unsigned Decimals Integers to Binary:
To translate from Decimal to Binary we repeatedly divide the number by 2 saving each remaineder as a binary digit.

| Division | Quotient | Remainder |
|----------|----------|-----------|
|37 / 2 | 18 | 1 |
| 18 / 2 | 9 | 0 |
|9 / 2 | 4 | 1|
|4 / 2 | 2 | 0 |
|2 / 2 | 1 | 0 |
|1 / 2| 0 | 1|

We can concatenate the binary bits from the remainder column of the table in reverse order `(D 5 , D 4 , . . .)` to produce binary `100101`. Because computer storage always consists of binary
numbers whose lengths are multiples of `8`, we fill the remaining two digit positions on the left
with zeros, producing `00100101`.

