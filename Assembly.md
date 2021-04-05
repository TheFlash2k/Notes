# Author: Ali Taqi Wajid

<br><h1 style="text-align:center;">Assembly Language.</h1><br>

# > x86 Data Types:
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

# > Basic x86 Microcomputer Design:
## 1. CPU:
It is called Central Processing Unit and it is where the calculations and all the logical operations take place and it contains a limited number of storage locations called `registers`.
The other components interact with the CPU through the use of PINS
Most of these pings connect to the `data bus`, `control bus` and `address bus`
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

# > Registers:
 - In any of the register, we cannot directly access the upper 16 bits.

# > Integers:
## Structure:

# > Directives:
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

# > Instruction:
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

### ENDIANESS:
Defines how data is stored and received from memory.
x86 uses Little Endian Order. This means that LSB is stored at the first memory address allocated. The rest are stored in consecutive memory block and go from LSB to MSB. Consider the following number : `12345678h`
In little endian, the table would be as follows:
```
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