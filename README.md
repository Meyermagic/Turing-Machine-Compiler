# Turing Machine Compiler

This project is a purely academic (read: useless) system to get a subset of C to compile to a turing machine. It builds upon some earlier work I've done with turing machine ruleset generation, and on the idea behind a Universal Turing Machine.

## Filetypes

* .tr0 / .turing
* .tr1
* .tr2
* .tr3
* .tasm
* .tape

## Machine Structure

The structure of the turing machine that is to run the compiled C code is specialized to some extent.

The machine has three symbols, despite being an essentially binary machine, with "2" acting primarily as a delimiter.

The machine has five tapes, each with a specific purpose:

* Input Tape

    The input tape is automagically linked to the stdin. Each byte from the stdin is written to the tape (probably bitwise big-endian, for simplified reading) to the right of center. Other than bytes from the stdin and potentially 2s as byte delimiters, the input tape is empty (initialized to all 0s).

* Output Tape

    The output tape is automagically linked to the stdout. As the reading head seeks right on the tape, each complete set of eight bits is written to the stdout.

* Program Tape

    The program tape stores the instructions from a fully compiled and assembled .tasm file. The program tape consists of single-byte instructions, delimited by 2s, with a single 2 at the center of the tape, and the first instruction to the right of it. Instructions are stored with a 3-bit or 4-bit opcode and 5-bit or 4-bit operand, although some instructions may span multiple contiguous bytes.

* Scratch Tape
* Working Tape

## Opcode List

## Assembly Language Overview
