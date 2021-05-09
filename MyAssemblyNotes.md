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
To translate Binary to Decimal we start from the left and multiply the each digit a power of `2`.
forexample consider the binary number `110` will be equal to `6`.
Solution: 
`2E2 x 1 + 2E1 x 1+ 2E0 x 0 = 6`
where `2En` means 2 to the `raised power`.

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

## Binary Addition:
When Adding Binary Number keep in mind that `1+1` will produce `10`with `1` being the carry to the next digit.

`00000100 + 00000111 = 00001011`

--- 

## Integer Storage Sizes (Data types):
Basic Storage unit in a `x86` computer is `byte` containing `8 bits`.Some of the data types are:

|Data| Type | Size |
|Byte |8|
|Word |16|
|DoubleWord| 32|
|Quadword| 64|
|Double Quadword| 128 |

----

## Hexadecimal Addition:
Hexadecimal Addition works the same way as decimal addition.Hexadecimal addition is used to locate new addresses in the memory.
For example, let’s add the hexadecimal values 6A2 and 49A. In the lowest digit position,`2 + A = decimal 12`, so there is no carry and we use `C` to indicate the hexadecimal sumdigit. In the next position, `A + 9 = decimal 19`, so there is a carry because `19 ≥ 16` , the number base. We calculate `19 MOD 16 = 3`, and carry a 1 into the third digit position. Finally,we add `1 + 6 + 4 = decimal 11`, which is shown as the letter `B` in the third position of the
sum. The hexadecimal sum is `B3C`.

So , `6A2 + 49A = B3C`

----

## Signed Binary Integers
Signed Binary Integers can be `positive` or `negitive`.The `Most Significant Bit MSB` represents the sign: `0 is positive and 1 is negitive`.
 
-----

## Two's-Complement Representation
`Negitive Integers` use Two's-Complement Representation using the rule that two's complement of a number is its `additive inverse`.
forexample consider a value 11111110, its two's complement will be:

|Starting value| 00000001|
|--------------|---------|
|Step 1: Reverse the bits |11111110|
|Step 2: Add 1 to the value from Step 1|11111110 +00000001|
|Sum: Two’s-complement representation|11111111|

## Hexadecimal Two's-Complement:
In two's complement of hexadecimal numbers we firstly reverse the number and then add 1 to it.To reverse the number we can subtract the number from `15`.
Here are the examples: 

`6A3D --> 95C2 + 1 --> 95C3`
`95C3 --> 6A3C + 1 --> 6A3D`

## Coverting Signed Binary to Decimal:
if the `Most Significant Bit` is `1` then it means the number is a `negitive signed integer`.For example, signed binary `11110000` has a `1` in the highest bit, indicating that it is a negative
integer.

|Starting value| 11110000|
|--------------|---------|
|Step 1: Reverse the bits| 00001111|
|Step 2: Add 1 to the value from Step 1|00001111 + 1|
|Step 3: Create the two’s complement|00010000|
|Step 4: Convert to decimal|16|

As the integer was negitive so its answer is `-16`.

## Coverting Signed Decimal to Binary:
Following are the Steps for this conversion:
<ul>
	<li>First, Convert The decimal integer to Binary</li>
	<li>If original Decimal was negitive create two's complement of the binary number from previous conversion</li>
</ul>

Forexample `-43` will be converted to binary as follows:
<ul>
	<li>43 in Binary is 00101011.</li>
	<li>As the original value is negitive we take two's complement of its binary which will be 11010101.That is how -43 will be represented.</li>
</ul>

## Converting Signed Decimal to Hexadecimal:
For the conversion do the following steps:
<ul>
	<li>Conver the absolute Decimal value to Hexadecimal.</li>
	<li>If original decimal was negitive take two's complement of its hexadecimal equilent.</li>
</ul>

## Converting Signed Hexadecimal to Decimal:
For conversion follow these steps mentioned below :
<ul>
	<li>If the giver hexadecimal number is negitive, take its two's complement ,otherwise retain the original number</li>
	<li>Convert it to decimal after the earlier step.</li>
	<li>if original value was negitive add minus sign to it.</li>
</ul>









