# Goals, Environment and Tools

> CS:APP, Chapter 1

## Computer System

A Computer System consists of hardware and systems software that work together to run application programs.

## Bits vs Bytes

### Bit

A bit is a 0 or a 1.

### Byte

A *byte* is an 8-bit chunk.

### Source Program / Source File

This is the file containing the language-specific language (for example, the C-code).

#### A Source Program is a sequence of bits

A Source Program is really a sequence of bits, each with a value of 0 1, organized in 8-bit chunks called *bytes*.

Each byte has an integer value that corresponds to some character.

For example, one byte could have the integer value 35 (which corresponds to the character *#*).

Each text line is terminated by the invisible *newline* character `\n` which is represented by the integer value 10.

### Text files vs Binary files

*Text Files* are files that consist exclusively of ASCII characters.

### Different contexts

The same sequence of bytes might represent an integer, a float, a number, a character or a machine instruction depending on the context. For example, with ASCII, we've just decided that the byte with the integer value 10 is a newline and that the byte with the integer value 35 is a *#*.

## A program's journey from Source program to Executable Object Program (binary)

The `gcc` compiler reads a source file and translates it into an executable object file.

Such a translation is performed in a sequence of four phases.

### Four phases

They are:

- Preprocessing
- Compiling
- Assembling
- Linking

### Preprocessing
Here, directives (things that starts with '#') are replaced accordingly.
For example, `#include<stdio.h>` is replaced with the contents of the `stdio.h` file.

The result is typically a new C program with the `.i` suffix.

### Compilation

Here, the compiler (cc1) translates the preprocessed text file into a new text file with a `.s` suffix. This is an **assembly-language** program.

**This describes low-level machine-language instructions in a textual form**.
This is useful because it provides a common output language for different compilers for different high-level languages.

### Assembling

Here, the assembler (as) translates the `.s` file into machine-language instructions. It packages them in a form called `relocatable object program` and stores the result in the object file with a `.o` suffix.

**This is a binary file**. It is by no means humanly readable.

### Linking

Here, the relocatable object program is merged with the relocatable object programs for all of the functions from the standard library that is called within the program. For example, `printf` is part of the standard C library. This must be merged ("linked").

This results in an executable object file (or simply *executable*) that can be loaded into memory and executed by the system.

## Hardware Organization of a System

