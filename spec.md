# SmallCPU spec

## Registers:
1. r0 (general purpose) (0b0000)
1. r1 (general purpose) (0b0001)
1. io0 (Input / Output register) (0b0010)
1. io1 (Input / Output register) (0b0011)
1. io2 (Input / Output register) (0b0100)
1. io3 (Input / Output register) (0b0101)
1. pc/ip (program counter / instruction pointer) (0b0110)
1. sptr (Stack pointer) (0b0111)
1. frp (Function Return Pointer) (0b1000)
1. cmpreg (register for storing compare results) (0b1001)

- r0, r1, io0, io1, io2, io3 and pc are all 16-bit registers while cmpreg is 2-bit.
- The following indicies must be the last in the byte (LSB) (0b0011 => 0b00000011, 0b0101 => 0b00000101)

## Architecture requirements:
1. Must support 16-bit registers and values
1. Must support 16-bit multiplication, division, subtraction and addition as well as Logical bitshifts (only one step at a time), AND, OR, XOR, NOT and comparisons (>, <, == and !=)
1. Can use caches, but doesn't have to
1. Must support a stack in its memory
1. Must follow the following table of instructions and byte values

## Instructions:
| Hex  | Decimal | Binary      | Opcode     | Arguments           | Description                                                  |
|------|---------|------------|-----------|-------------------|--------------------------------------------------------------|
| 0x00 | 0       | 0b00000000 | NOP       | NONE              | Doesn't do anything                                          |
| 0x01 | 1       | 0b00000001 | LDI       | REGISTER, VALUE   | Loads an immediate value into the specified register        |
| 0x02 | 2       | 0b00000010 | MOV       | REGISTER A, REGISTER B | Copies the value in register B to register A           |
| 0x03 | 3       | 0b00000011 | STR       | ADDRESS, REGISTER | Stores the value in the register to the specified RAM address |
| 0x04 | 4       | 0b00000100 | LOD       | REGISTER, ADDRESS | Loads the value stored at the address in the register       |
| 0x05 | 5       | 0b00000101 | CMP       | REGISTER A, REGISTER B | Compares the values in the registers and sets cmpreg     |
| 0x06 | 6       | 0b00000110 | JMP       | LABEL             | Makes the program counter jump to that label                |
| 0x07 | 7       | 0b00000111 | JEQ       | LABEL             | Jumps if cmpreg is 0b00 (Equal)                             |
| 0x08 | 8       | 0b00001000 | JNE       | LABEL             | Jumps if cmpreg is 0b01 or 0b10 (Not equal)                |
| 0x09 | 9       | 0b00001001 | JGT       | LABEL             | Jumps if cmpreg is 0b10 (Greater than)                      |
| 0x0A | 10      | 0b00001010 | JLT       | LABEL             | Jumps if cmpreg is 0b01 (Lesser than)                       |
| 0x0B | 11      | 0b00001011 | JLE       | LABEL             | Jumps if cmpreg isn't 0b10 (Lesser than or equal)           |
| 0x0C | 12      | 0b00001100 | JGE       | LABEL             | Jumps if cmpreg isn't 0b01 (Greater than or equal)          |
| 0x0D | 13      | 0b00001101 | ADD       | REGISTER A, REGISTER B | Adds the contents of two registers and stores in A      |
| 0x0E | 14      | 0b00001110 | SUB       | REGISTER A, REGISTER B | Subtracts the contents of two registers and stores in A |
| 0x0F | 15      | 0b00001111 | MUL       | REGISTER A, REGISTER B | Multiplies the contents of two registers and stores in A |
| 0x10 | 16      | 0b00010000 | DIV       | REGISTER A, REGISTER B | Divides the contents of two registers and stores in A   |
| 0x11 | 17      | 0b00010001 | SHL       | REGISTER           | Bitshifts the register one step to the left                |
| 0x12 | 18      | 0b00010010 | SHR       | REGISTER           | Bitshifts the register one step to the right               |
| 0x13 | 19      | 0b00010011 | AND       | REGISTER A, REGISTER B | Bitwise AND between two registers, stored in A         |
| 0x14 | 20      | 0b00010100 | OR        | REGISTER A, REGISTER B | Bitwise OR between two registers, stored in A          |
| 0x15 | 21      | 0b00010101 | XOR / EOR | REGISTER A, REGISTER B | Bitwise XOR between two registers, stored in A        |
| 0x16 | 22      | 0b00010110 | NOT       | REGISTER           | Bitwise NOT of the register, stored in-place             |
| 0x17 | 23      | 0b00010111 | PSH       | REGISTER           | Pushes the register value onto the stack                 |
| 0x18 | 24      | 0b00011000 | POP       | REGISTER           | Pops the top stack value into the register               |
| 0x19 | 25      | 0b00011001 | CLL       | LABEL             | Stores current PC into frp and jumps to the label        |
| 0x1A | 26      | 0b00011010 | RET       | NONE               | Jumps back to the address in frp                          |
| 0x1B | 27      | 0b00011011 | HLT       | NONE               | Halts program execution                                   |
