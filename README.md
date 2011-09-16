# Turing Machine Compiler

This project is a purely academic (read: useless) system to get a subset of C to compile to a turing machine. It builds upon some earlier work I've done with turing machine ruleset generation, and on the idea behind a Universal Turing Machine.

## Filetypes

* .turing

    Raw Turing Machine ruleset file. Compressed, machine-readable version.

* .tr0

    Raw Turing Machine ruleset file. Uncompressed, human-readable version.

* .tr1

    Layer-1 Turing Machine Abstraction Language file. Adds, to Layer-0, named states, tapes, and symbols.

* .tr2

    Layer-2 Turing Machine Abstraction Language file. Adds, to Layer-1, a multistate array abstraction and various state transition shorthand.

* .tr3

    Layer-3 Turing Machine Abstraction Language file. Adds, to Layer-2, ruleset "chaining".

* .tape

    Turing Machine Tape file.

* .tasm

    Turing Machine Assembly Language file.

* .tc

    Turing Machine C file.

## Machine Structure

The structure of the turing machine that is to run the compiled C code is specialized to some extent.

The machine has three symbols, despite being an essentially binary machine, with "2" acting primarily as a delimiter.

The machine has five tapes, each with a specific purpose:

* Input Tape

    The input tape is automagically linked to the stdin. Each byte from the stdin is written to the tape (probably bitwise big-endian, for simplified reading) to the right of center. Other than bytes from the stdin and potentially 2s as byte delimiters, the input tape is empty (initialized to all 0s).

* Output Tape

    The output tape is automagically linked to the stdout. As the reading head seeks right on the tape, each complete set of eight bits is written to the stdout.

* Program Tape

    The program tape stores the instructions from an assembled .tasm file. The program tape consists of single-byte instructions, delimited by 2s, with a single 2 at the center of the tape, and the first instruction to the right of it. Instructions are stored with a 3-bit or 4-bit opcode and 5-bit or 4-bit operand, although some instructions may span multiple contiguous bytes. 

* Scratch Tape

    The scratch tape stores partial results within the computation of a single instruction, especially for instructions which move data from one place to another on the working tape. It is not guaranteed to maintain its value between instructions, and it cannot be assumed to hold any specific structure at start of an instruction. 

* Working Tape

    The working tape has a very specific structure customized to the goal of running machine code on a turing machine. It is broken into three parts: condition bit, registers, and stack.

    The center of the tape stores the condition bit: 0, 1, or 2, for negative, positive, or zero respectively. To the right and left of the condition bit are 2s, acting as delimiters.

    To the left of the condition bit, after the delimiting 2, are the registers. There are 32 registers, each 8-bits wide, all delimited by 2s, with a trailing 2 to the far left.

    To the right of the condition bit, again after the delimiting 2, is the stack. At initialization, the stack stores a single 0-byte. The stack is structured as a sequence of groups of 8 1s or 0s, terminated on the right by a single 2. Because seeking within the stack past the first byte is unnecessary, and beyond the scope of the data structure, the non-delimited structure allows the stack to hold an "infinite" amount of data (in the sense that the simulated tape is infinite). Making room for a new byte on the stack simply consists of seeking to the right across the stack, copying each bit one to the right during the seek, overwriting the 2 which terminates the stack, then seeking back to the 2 to the right of the center of the tape, and repeating 7 more times. Popping a byte of the stack is similar.

## Machine Code Instructions (Opcode list)

## Assembly Language Overview


