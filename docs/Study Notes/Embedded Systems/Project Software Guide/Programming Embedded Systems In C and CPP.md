---
id: Programming Embedded Systems In C and CPP
aliases:
  - Programming Embedded Systems In C and C++
tags: []
---

# Programming Embedded Systems In C and C++

## Chapter 3: Compiling,
Linking and Locating

- The complete process of compilation of code from source to binary
image is done in 3 distinct steps:
    - Compilation: Converting each code file into independent object
    files.
        - Input: Code files
        - Output: Multiple .o files (object files)
    - Linker: Merge all the object files generated in last step into a
    single object file with relocatable program. i.e. Memory positions of
    the code is relative at this point in the object file.
        - Input: Multiple .o files
        - Ouput: Single Merged .o file with the memory location relative to
        each other
    - Locator[1](about:blank#fn1): The relocatable object file is
    assigned exact location based on the starting address and size of
    different types of memory for the system
        - Input: Relocatable Object File
        - Output: Mapped Binary File ready to be uploaded to the embedded
        device

### Compiling

The job of a compiler is mainly to translate the programs written in
human readable language into an equivalent set of opcodes for a
particular processor.

**Difference between Compiler and Assembler:** -
Compiler: A bit complex set of conversion which convert human readable
code into opcodes in binary. - Assembler: A simpler form of conversion
which converts the assembly format of code into opcode in binary. (Much
simpler to convert that a compiler).

**Cross Compiler**: A compiler that runs of one platform
and compiles code for another platform. This is often used in Embedded
Systems because the processing power on Embedded Systems in too small to
have space for both a compiler and the program that it needs to run.

The *GNU C/C++ Compiler (gcc)* and *assembler (as)* can
be configured as either native compiler or cross-compiler.

**Object File (.o file)** - The object file can be
thought of as a very large, flexible data structure. The structure of
the file is usually defined by a standard format like the
***Common Object File Format (COFF)*** or
***Extended Linker Format (ELF)***. - Most
Unix-based platform support standards like *.coff* or
*.elf* which is supported by *gcc*. But there are some
platforms which have their own proprietary formats. - The object file
generated from the compiler is incomplete. Which means that if you have
some definitions of variables or function that don’t exist in current
file, a relative term is placed on its place. This makes the object file
incomplete at this moment.

**How the object file is formatted** -
**.text**: Contains the basic code block. Instructions for
code flow. - **.data**: Contains all the initialized global
variables with their values. - **.bss**: Contains all the
uninitialized global variables.

### Linking

The main task of the linker is to merge the individual object files
generated from the compiler into a single object file which has all the
memory location in relative spaces.

The variables of the object file which were not linked are done in
this stage. The non-linked variables are replaced with the correct
address/references and the final code generated is an object file where
all the variables are reference in relative to a memory position that is
still not fed in to finalize the code.

The *GNU Linker (ld)* runs of all the same host platform as
the GNU Compiler. It is a command line tool that takes the name of all
object files to be linked together as arguments. For embedded systems,
there is a special file linked along with all called as
***starter code***. This file contains the code
which tells the basic steps the microcontroller needs to go through
before starting the code inside *main* function.

> A company called Cygnus has created a free library version
called newlib which is equivalent to Standard
C Library for embedded systems.
> 

### Locating

The task of a locator is to convert the relative position from the
object file of linker into a file with exact specifications of the
memory start addresses and size of memory.

It looks something like this:

```cpp
MEMORY
{ram : ORIGIN = 0x00000, LENGTH = 512K        rom : ORIGIN = 0x80000, LENGTH = 512K}SECTIONS
{  data ram :                /* Initialized data. */  {    _DataStart = . ;    *(.data)      _DataEnd = . ;  } >rom
bss :               /* Initialized data. */  {    _BssStart = . ;    *(.bss)      _BssEnd = . ;  }  _BottomOfHeap = . ;               /* The heap starts here. */  _TopOfStack = 0x80000 ;           /* The stack ends here. */  text rom :                        /* The actual instructions. */  {    *(.text)  }}
```

---

1. For a regular system (i.e. Computers), the Locator is
used during runtime. i.e. Memory allocation happens only when you run
the program to save memory. Whereas, in embedded systems, since the task
of the system is specific, it is preallocated.[↩︎](about:blank#fnref1)
