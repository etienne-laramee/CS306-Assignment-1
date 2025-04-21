# 8-Bit ALU – CS308 Assignment 1
## Overview
This project implements a functional 8-bit ALU using Logisim. It supports 14 core operations on 8-bit operands, forming the foundation of a basic CPU.

## Implemented Instructions
**Phase One – Core Operations**
ADD – Arithmetic addition

INC – Increment

DEC – Decrement

CMP – Comparison (sets Equal, Less Than, Greater Than flags)

NOT – Bitwise NOT

AND – Bitwise AND

OR – Bitwise OR

SHL – Shift Left (Logical)

SHR – Shift Right (Logical)

**Phase One – Additional Instructions**
SUB – Subtraction

XOR – Bitwise XOR

NAND – Bitwise NAND

ASR – Arithmetic Shift Right

NEG – Negation (two’s complement)

Note: 2 additional opcode slots are reserved for branching in future phases.

## How to Use
Open Etienne Laramee CS308 Assignment 1.circ in Logisim Evolution

Use the A and B input pins to provide 8-bit operands

Use the ALU control lines to select operations (e.g., 4-bit opcode)

Observe the result and flags (ZERO, NEG, CARRY, etc.) on output pins

**Author**
Etienne Laramée
CS308 – Computer Architecture
