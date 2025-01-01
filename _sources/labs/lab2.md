# Lab2: Single Cycle CPU

## Introduction

In Lab2, you are tasked with implementing a Single Cycle CPU based on the RISC-V ISA. Upon completing this lab, you should have a deeper understanding of CPU architecture and the RISC-V instruction set.

Don’t panic! This lab is not as difficult as it might seem. To successfully implement the RISC-V CPU, we encourage you to study the functionality of each component and understand how they cooperate. We will also introduce a useful RISC-V simulator, Ripes, which demonstrates the CPU workflow step by step.

## Lab Source Code

The TAs have prepared a template for you. You can follow the template or modify it as needed. However, there are certain elements you should not change:

- Registers
- Instruction Memory
- Data Memory
- CPU I/O interface and register instance names

In the provided source code, we include a simple instruction file, `TEST_INSTRUCTIONS.txt`, containing machine code generated from `TEST_INSTRUCTIONS.asm`.

```{important}
The `rst` signal is active low, which means the module will reset when the `rst` signal is set to zero. You should follow this design in your implementation.
```

## Ripes

[Ripes](https://ripes.me/) is a visual computer architecture simulator and assembly code editor designed for the RISC-V instruction set architecture.

![ripes1](images/lab2/ripes1.png)

As shown in the picture above, you can write your own assembly code, clock the processor, and verify the register values.

You can copy the generated machine code into `TEST_INSTRUCTIONS.txt` in the following format to run your code on your CPU design. Each line in the file should contain 8 bits. A single 4-byte instruction is represented by 4 lines, and the file should end with a new empty line.

```asm
addi t2 zero 20
jalr ra (t2)0
addi t0 zero 1
addi t0 t0 1
addi t0 t0 1
addi t1 zero 100
```

```
00000001
01000000
00000011
10010011
00000000
00000011
10000000
11100111
00000000
00010000
00000010
10010011
00000000
00010010
10000010
10010011
00000000
00010010
10000010
10010011
00000110
01000000
00000011
00010011
```

```{important}
Ensure that you change the settings in Ripes to match the constraints of the register values specified for this lab.
![lab2-ripes2](images/lab2/ripes2.png)
```

## Single Cycle CPU

### Architecture

You can follow the architecture diagram to implement a single-cycle CPU.

![lab2-artitecture](images/lab2/artitecture.jpg)

### Instruction

Implement the following instructions. The RV32I ISA layout is provided below.

- add  
  ![add](images/lab2/add.png)
- addi  
  ![addi](images/lab2/addi.png)
- sub  
  ![sub](images/lab2/sub.png)
- and  
  ![and](images/lab2/and.png)
- andi  
  ![andi](images/lab2/andi.png)
- or  
  ![or](images/lab2/or.png)
- ori  
  ![ori](images/lab2/ori.png)
- slt  
  ![slt](images/lab2/slt.png)
- slti  
  ![slti](images/lab2/slti.png)
- lw  
  ![lw](images/lab2/lw.png)
- sw  
  ![sw](images/lab2/sw.png)
- beq  
  ![beq](images/lab2/beq.png)
- bne
  ![bne](images/lab2/bne.png)
- blt
  ![blt](images/lab2/blt.png)
- bge
  ![bge](images/lab2/bge.png)
- jal  
   `jal` stores `pc+4` in `regs[rd]`, executes `pc = pc + imm << 1`
  ![jal](images/lab2/jal.png)
- jalr  
  `jalr` stores `pc+4` in `regs[rd]`, exectues `pc = regs[rs1] + imm`
  ![jalr](images/lab2/jalr.png)

```{important}
All operations are signed. You must follow this ISA table to implement your instructions:
![lab2-2](images/lab2/ISAtable.png)
```

## Requirements

Implement your RISC-V single-cycle CPU. Your CPU should support the following RISC-V ISA instructions:

- Arithmetic and Logical Operations: `add`, `addi`, `sub`, `and`, `andi`, `or`, `ori`, `slt`, `slti`
- Memory Operations: `lw`, `sw`
- Branch Operations: `beq`, `bne`, `blt`, `bge`
- Jump Operations: `jal`, `jalr`

TAs have prepared a Verilator testbench and some `TEST_INSTRUCTION.txt` files to grade your design. The correctness will be verified by comparing the register values.

```{warning}
Do not modify the register, instruction memory, or CPU I/O interface; otherwise, you will receive 0 points.
```

## Submission

Please submit your source code as a ZIP file to E3. The name of the ZIP file should be `lab2_<student_id>.zip`, and the structure should be as follows:

```
lab2_<student_id>.zip
├── lab2_<student_id>
│   ├── ALU.v
│   ├── ALUCtrl.v
│   ├── Adder.v
│   ├── BranchComp.v
│   ├── Control.v
│   ├── DataMemory.v
│   ├── ImmGen.v
│   ├── InstructionMemory.v
│   ├── Mux2to1.v
│   ├── Mux3to1.v
│   ├── PC.v
│   ├── Register.v
│   ├── ShiftLeftOne.v
│   └── SingleCycleCPU.v
```

You can check if the unzipped file contains the folder structure as described by running:

```bash
unzip lab2_<stduent_id>.zip
tree lab2_<stduent_id>
```

```{warning}
The deadline for submission is x/xx at 23:59. Submissions in an incorrect format will result in a 10-point deduction.
```

## Hints

- Read the textbook first to understand each submodule’s functionality.
- Use waveform debugging to simplify the debugging process.
- Try generating your own RISC-V machine code using Ripes. You can write simple assembly code to verify if your implementation works as expected.

## Reference

- Computer Organization and Design RISC-V Edition, CH4
- [Ripes](https://github.com/mortbopet/Ripes)
- [RISC-V Instruction Set Specifications](https://msyksphinz-self.github.io/riscv-isadoc/html/rvi.html)
