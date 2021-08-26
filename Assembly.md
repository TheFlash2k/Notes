# Author: Ali Taqi Wajid
### Book : [KIP R. Irvine Assembly Language for x86 Processor Sixth Edition](http://index-of.es/Programming/Assembly/Assembly%20Language%20for%20x86%20Processors%206th%20Ed.pdf)
### Course : *CS232*- Computer Organization and Assembly Langauge
---
# NOTE: I'm learning and making these notes at the same time, do let me know if there's any issue in any of these :)
---
# List of Content:
### [Number Conversions](#Number-Conversions) -> Haven't written about it yet (Very long topic)
### [x86 Data Types](#x86-data-types-1)
### [Basic x86 Microcomputer Design](#basic-x86-microcomputer-design-1)
### [Instruction Execution Cycle](#instruction-execution-cycle)
### [x86 Operating Modes](#x86-operating-modes-1)
### [Registers](#registers-1)
### [Integers](#integers-1)
### [Directives](#directives-1)
### [Instruction](#instruction-1)
### [Endianess](#endianess-1)
---

# Number Conversions:
[A very good online table](https://kb.iu.edu/d/afdl)
[A good signed/unsigned converter](https://www.mathsisfun.com/binary-decimal-hexadecimal-converter.html)

# x86 Data Types:
## Unsigned:
| System | Range | Power of 2 | In Bytes | In Bits |
| -------- | ------ | ----------- | -------- |--------|
| Byte | 0-255 | 0 to (2<sup>8</sup> - 1) | 1 Byte | 8 bits |
| Word | 0-65535 | 0 to (2<sup>16</sup> - 1) | 2 Byte | 16 bits |
| Doubleword | 0-4,294,967,295 | 0 to (2<sup>32</sup> - 1) | 4 Byte | 32 bits |
| Quadword | 0-18,446,744,073,709,551,615 | 0 to (2<sup>64</sup> - 1) | 8 Byte | 64 bits |

## Signed:
| System | Range | Power of 2 | In Bytes | In Bits |
| -------- | ------ | ----------- | -------- |--------|
| Byte | -128 to +127 | (-2<sup>7</sup> to +2<sup>7</sup> - 1) | 1 Byte | 8 bits |
| Word | -32,768 to +32,767 | (-2<sup>15</sup> to +2<sup>15</sup> - 1) | 2 Byte | 16 bits |
| Doubleword | -2,147,483,648 to +2,147,483,647 | (-2<sup>31</sup> to +2<sup>31</sup> - 1) | 4 Byte | 32 bits |
| Quadword | -9,223,372,036,864,775,808 to +9,223,372,036,864,775,807 | (-2<sup>63</sup> to +2<sup>63</sup> - 1) | 8 Byte | 64 bits |

# Basic x86 Microcomputer Design:
## 1. CPU:
It is called Central Processing Unit and it is where the calculations and all the logical operations take place and it contains a limited number of storage locations called `registers`.
The other components interact with the CPU through the use of PINS
Most of these pins connect to the `data bus`, `control bus` and `address bus`
It also has the following parts:
- High Frequency Clock
- Contrl Unit (CU)
- Arithmetic Logic Unit (ALU)

### Clock:
A clock synchronizes the internal operations of the CPU with other components within the system.
### Control Unit:
The control unit coordinates the sequences of steps involved in executing machine instructions.
### Arthimetic Logic Unit:
It performs all the arthimetic opertions such as addition, subtraction and/or multiplication. And it also handles the boolean experessions such as AND, OR and/or NOT.

## 2. Memory Storage Unit:
It is the main storage unit.
Receives requests for data from the CPU.
Is used to transfer data from RAM to CPU and vice versa.
Holds the data and instructions of the program(s) that is/are running.
Has the following types:
- ROM
- RAM

## 3. Buses:
![](buses.png) <br>
A bus is a group of parallel wires that copy data from one of parts of the computer to another. A bus is a pathway for digital signals to rapidly move data. There are 3 buses:
- Data Bus
- Control Bus
- Address Bus
### Data Bus:
The data bus transfers data between the cpu and the memory.
### Control Bus:
Control bus uses binary signals to synchronize actions of all devices attached to the system bus.
### Address Bus:
Address bus holds the addresses of the instructions and data when they are currently execution. The address bus carries addressing signals from the processor to memory, I/O (or peripherals), and other addressable devices around the processor. Control signals move out of the processor, but not in to it.
[Reference](https://www.microcontrollertips.com/internal-processor-bus-data-address-control-bus-faq)

# Instruction Execution Cycle
> Haven't really touched this topic yet so not making any notes about it.

# x86 Operating Modes:
There are three primary operating modes:
- Protected Mode
- Real-Address Mode
- System Management Mode

## Protected Mode:
It has a sub-mode called `Virtual-8086 Mode`. This mode is the native state of the processor in which all instructions and features are available.  Programs are given seperate memory areas called `segments`. Which allows the processor to prevent programs from referencing outside of their segments. While in protected mode, the processor can directly executre real-address mode software such as `Obsidian`, `Word` etc in a safe multi-tasking environment. This means that, consider if a program was running such as `Sublime Text` and the program crashes, this mode would allow the program to be isolated and do not affected the other programs running.

## Real-Address Mode:
In this mode, the programming environment of Intel 8086 processor is implemented with a few extra features such as the ability to switch into other modes. This mode allows direct access to the system memory/hardware devices. and is used to run `Microsoft` or other vendors' programs. Programs running in this mode may allow the Operating System to crash.

## System Management Mode:
This is also known as `SMM`. It provides the Operating System with the mechanisms to implement to functions such as Power Management, System Security. These are often used by computer manufacturers who customize their processor for a particular system setup. 

## Difference between each mode:
One of the major difference b/w each of the modes is in the way memory addressing works. Both the amount of memory that can be addressed and the translation process between logical addresses and to physical addresses may vary depending on the operating mode.

# Registers:
![](registers.png) <br>
 A register is a storage unit / container inside the processor core which can be accessed at much higher speeds than the conventional memory.
 ## 8086 Registers:
 Before moving on towards the more complex `extended` registers of x86 architecture, we must first know the basics, the `16 bit` registers or the `8086` registers. These registers are split into the following four categories.
 - General Purpose Registers
 - Segment Registers
 - Special Purpose Registers

### 16-bit General Purpose Registers
General There are eight general purpose registers:
#### AX:
AX is used as an [`accumulator`](https://www.computerhope.com/jargon/a/accumulator.htm) and is preffered for most arthimetic and logical operations. It acts as a temporary storage location which holds an intermediate value in a mathematical and logical calculation. Each of the previous value in the accumulator is overwritten with new intermediate results of an operation.
Consider the following example, `3 + 4 + 5`, the accumulator would firstly hold the value `3`, then add 4 and  then it would hold `7` and then 5 would be added to 7 and the accumulator would hence hold `12`. The `AX` register consists of two sub-registers, each of `8 bits`. One is called `AH`, the other is `AL`, where H and L represent `High` and `Low` respectively.
In order to understand these, consider a 16-bit binary number,
<b><p style="text-align:center;">01011010-01001001</p></b>
This number is broken down into two seperate portions, each of 8 bit.
The upper portion - `01011010` is called `AH` -> Accumulator High, while, the lower portion `01001001` is called `AL` -> Accumulator Low.
-> NOTE: All The `General Purpose Registers` are divided into sub-registers, each of `8 bits` in a similar fashion.
Consider a following small assembly program to get a better grasp at the concept:
-> Ignore the `syntax` at this moment, everything will be explained later in a much better fashion.
```asm
; Example code for understanding the use of AX
.386
.model flat , stdcall;
.stack 4096
.code
main PROC
	; Adding three numbers: 3 + 4 + 5
	mov ax, 3
	add ax, 4
	add ax, 5
	; ax -> 12
	ret
main ENDP
END main
```
After running this program, we can have a look at the registers window in our Visual Studio, we can see that, AX -> 12. Just by looking at the `main` procedure, we can somewhat grasp as to what is going on.
#### BX:
BX is used as a `base` register and is typically used to hold the address of a procedure or a variable. In other words, it stores the value of the offset. Just like AX, this also can be divided into 2 sub-registers of 8 bits known as `BH` and `BL`
#### CX:
CX is called as `count` register and is typically used for looping. Just like AX, this also can be divided into 2 sub-registers of 8 bits known as `CH` and `CL`
#### DX:
DX is called as `data` register and is commonly used for multiplication and division. Just like AX, this also can be divided into 2 sub-registers of 8 bits known as `DH` and `DL`.
> Each of these `GPR` can be treated either as a `16 bit` quantity or an `8 bit` quantity. We have already discussed this topic in [AX](#AX)
#### SI:
`SI` stands for `Source Index`. It is used in the pointer addressing of data and as a source in some string related operations. It’s offset is relative to data segment.
#### DI:
`DI` stands for `Destination Index`. It is used in the pointer addressing of data and as a destination in some string related operations.It’s offset is relative to extra segment.
#### BP:
`BP` stands for `Base Pointer`. It is primary used in accessing parameters passed by the stack. It’s offset address relative to stack segment.
#### SP:
`SP` stands for `Stack Pointer`. It points to the topmost item of the stack. If the stack is empty the stack pointer will be `(FFFE)H` It’s offset address relative to stack segment.

### Segment Registers:
Segment registers deal with selecting blocks of main memory. A segment register is 16-bit. A segment register points at the beginning of a segment in memory. In `8086`, the segments can be no longer than 64K bytes. This `64K segment limitation` caused a lot of issues. Keeping those issues aside, there are 4 segment registers:
#### CS:
`CS` stands for `Code Segment`. This register points at the segment containing the currently executing machine instructions. The `64K segment limitation` that we talked about earlier doesn't apply here as the `8086` programs can be longer than the 64K bytes size. What we need is multiple code segments. As we know that we can easily change the value of the `CS` register, we can easily switch to next segment having the remaining part of the code and then execute it from there.
#### DS:
`DS` stands for `Data Segment`. This register generally points at the global variables in the program. Again, we're limited to `64K bytes` but we can always change the value and access the data in the other segments.
#### ES:
`ES` stands for `Extra Segment`. What it is, is, well - an extra segment. This segment comes into play when the programs want access to segments and it is unable to access or modify the other segments.
#### SS:
`SS` stands for `Stack Segment`. This register points at the segment containing the stack. `The Stack` contains all the important stuff such as `machine state information`, `subroutine return addresses`, `procedure parameters`, and `local variables of a particular subroutine`. In a normal 8086 implementation, the `Stack Segment` register cannot be modified as many of the underlying implementation of the program and that of the system, depends on it.

### Special Purpose Registers:
There are two special purpose registers. We cannot access these registers the same way as we can `GPR`. These are controlled and manipulated by the CPU directly. The special purpose registers are:
#### Instruction Pointer:
`Instruction Pointer` is also reffered to as `IP`. This contains the address of the instruction currently being executed.
#### Flags Register:
Flags register; unlike all the other registers, only hold collection of 1 bit values. The size of the flag register is `16 bits` but, `8086` only uses `9 bits`. Out of those, only `four` is used most of the time. 4 of those flags are:
- Zero
- Carry
- Sign
- Overflow

## x86 Registers.
### General Purpose Registers:
Similar to [16-bit general purpose registes](#16-bit-general-purpose-registers), there are 8 32 bits general purpose registers:
- EAX
- EBX
- ECX
- EDX
- EBP
- ESP
- ESI
- EDI

Where `E` in each of these stands for `Extended`. Meaning each of these are extended from their orignal counter-parts. EAX is `Extended` from `AX` etc. Meaning, an additional 32 bit extra data can be stored in each of these registers. As we can notice that, the 16-bit Segment Registers from 8086 have also been added here as part of the General Purpose Registers.

### Segment Registers:
Similar to 8086, the Segment Registers in x86 are also 16 bits. But, from 4 in 8086, their amount has been increased to 6. These are:
- CS
- ES
- SS
- DS
- FS
- GS

### Special Purpose Registers
#### Instruction Pointer
#### Flags Register

## x64 Registers:
### General Purpose Registers
### Segment Registers
### Special Purpose Registers

# Integers:
## Structure:
> Haven't touched this topic but writing the heading so i remember that i have to study it.

# Directives:
- Assist and control assembly process
- Change the way code is assembled
- Also called pseudo-ops
- Not a part of the instruction set

## Directives used:
### .CODE:
- Indicate the start of a code segment
- The actual source code goes in this segment

### .DATA:
- Indicates the start of a data segment
- All the variables that are to be used can be declared in this segment

### .STACK:
- Indicated the start of a stack segment

### .END:
- Marks the end of a module.

### .DD:
- Allocates a double word (4 bytes)

### .DWORD:
- Same as .DD

# Instruction:
- A statement that becomes exectuable when a program is assembled is called an Instruction.
- They are translated by assembler into machine language bytes.

## Parts of an Instruction
```asm
	[label:] mnemonic [operands] [;comment]
where label and comments are optional

Example:
	start:	mov eax,1000h ;EAX = 10000h
	; Where:
	; start: 		 <- Label:
	; mov    		 <- mnemoic
	; eax    		 <- Register -> Operand 1
	; 1000h  		 <- Operand 2
	; ;EAX = 10000h  <- Commend
```
### Label:
- Used as a place marker for instructions and data
```asm
; Examples:
; Data Label
count DWORD 100 ; <- Declares a variable named count of 32 bytes and assigns a value of 100

; Code Labels
; Example:
;; Loop:
start:
	mov ax, bx
	jmp start ; goes back to start
; These can also be oneliners:
start: mov ax,bax ; <- This is completely valid
```

### Mnemonic:
- Identifies an instruction:
#### Some mnemonics:
- mov : Assigns one value to another.
```asm
	mov 10, eax; Assigns 10 to eax.
```
- add: Adds two values together.
```asm
	add 10, 15; adds 10 to 15.
```
- sub: Subtracts one value from another
```asm
	mov 15, eax
	sub eax, 20
```
- mul: Multiplies two values
```asm
	mul 20, 5; 100
```
- jmp: Jump to a new location -> Is used to jump to a data label
```asm
	start:
		mov	eax, 25;
		jmp start; -> Acts as an infinte loop
```
- Call: Calls a procedure. Just like a function call in c++
```asm
	call DUMPREGS; an irvine library procedure.
```

### Operand:
- Also known as Arthimetic
- Quantity on which an operation can be done.
- Operands can be constants e.g. 20
- They can also be constant expression e.g. 35-7
- They can also be a register name such as `EAX`
- They can also be a location in memory e.g. `count` where `count` is variable name

### Block Comments:
- Block Comments:
- `COMMENT` Keyword is used to declare a multiline comment.
```asm
COMMENT !
	THIS IS
		A MULTI LINE
			COMMENT
!
COMMENT &
	AMPERSAND can also be used to
		represent multiline
	comments
&
```

### Simple Assembly Program:
```asm
.386
.model flat , stdcall;
.stack 4096
.code
main PROC
	mov eax, 10000h
	add eax, 40000h
	sub eax, 20000h
	ret
main ENDP
END main
```

#### Breakdown:
##### .386:
This directive identifies the minimum CPU required to run the program
##### .model:
This is used for two purposes
- Identifying the segmentation model (memory model)
- Identifies the convention used to Pass parameters to procedures
-> FLAT:
This tells the assembler to use the Flat memory model meaning that it tells the assembler to generate  code for Protected-mode programs.
-> STDCALL:
Enables the calling of built-in Windows API.
##### .STACK:
Used to allocate a stack of N bytes.
##### .CODE:
Marks the beginning of the code segment where all the executable statement are located.
##### PROC:
Identifies the beginning of a procedure. The name of the procedure is followed by the PROC keyword. In our program:
`main PROC`. Where `main` is the name of the procedure.
##### ENDP:
Marks the end of a procedure. Similar to PROC, ENDP is preceded by the name of the procedure.
##### END:
END is the last instruction that is to be executed. It is followed by the name of the starter procedure.

### Declaring Variables in x86 Assembly:
```asm
.386
.MODEL flat
.STACK 4096

.DATA
num1	dword	11111111h
num2	dword	10101010h
ans		dword	0

.code
main PROC
	mov eax, num1
	add eax, num2
	mov ans, eax
	ret
main ENDP
END main
```

### Datatypes:
#### Directives:
| Directive | Number of BitsS | Description |
| ----------|------------------|-------------|
| BYTE | 8 Bit | 8-bit unigned Integer |
| SBYTE  | 8 Bit | 8-bit unsigned Integer |
| WORD | 16 Bit | 16-bit unsigned Integer |
| SWORD | 16 Bit | 16-bit Signed Integer |
| DWORD | 32 Bit | 32-bit unsigned Integer |
| SDWORD | 32 Bit | 32-bit Signed Integer. SD stands for Signed Double |
| FWORD | 32 Bit | 32-bit Signed Integer where F is Far pointer in protected mode|
| QWORD | 64 Bit | 64-bit integer. Q = Quad |
| TBYTE | 80 Bit | TBytes means 10 bytes and hences occupies 80 Bits in memory. |
| REAL4 | 32 Bit | 4 Bytes IEEE short real |
| REAL8 | 64 Bit | 8 Bytes IEEE long real | 
| REAL10 | 80 Bit | 10 Bytes IEEE extended real | 

#### Legacy Directives:

| Directive | Number of Bits |
| ---------- | -------------- |
| DB | 8 bit integer |
| DW  | 16 bit integer |
| DD | 32 bit integer or real |
| DQ | 64 bit integer or real |
| DT | 80 bit integer |

### BYTE:
A BYTE occupies 8 bits in memory and can represent 0-255 if unsigned or -128 to +127 if it is a signed BYTE. 
```asm
;; Initialized Variable
	char1 BYTE 'c'  ; character constant
	num2 BYTE 0     ; smallest unsigned byte
	num3 BYTE 255   ; largest unsigned byte
	num4 SBYTE -128 ; smallest signed byte
	num5 SBYTE +127 ; largest signed byte
	num6 BYTE 00011011b ; Binary digit
;; Uninitialized Variable
	num6 SBYTE ?    ; uninitialized variable
;; Multiple Initializers
	list BYTE 10, 20, 30, 40 ; Array
	;; They will be stored in consecutive memory blocks
	COMMENT !
		OFFSET   |   VALUE
		0000     |    10
		0001     |    20
		0002     |    30
		0003     |    40
	!
	;; The first number will be stored at the lowest memory address
	;; The last number at the highest.
```
- Strings

```asm
hello BYTE "Hello, World", 0 ; This is a null terminated string
;; Strings can also be multilined
hello BYTE "Welcome Back"
	  BYTE "Are you ready?"
	  BYTE "Let's GOooooo", 0 ; The last portion is null-terminated.
```

### WORD:
A WORD occupies 16 bits in memory and can represent 0-65535 if unsigned or -32768 to +32767 if it is signed. 
```asm
;; Initialized Variable
	word_1 WORD 65535d    ; largest unsigned value
	word_2 WORD -32768d   ; smallest signed value
;; Uninitialized Variable
	uninit WORD ?         ; uninitialized variable, unsigned
;; Multiple Initializers
	list WORD 1,2,3,4,5 ; Array
	;; They will be stored in consecutive memory blocks
	COMMENT !
		OFFSET   |   VALUE
		0000     |    1
		0002     |    2
		0004     |    3
		0006     |    4
		0008     |    5
	!
	;; The first number will be stored at the lowest memory address
	;; The last number at the highest.
	;; Each offset has a difference of 2 as a WORD consists of 2 BYTES
```

### DWORD:
A DWORD occupies 32 bits or 8 Bytes in memory and can represent 0-4,294,967,296 if unsigned or -2,147,483,648 to +2,147,483,647 if it is signed. 
```asm
;; Initialized Variable
	dword_1 DWORD 4294967296d     ; largest unsigned value
	dword_2 DWORD -2147483648d   ; smallest signed value
;; Uninitialized Variable
	uninit DWORD ?         ; uninitialized variable, unsigned
;; Multiple Initializers
	list DWORD 1,2,3,4,5 ; Array
	;; They will be stored in consecutive memory blocks
	COMMENT !
		OFFSET   |   VALUE
		0000     |    1
		0004     |    2
		0008     |    3
		000C     |    4
		0010     |    5
	!
	;; The first number will be stored at the lowest memory address
	;; The last number at the highest.
	;; Each offset has a difference of 4 as a WORD consists of 4 BYTES
```

# ENDIANESS:
Defines how data is stored and received from memory.
x86 uses Little Endian Order. This means that LSB is stored at the first memory address allocated. The rest are stored in consecutive memory block and go from LSB to MSB. Consider the following number : `12345678h`
In little endian, the table would be as follows:
```sql
12345678h

Little Endian:
\x78\x56\x34\x12
Where:
78 is Least Significant Bit -> LSB
12 is Most Significant Bit -> MSB

 OFFSET       |    VALUES
  0000        |     78
  0001        |     56
  0002        |     34
  0003        |     12

If we were to look at Big Endian, the table would be as follows:

 OFFSET       |    VALUES
  0000        |     12
  0001        |     34
  0002        |     56
  0003        |     78
		  
```

# Data Transfer Instructions
![](reginfo.png) <br>
## Operand Types
### Instruction format with varying number of operands:
```asm
mnemonic 
mnemonic [destination]
mnemonic [destination], [source]
mnemonic [destination], [source 1], [source 2]
```
### Types of Instruction Operands:
There are 3 instruction operands:
1) Immediate -> uses numeric literal expressions
2) Register -> uses named Registers in the CPU
3) Memory -> uses references i.e. memory locations.

### Direct Memory Operand
Variable names are references to offset within the .DATA segment. Consider the following assembly code:
```asm
.DATA
	num1 BYTE 13h
```
This code shows that a `BYTE` with an identifier of `num1` has been allocated with data `13h` in the data segment.
Suppose, this `num1` was located at an offset of `10400h`. If we were to move it to a register, let's say, `AL` register which is of 8 bits. The Assembly Instruction would look something like this
```asm
mov AL, num1
```
Now, Let's suppose, before moving num1 to AL, AL was specifically set to `NULL` and hence, the value of `AL` would've been -> `00000000`. Now, we know that the offset of `num1` was `10400h`, Hence, after execution of this code, `AL` would become, `00010400`. Meaning that, the offset of num1 in memory would be written into the AL register. Although, we can also simply do a direct reference through use of OPCodes but it is consider good practice and makes the code readable if we simply use the variable/symbolic names

## The MOV Instruction.
The `MOV` instruction is self-explanatory. What it does is just simply copy data from the source operand to destination operand. A simple MOV Instruction format looks something like this:
```asm
mov [destination], [source]
```
The destination operand in this instruction is changed while the source remains unchanged. 
### Copying Smaller Values to Larger ones
In Assembly, we cannot directly copy a smaller value to a larger one. Consider `EAX`, if we were to copy a 16-bit instruction to that register, we would face an error as `EAX` is of `32-bits`. Now, in order to do that, we use a few methods. One of them is:
```asm
; Consider a count variable of 16 bits unsigned value.
.data
count WORD 1

.code
mov ECX, 0
mov CX, count
```
By looking at the code above, we can see that, what we're actually doing is firstly setting the entire 32-bit register to 0. Which would make ECX look something like this
```asm
00000000 00000000 00000000 00000000
				 ======= CX =======
============== ECX ================
```
                                   
Now, when `mov CX, count` is executed, the first 16 bits of ECX are set to the value of `count` and hence it is changed to something like this:
```asm
00000000 00000000 XXXXXXXX XXXXXXXX
				 ======= CX =======
============== ECX ================
; Where X represents any hexadecimal value.
```

### MOVZX (MOV with zero-extend)
> Works only with unsigned values