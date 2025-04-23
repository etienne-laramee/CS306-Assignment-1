# Phase 1
I propose to add the following 5 instructions to the instructions set of my CPU:
## Subtraction
**Subtraction** is a fundamental component of arithmetic operations. Although subtraction can be emulated using addition and two's complement, doing so requires additional instructions and CPU cycles. Since a subtraction circuit can be efficiently built by extending an existing adder with minimal components (e.g., using half-adders and subtractors), its inclusion is a low-cost, high-value enhancement to the ALU.
## Multiplication
**Multiplication** of unsigned integers is composed of repeated additions. While it can be emulated in software, hardware multiplication significantly reduces instruction count and execution time, especially for operations in graphics, signal processing, and mathematical computations. It is a standard operation in most instruction set architectures.
## XOR
**XOR** (exclusive OR) compares two binary inputs and returns 1 where the bits differ. This operation is essential for applications such as parity checks, cryptographic algorithms, and network protocols. Its versatility and computational efficiency justify its inclusion in the instruction set.
> GeeksforGeeks. (2021). _Bitwise XOR operator in programming_. [https://www.geeksforgeeks.org/bitwise-xor-operator-in-programming/](https://www.geeksforgeeks.org/bitwise-xor-operator-in-programming/)
## NAND
**NAND** is a universal logic gate that can be used to construct all other logic gates, including AND, OR, and NOT. Including a NAND instruction adds flexibility to the ALU, allowing efficient implementation of complex logical operations.
> ChipVerify. (n.d.). _Universal gates - NAND and NOR_. [https://www.chipverify.com/digital-fundamentals/universal-gates](https://www.chipverify.com/digital-fundamentals/universal-gates)
## Two’s Complement
**Two’s complement** is used to represent signed integers and enables the CPU to perform subtraction and other signed arithmetic operations. It simplifies circuit design and allows for a unified method of handling both positive and negative integers.
> Franklin, T. (2002). _Two's complement representation_. Cornell University. [https://www.cs.cornell.edu/~tomf/notes/cps104/twoscomp.htm](https://www.cs.cornell.edu/~tomf/notes/cps104/twoscomp.htm)

# Phase 2

| Binary code | Opcode | Details        | Operands   | Justification                            |
| ----------- | ------ | -------------- | ---------- | ---------------------------------------- |
| `0000`      | `OR`   | Bitwise OR     | `R1`, `R2` | Binary operation, needs two inputs.      |
| `0001`      | `AND`  | Bitwise AND    | `R1`, `R2` | Binary operation, needs two inputs.      |
| `0010`      | `ADD`  | Addition       | `R1`, `R2` | Common arithmetic, needs two inputs.     |
| `0011`      | `SUB`  | Subtraction    | `R1`, `R2` | Common arithmetic, needs two inputs.     |
| `0100`      | `INC`  | Increment by 1 | `R1`       | Unary operation.                         |
| `0101`      | `DEC`  | Decrement by 1 | `R1`       | Unary operation.                         |
| `0110`      | `NOT`  | Bitwise NOT    | `R1`       | Unary operation.                         |
| `0111`      | `MUL`  | Multiply       | `R1`, `R2` | Common arithmetic, needs two inputs.     |
| `1000`      | `CMP`  | Compare        | `R1`, `R2` | Compare two values.                      |
| `1001`      | `XOR`  | Exclusive OR   | `R1`, `R2` | Binary operation, needs two inputs.      |
| `1010`      | `NAND` | NOT AND        | `R1`, `R2` | Binary operation, needs two inputs.      |
| `1011`      | `TCPL` | 2's Complement | `R1`       | Unary operation. Negates a single input. |
| `1100`      | `SHL`  | Shift Left     | `R1`       | Unary operation.                         |
| `1101`      | `SHR`  | Shift Right    | `R1`       | Unary operation.                         |

# Phase 3
I implemented phase 3 in Logisim by installing a 4-select-bit input multiplexer (making for 16 possible inputs).
I chose a 4 bit selector because I have 14 possible operations (see table in phase 2).
As asked in phase 1, I only implemented the first 9 operations, plus **subtraction**, for a total of 10 implemented operations.
Therefore, while the Opcode selector can take 16 different values, from 0000 to 1111, some of them won't produce a valid output. They are: Multiplication (0111), XOR (1001), NAND (1010) and 2's complement (1011).
# Phase 4

```asm title:"Add until '0'"
; It is assumed the register content is modified by the end of each iteration.
; The 2 registers used are R1 and R2.
; It is assumed the ADD instruction compares the two operands and stores the
;   result in its first operand (R2 in this program).
; It is assumed that CMP compares two operands for equality and that
;   we can then jump elsewhere depending on the comparison results.

start:
	CMP R1, 0x00 ; Check if the register equals 0.
	JE end       ; If the new value to add is 0, jump to the end.
	ADD R2, R1   ; Otherwise, perform the addition, then jump back to the start
	JMP start
end:
	END
```

```asm title:"Shift until all 0s"
; The register to be shifted is R1.
; The second register R2 is used to perform comparisons.
; The AND operation is assumed to store the result in its first operand.
; With this program I try to answer: If the register's content
;   is "11111111" to start with, how do we shift right until
;   the LSB is '0'?

start:
	MOV R2, R1
	AND R2, 0x01 ; Bitwise AND to isolate the LSB
	CMP R2, 0x00 ; Check if the LSB is equal to 0
	JE end       ; If so, our work is done
	SHR R1       ; Otherwise we shift and start the loop over
	JMP start
end:
	END
```
