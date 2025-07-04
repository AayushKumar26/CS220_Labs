# IITK Mini-MIPS

## Project Overview

This Mini MIPS processor is developed as part of the CS220 (Introduction to Computer Organisation) curriculum at IIT Kanpur under the guidance of [Prof. Debapriya Basu Roy](https://dbroy24.wixsite.com/research/), Assistant Professor in CSE Dept. at IIT Kanpur. This document provides comprehensive coverage of instruction formats, opcodes, function codes, and control signals required for implementing a complete MIPS-like processor.

## Objectives

- **Complete ISA Specification**: Define all supported instruction types with precise encoding
- **Instruction Format Reference**: Provide clear documentation of R-type, I-type, J-type, and F-type formats
- **Control Signal Mapping**: Detailed control unit design specifications
- **Register Architecture**: Complete register file organization and naming conventions
- **ALU Operations**: Comprehensive ALU control and operation codes
- **Assembly Programming**: Enable efficient assembly language programming and machine code generation

## Features

- **70+ Instructions**: Complete instruction set covering arithmetic, logical, memory, branch, and floating-point operations
- **Multiple Instruction Formats**: 
  - **R-Type**: Register-based operations (`add`, `sub`, `and`, `or`)
  - **I-Type**: Immediate and memory operations (`addi`, `lw`, `sw`, `beq`)
  - **J-Type**: Jump operations (`j`, `jal`)
  - **F-Type**: Floating-point operations (`add.s`, `sub.s`, `c.eq.s`)
- **Comprehensive Register Set**: 32 general-purpose registers with standard MIPS naming
- **Advanced Features**: Multiply-accumulate, floating-point comparisons, and conditional moves
- **Control Unit Support**: Complete control signal specification for processor implementation

---

## Instruction Format

| Format | Description |
|--------|-------------|
| R-type | opcode(6) \| rs(5) \| rt(5) \| rd(5) \| shamt(5) \| funct(6) |
| I-type | opcode(6) \| rs(5) \| rt(5) \| immediate(16) |
| J-type | opcode(6) \| address(26) |
| F-type | opcode(6) \| ftype(5) \| fs(5) \| ft(5) \| fd(5) \| funct(6) |

---

## Arithmetic Instructions (R-type unless specified)

| Instruction | Type | Opcode | Funct | Description |
|------------|------|--------|-------|-------------|
| add        | R    | 000000 | 100000 | rd = rs + rt |
| sub        | R    | 000000 | 100010 | rd = rs - rt |
| addu       | R    | 000000 | 100001 | rd = rs + rt (unsigned) |
| subu       | R    | 000000 | 100011 | rd = rs - rt (unsigned) |
| addi       | I    | 100001 | -     | rt = rs + immediate |
| addiu      | I    | 001010 | -     | rt = rs + immediate (unsigned) |
| madd       | R    | 000000 | 101100 | HI, LO += rs * rt |
| maddu      | R    | 000000 | 101101 | HI, LO += rs * rt (unsigned) |
| mul        | R    | 000000 | 011000 | HI, LO = rs * rt |
| and        | R    | 000000 | 100100 | rd = rs & rt |
| or         | R    | 000000 | 100101 | rd = rs \| rt |
| andi       | I    | 001100 | -     | rt = rs & immediate |
| ori        | I    | 001101 | -     | rt = rs \| immediate |
| not        | R    | 000000 | 110000 | rd = ~rs |
| xori       | I    | 001110 | -     | rt = rs ^ immediate |
| xor        | R    | 000000 | 100110 | rd = rs ^ rt |

---

## Shift Instructions (R-type)

| Instruction | Type | Opcode | Funct | Description |
|------------|------|--------|-------|-------------|
| sll        | R    | 000000 | 000000 | rd = rt << shamt |
| srl        | R    | 000000 | 000010 | rd = rt >> shamt |
| sla        | R    | 000000 | 000000 | rd = rt <<< shamt (same as sll) |
| sra        | R    | 000000 | 000011 | rd = rt >>> shamt |

---

## Data Transfer Instructions

| Instruction | Type | Opcode | Funct | Description |
|------------|------|--------|-------|-------------|
| lw         | I    | 100011 | -     | rt = Mem[rs + immediate] |
| sw         | I    | 101011 | -     | Mem[rs + immediate] = rt |
| lui        | I    | 001111 | -     | rt = immediate << 16 |

---

## Conditional Branch Instructions (I-type)

| Instruction | Type | Opcode | Description |
|-------------|------|--------|-------------|
| beq         | I    | 000100 | if (rs == rt) branch |
| bne         | I    | 000101 | if (rs != rt) branch |
| bgt         | I    | 000111 | if (rs > rt) branch |
| bgte        | I    | 000110 | if (rs >= rt) branch |
| ble         | I    | 000001 | if (rs <= rt) branch |
| bleq        | I    | 000001 | if (rs <= rt) branch |
| bleu        | I    | 000001 | if (rs < rt) branch (unsigned) |
| bgtu        | I    | 000111 | if (rs > rt) branch (unsigned) |

---

## Unconditional Branch Instructions

| Instruction | Type | Opcode | Description |
|-------------|------|--------|-------------|
| j           | J    | 000010 | Jump to address |
| jr          | R    | 000000 | funct: 001000 (Jump to rs) |
| jal         | J    | 000011 | Jump and link (ra = PC + 4) |

---

## Comparison Instructions

| Instruction | Type | Opcode | Funct | Description |
|-------------|------|--------|-------|-------------|
| slt         | R    | 000000 | 101010 | rd = (rs < rt) ? 1 : 0 |
| slti        | I    | 001010 | -     | rt = (rs < immediate) ? 1 : 0 |
| seq         | I    | 001000 | -     | rt = (rs == immediate) ? 1 : 0 |

---

## Floating Point Instructions

| Instruction | Type | Opcode | Funct | Description |
|-------------|------|--------|-------|-------------|
| mfcl        | R    | 010001 | 000000 | rt = f0 |
| mtc1        | R    | 010001 | 000001 | f0 = rt |
| add.s       | F    | 010001 | 000010 | fd = fs + ft |
| sub.s       | F    | 010001 | 000011 | fd = fs - ft |
| c.eq.s      | F    | 010001 | 110010 | cc = (fs == ft) |
| c.le.s      | F    | 010001 | 110001 | cc = (fs <= ft) |
| c.lt.s      | F    | 010001 | 110000 | cc = (fs < ft) |
| c.ge.s      | F    | 010001 | 110011 | cc = (fs >= ft) |
| c.gt.s      | F    | 010001 | 110100 | cc = (fs > ft) |
| mov.s       | F    | 010001 | 000100 | fd = fs (if cc set) |

---

## Register Mapping

| Register Name | Number | Description         | Preserved |
|---------------|--------|---------------------|-----------|
| $zero         | 0      | Constant 0          | N/A       |
| $at           | 1      | Assembler temporary | No        |
| $v0–$v1       | 2–3    | Function results    | No        |
| $a0–$a3       | 4–7    | Function arguments  | No        |
| $t0–$t7       | 8–15   | Temporaries         | No        |
| $s0–$s7       | 16–23  | Saved temporaries   | Yes       |
| $t8–$t9       | 24–25  | More temporaries    | No        |
| $k0–$k1       | 26–27  | OS reserved         | N/A       |
| $gp           | 28     | Global pointer      | Yes       |
| $sp           | 29     | Stack pointer       | Yes       |
| $fp           | 30     | Frame pointer       | Yes       |
| $ra           | 31     | Return address      | Yes       |

---

## R-Type Function Codes

| Funct   | Instruction | Description              |
|---------|-------------|--------------------------|
| 100000  | add         | rd = rs + rt             |
| 100010  | sub         | rd = rs - rt             |
| 100001  | addu        | rd = rs + rt (unsigned)  |
| 100011  | subu        | rd = rs - rt (unsigned)  |
| 100100  | and         | rd = rs & rt             |
| 100101  | or          | rd = rs \| rt            |
| 100110  | xor         | rd = rs ^ rt             |
| 110000  | not         | rd = ~rs                 |
| 000000  | sll         | rd = rt << shamt         |
| 000010  | srl         | rd = rt >> shamt         |
| 000011  | sra         | rd = rt >>> shamt        |
| 101010  | slt         | rd = (rs < rt) ? 1 : 0   |
| 101100  | madd        | HI, LO += rs * rt        |
| 011000  | mul         | HI, LO = rs * rt         |
| 001000  | jr          | PC = rs                  |

---

## Control Signal Summary

| Signal        | Width | Meaning                                           |
|---------------|-------|---------------------------------------------------|
| reg_dst       | 2     | 2 = $ra(for JAL), 1 = rd (R-type), 0 = rt (I-type) |
| alu_src       | 1     | 1 = immediate, 0 = register                      |
| mem_to_reg    | 1     | 1 = data from memory, 0 = ALU result             |
| reg_write     | 1     | 1 = enable register write                         |
| mem_read      | 1     | 1 = enable memory read                            |
| mem_write     | 1     | 1 = enable memory write                           |
| branch_type   | 3     | 3-bit code for type of branching                  |
| jump          | 1     | 1 = enable jump                                   |
| jump_src      | 1     | 1 = jump address, 0 = read_data1(for jr)         |
| alu_op        | 4     | ALU control bits (depends on opcode)             |

---

## ALUOp Decoding Table

| ALUOp Code | Operation | Used For                              |
|------------|-----------|---------------------------------------|
| 0          | RTYPE     | R-type instructions (opcode = 0)      |
| 1          | AND       | and                                   |
| 2          | ADDI      | addi                                  |
| 3          | SLT       | slti, bgt, ble, bgte, bleq            |
| 4          | SLTU      | sltu, bgtu, bleu                      |
| 6          | AND       | andi                                  |
| 7          | OR        | ori                                   |
| 8          | XOR       | xori                                  |
| 10         | SEQ       | seq                                   |
| 11         | J         | j                                     |
| 12         | JAL       | jal                                   |
| 15         | RTYPE     | R-type instructions (opcode = 0)      |

---

## ALU Control Decoding Table

| ALU_control Code | Mnemonic   | Operation                      | Source (ALUOp / funct) |
|------------------|------------|--------------------------------|------------------------|
| 0                | ALU_ADD    | ADD                            | funct = 100000         |
| 1                | ALU_ADDU   | Unsigned Addition              | funct = 100001         |
| 2                | ALU_SUB    | Subtraction                    | funct = 100010         |
| 3                | ALU_SUBU   | Unsigned subtraction           | funct = 100011         |
| 4                | ALU_AND    | Bitwise and                    | funct = 100100         |
| 5                | ALU_OR     | Bitwise or                     | funct = 100101         |
| 6                | ALU_NOT    | Logical not                    | funct = 110000         |
| 7                | ALU_XOR    | Bitwise xor                    | funct = 100110         |
| 8                | ALU_SLL    | Logical Shift left             | funct = 000000         |
| 9                | ALU_SRL    | Logical Shift Right            | funct = 000010         |
| 10               | ALU_SRA    | Shift right arithmetic         | funct = 000011         |
| 11               | ALU_SLT    | Set less than                  | funct = 101010         |
| 12               | ALU_SEQ    | Set on equal                   | ALUOp = 10             |
| 13               | ALU_MUL    | Multiplication                 | funct = 011000         |
| 14               | ALU_MADD   | Multiply-Add Signed            | funct = 101100         |
| 15               | ALU_MADDU  | Multiply-Add Unsigned          | funct = 101101         |

---

## Instruction Encoding Examples

### R-Type Example: `add $t0, $t1, $t2`
```
Instruction: add $t0, $t1, $t2
Binary Encoding:
Opcode: 000000 (6 bits)
rs:     01001  (5 bits) = $t1 (9)
rt:     01010  (5 bits) = $t2 (10)
rd:     01000  (5 bits) = $t0 (8)
shamt:  00000  (5 bits) = 0
funct:  100000 (6 bits) = add function

32-bit: 00000001001010100100000000100000
Hex:    0x012A4020
```

### I-Type Example: `addi $t0, $t1, 100`
```
Instruction: addi $t0, $t1, 100
Binary Encoding:
Opcode:    100001 (6 bits)
rs:        01001  (5 bits) = $t1 (9)
rt:        01000  (5 bits) = $t0 (8)
immediate: 0000000001100100 (16 bits) = 100

32-bit: 10000101001010000000000001100100
Hex:    0x21280064
```

### J-Type Example: `j 0x400000`
```
Instruction: j 0x400000
Binary Encoding:
Opcode:  000010 (6 bits)
address: 00000001000000000000000000 (26 bits) = 0x100000 (word address)

32-bit: 00001000000001000000000000000000
Hex:    0x08100000
```

---

## Usage Examples

### Basic Arithmetic
```assembly
# Add two registers
add $t0, $t1, $t2      # $t0 = $t1 + $t2

# Add immediate value
addi $t0, $t1, 100     # $t0 = $t1 + 100

# Multiply two values
mul $t0, $t1, $t2      # HI, LO = $t1 * $t2
```

### Memory Operations
```assembly
# Load word from memory
lw $t0, 0($t1)         # $t0 = Memory[$t1 + 0]

# Store word to memory
sw $t0, 4($t1)         # Memory[$t1 + 4] = $t0

# Load upper immediate
lui $t0, 0x1000        # $t0 = 0x1000 << 16
```

### Control Flow
```assembly
# Conditional branch
beq $t0, $t1, label    # if ($t0 == $t1) goto label

# Unconditional jump
j target_address       # goto target_address

# Jump and link
jal function_start     # $ra = PC + 4; goto function_start
```

### Floating-Point Operations
```assembly
# Add single-precision floats
add.s $f0, $f1, $f2    # $f0 = $f1 + $f2

# Compare floats
c.eq.s $f0, $f1        # cc = ($f0 == $f1)

# Move based on condition
mov.s $f0, $f1         # if (cc) $f0 = $f1
```

---

## Performance Considerations

### Instruction Timing
- **R-Type**: 1 cycle (single-cycle implementation)
- **I-Type**: 1 cycle (load/store may require additional memory access time)
- **J-Type**: 1 cycle
- **F-Type**: 1-4 cycles (depending on floating-point complexity)

### Memory Organization
- **Word-aligned**: All addresses must be multiples of 4
- **Big-endian**: Most significant byte at lowest address
- **Separate spaces**: Instruction and data memory are separate

---

## Testing and Validation

### Instruction Testing
```assembly
# Test arithmetic operations
.text
main:
    addi $t0, $zero, 10    # $t0 = 10
    addi $t1, $zero, 5     # $t1 = 5
    add $t2, $t0, $t1      # $t2 = 15
    sub $t3, $t0, $t1      # $t3 = 5
    mul $t4, $t0, $t1      # HI:LO = 50
```

### Branch Testing
```assembly
# Test branch operations
.text
test_branch:
    addi $t0, $zero, 10
    addi $t1, $zero, 5
    beq $t0, $t1, equal    # Should not branch
    bgt $t0, $t1, greater  # Should branch
equal:
    # This should not execute
greater:
    # This should execute
```

---
