# IITK Mini-Mips
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

## Instruction Set Architecture

### Instruction Formats

| Format | Bit Layout | Description |
|--------|------------|-------------|
| **R-Type** | `opcode(6) \| rs(5) \| rt(5) \| rd(5) \| shamt(5) \| funct(6)` | Register operations |
| **I-Type** | `opcode(6) \| rs(5) \| rt(5) \| immediate(16)` | Immediate operations |
| **J-Type** | `opcode(6) \| address(26)` | Jump operations |
| **F-Type** | `opcode(6) \| ftype(5) \| fs(5) \| ft(5) \| fd(5) \| funct(6)` | Floating-point operations |

### Instruction Categories

#### Arithmetic Instructions (18 instructions)
- **Basic Operations**: `add`, `sub`, `addi`, `mul`
- **Unsigned Operations**: `addu`, `subu`, `addiu`
- **Advanced Operations**: `madd`, `maddu` (multiply-accumulate)
- **Bitwise Operations**: `and`, `or`, `xor`, `not`, `andi`, `ori`, `xori`

#### Shift Instructions (4 instructions)
- **Logical Shifts**: `sll`, `srl`
- **Arithmetic Shifts**: `sla`, `sra`

#### Data Transfer Instructions (3 instructions)
- **Memory Operations**: `lw`, `sw`
- **Immediate Load**: `lui`

#### Control Flow Instructions (11 instructions)
- **Conditional Branches**: `beq`, `bne`, `bgt`, `bgte`, `ble`, `bleq`, `bleu`, `bgtu`
- **Unconditional Jumps**: `j`, `jr`, `jal`

#### Comparison Instructions (3 instructions)
- **Set Operations**: `slt`, `slti`, `seq`

#### Floating-Point Instructions (10 instructions)
- **Data Movement**: `mfcl`, `mtc1`, `mov.s`
- **Arithmetic**: `add.s`, `sub.s`
- **Comparisons**: `c.eq.s`, `c.le.s`, `c.lt.s`, `c.ge.s`, `c.gt.s`

## Getting Started

### Prerequisites

- **Assembly Programming Knowledge**: Basic understanding of assembly language
- **Computer Architecture**: Familiarity with processor design concepts
- **Binary/Hexadecimal**: Comfort with number systems and bit manipulation

### Quick Reference

1. **Instruction Lookup**
   ```bash
   # Find instruction details
   grep -i "add " instruction_reference.md
   ```

2. **Opcode Generation**
   ```python
   # Generate machine code for instruction
   python tools/assembler.py "add $t0, $t1, $t2"
   ```

3. **Control Signal Reference**
   ```bash
   # Check control signals for instruction type
   grep -A 5 "R-Type" control_signals.md
   ```

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

## Register Organization

### General-Purpose Registers
| Register | Number | Purpose | Preserved |
|----------|--------|---------|-----------|
| `$zero`  | 0      | Constant 0 | N/A |
| `$at`    | 1      | Assembler temporary | No |
| `$v0-$v1`| 2-3    | Function results | No |
| `$a0-$a3`| 4-7    | Function arguments | No |
| `$t0-$t7`| 8-15   | Temporaries | No |
| `$s0-$s7`| 16-23  | Saved temporaries | Yes |
| `$t8-$t9`| 24-25  | More temporaries | No |
| `$k0-$k1`| 26-27  | OS reserved | N/A |
| `$gp`    | 28     | Global pointer | Yes |
| `$sp`    | 29     | Stack pointer | Yes |
| `$fp`    | 30     | Frame pointer | Yes |
| `$ra`    | 31     | Return address | Yes |

### Special Registers
- **PC**: Program Counter (32-bit)
- **HI/LO**: Multiplication/Division results (32-bit each)
- **CC**: Condition Code (1-bit, for floating-point comparisons)

## Control Unit Specification

### Control Signals
| Signal | Width | Description |
|--------|-------|-------------|
| `reg_dst` | 2 | Register destination select |
| `alu_src` | 1 | ALU source select |
| `mem_to_reg` | 1 | Memory to register select |
| `reg_write` | 1 | Register write enable |
| `mem_read` | 1 | Memory read enable |
| `mem_write` | 1 | Memory write enable |
| `branch_type` | 3 | Branch type selector |
| `jump` | 1 | Jump enable |
| `jump_src` | 1 | Jump source select |
| `alu_op` | 4 | ALU operation select |

### ALU Operations
The ALU supports 16 different operations with 4-bit control codes:

| Code | Operation | Description |
|------|-----------|-------------|
| 0000 | ADD | Addition |
| 0001 | ADDU | Unsigned addition |
| 0010 | SUB | Subtraction |
| 0011 | SUBU | Unsigned subtraction |
| 0100 | AND | Bitwise AND |
| 0101 | OR | Bitwise OR |
| 0110 | NOT | Logical NOT |
| 0111 | XOR | Bitwise XOR |
| 1000 | SLL | Shift left logical |
| 1001 | SRL | Shift right logical |
| 1010 | SRA | Shift right arithmetic |
| 1011 | SLT | Set less than |
| 1100 | SEQ | Set equal |
| 1101 | MUL | Multiply |
| 1110 | MADD | Multiply-add |
| 1111 | MADDU | Multiply-add unsigned |

## Instruction Encoding Examples

### R-Type Example: `add $t0, $t1, $t2`
```
Opcode: 000000 (6 bits)
rs:     01001  (5 bits) = $t1 (9)
rt:     01010  (5 bits) = $t2 (10)
rd:     01000  (5 bits) = $t0 (8)
shamt:  00000  (5 bits) = 0
funct:  100000 (6 bits) = add function
```

### I-Type Example: `addi $t0, $t1, 100`
```
Opcode:    100001 (6 bits)
rs:        01001  (5 bits) = $t1 (9)
rt:        01000  (5 bits) = $t0 (8)
immediate: 0000000001100100 (16 bits) = 100
```

### J-Type Example: `j 0x400000`
```
Opcode:  010001 (6 bits)
address: 00000001000000000000000000 (26 bits) = 0x100000 (word address)
```

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

*This ISA specification is designed for educational purposes and demonstrates fundamental concepts in instruction set architecture and processor design.*
