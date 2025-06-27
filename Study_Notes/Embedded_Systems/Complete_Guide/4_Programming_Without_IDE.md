---
id: embedded
aliases:
  - 4. Programming without IDE
tags:
---


# 4. Programming without IDE

Makefile + Toolchain based Compilation

> Guide: 

> - Microcontroller Programming without IDE (Makefile + Toolchain) 
> - Build and Flash your Microcontroller Project from the Command-line with a Makefile

**Why would you even need a custom toolchain?** 

- Proprietary Toolchain can be expensive. 
- You cannot automate a lot of processes in propriertary toolchain as they need to be done using mouse input which might be an issue when automated testing needs to be configured on cloud or local desktop. 
- Support can be limited due to expenses or permission to user to edit it. 
- Less control over compilation due to Proprietary code.

*:notebook: For our usage, we are working on MSP430 here. So we will be using the toolchains related to it. The open source alternative that we are using is **gcc**.*

## Fetch appropriate toolchain

1. Download the gcc toolchain for MSP430 from website [here](https://www.ti.com/tool/MSP430-GCC-OPENSOURCE). (I suggest to download the version with support files included. This will avoid the hassel of setting up everything personally)
2. I tend to use multiple microcontroller so we want to seperate toolchains from each other to avoid confusion and have an easy access. So I tend to store my toolchains in `~/dev/tools/`
folder.
    
    The zip file is downloaded in `~/Downloads/`. So:
    
    1. Unzip the file
    2. Create a folder to store the toolchains if non-existent and move
    this toolchain there.
        
        ```bash
            mkdir -p ~/dev/tools
        ```
        
    3. Open the folder and run the file using
        
        ```bash
        sudo ./*.run
        ```
        
        Use the `/dev/tools` as the install directory for easy managing.
        
    4. Add the toolchain to path. 
    `bash cd ~/dev/tools/msp430-gcc/bin/ export PATH="$PATH:$(pwd)"`
    Doing this will add the MSP430 GCC Compiler to your path which can be called easily using `msp430-elf-gcc` command from anywhere in the system so you don’t need to call the file from the actual path everytime.

## Configure the toolchain

### Basic Compilation

If you try to run the code:

```bash
msp430-elf-gcc main.c
```

You will recieve the following error:

```bash
main.c:53:10: fatal error: msp430.h: No such file or directory
   53 | #include <msp430.h>      |          ^~~~~~~~~~compilation terminated.
```

This is due to the fact the compiler does not know currently where the headers for the chip is located. We need to provide the compiler with those details. You can find `msp430.h` using:

### Finding the Necessary Header Files

```bash
cd ~/dev/tools/msp430-gcc
find -name "msp430.h"
```

This will output the relative location of the file which in this case is:

```bash
./include/msp430.h
```

Go to the folder where msp430 lies and get the path of the file using:

```bash
cd include
pwd
```

This will output the absolute path of `msp430.h`:

```bash
/home/ws/dev/tools/msp430-gcc/include
```

Copy this path and go back to the folder which contained `main.c` and use the `-I` include flag to include this folder for compilation:

```bash
msp430-elf-gcc -I /home/ws/dev/tools/msp430-gcc/include main.c
```

This will generate a new set of errors:

```bash
msp430-elf-gcc -I /home/ws/dev/tools/msp430-gcc/include main.c
main.c: In function 'main':
main.c:58:5: error: 'WDTCTL' undeclared (first use in this function)   58 |     WDTCTL = WDTPW + WDTHOLD;                 // Stop watchdog timer
      |     ^~~~~~main.c:58:5: note: each undeclared identifier is reported only once for each function it appears in
main.c:58:14: error: 'WDTPW' undeclared (first use in this function)   58 |     WDTCTL = WDTPW + WDTHOLD;                 // Stop watchdog timer
      |              ^~~~~main.c:58:22: error: 'WDTHOLD' undeclared (first use in this function)   58 |     WDTCTL = WDTPW + WDTHOLD;                 // Stop watchdog timer
      |                      ^~~~~~~main.c:59:5: error: 'P1DIR' undeclared (first use in this function)   59 |     P1DIR |= 0x01;                            // Set P1.0 to output direction
      |     ^~~~~main.c:63:9: error: 'P1OUT' undeclared (first use in this function)   63 |         P1OUT ^= 0x01;                        // Toggle P1.0 using exclusive-OR
      |         ^~~~~
```

This are the set of declaration which are missing. This issue occurs because when manufacturer creates a header file, they create it for a large number of variant. MSP430 has more than 1 variant with different internal structure. Creating seperate headers for each would be a nightmare to manage. So, the developers used macros to segregate specific versions of mcu from a single header file.

### Specify the MCU Version

Solution: 

- The version of mcu can be specified either by adding a `#define <mcu-version>` in the msp430.h header file which might create an issue of altering this header for all the other firmwares using this header. 
- Include the mcu version flag `-mmcu` in the compilation command using

```bash
msp430-elf-gcc -mmcu="msp430g2553" -I /home/ws/dev/tools/msp430-gcc/include main.c
```

This will solve the missing header issue but will present you with another error similar to this:

```bash
msp430-elf-gcc -mmcu="msp430g2553" -I /home/ws/dev/tools/msp430-gcc/include main.c
/home/ws/dev/tools/msp430-gcc/bin/../lib/gcc/msp430-elf/9.3.1/../../../../msp430-elf/bin/ld: cannot open linker script file msp430g2553.ld: No such file or directory
collect2: error: ld returned 1 exit status
```

### Finding the Linker Script

This issue occurs because the compiler though the command says which linker script to use, it does not tell it where the linker script is located. The linker script can be found in the same directory as `msp430.h` and will be named specific to the mcu version. For us, it is named: `msp430g2553.ld`

Specify the location of linker using `-L` flag:

```bash
msp430-elf-gcc -mmcu="msp430g2553" -I /home/ws/dev/tools/msp430-gcc/include -L /home/ws/dev/tools/msp430-gcc/include main.c
```

All the work done till now is done manually. Which means you need to perform this process everytime you need to compile the file which can be tiresome. We can automate this using **Makefile**.

## Makefile

### Creating a Basic Makefile

Makefile is a file which contains instructions on how to compile a file. It is useful for following reasons: 

- Avoids the hassle to manually type the command for compilation everytime. 
- Complex commands can be seperated into individual smaller commands which can help you to keep track of the process. 
- Avoid the hassle to compile each file everytime by keeping track of files that are not changed which results to quicker compilation.

A format of Makefile looks somewhat like this shown below: 

- Target: It is the name of the process you want to execute. Can be `clean` if you want to remove all compiled files or `debug` if you want to compile for debugging. 
- Prerequisite: This are the basic requirements that need to meet before executing the command. This can either be any other process that needs to be completed within the makefile or any other file that needs to exist for the process to complete. 
- Recipe: The set of command that will be passed into the actual command line when you call a target. 
- Rule: It is a complete set of Target, Prerequisite and Recipe for a command to execute.

![https://www.notion.so/Custom-Makefile/Makefile-Intro.png](https://www.notion.so/Custom-Makefile/Makefile-Intro.png)

Makefile Format

A sample Makefile can look like this:

```
# Directories
MSP430_ROOT_DIR = ~/dev/tools/msp430-gcc
MSP430_HEADER_DIR = $(MSP430_ROOT_DIR)/include/
MSP430_LINKER_DIR = $(MSP430_ROOT_DIR)/include/

# Compiler
CC = $(MSP430_ROOT_DIR)/bin/msp430-elf-gcc

# Base Flags
MCU_FLAG = -mmcu=msp430g2553
WARNING_FLAGS = -Wall -Wshadow -Werror -Wextra
COMPILATION_FLAGS = $(WARNING_FLAGS) -Og -g
DEBUG_FLAGS =
RELEASE_FLAGS =

# Target
TARGET = blink.out

blink: main.c led.c
        $(CC) $(MCU_FLAG) -I $(MSP430_HEADER_DIR) -L $(MSP430_LINKER_DIR) $(COMPILATION_FLAGS) main.c led.c -o $(TARGET)

clean:
        rm -rf *.out
```

The variables in the start make it easier to manage paths and directories to be reused. This helps making recipes easier to write and handle by reducing the chance of typing error.

### Improvement by seperating tags and Flags

Makefile can be improved in the following way by breaking down variables into smaller parts. This can help you to make variables a bit more specific which can help you to expand the functionality of Makefile.

```
# Directories
MSP430_ROOT_DIR = /home/ws/dev/tools/msp430-gcc
MSP430_INCLUDE_DIR = $(MSP430_ROOT_DIR)/include/
MSP430_LINKER_DIR = $(MSP430_ROOT_DIR)/include/
INCLUDE_DIR = $(MSP430_INCLUDE_DIR)
LIB_DIR = $(MSP430_LINKER_DIR)

# Compiler
CC = $(MSP430_ROOT_DIR)/bin/msp430-elf-gcc

# Base Flags
MCU_FLAG = msp430g2553
WARNING_FLAGS = -Wall -Wshadow -Werror -Wextra
COMPILER_FLAGS = $(WARNING_FLAGS) $(addprefix -I, $(INCLUDE_DIR))
LINKER_FLAGS = -mmcu=$(MCU_FLAG) $(addprefix -L, $(LIB_DIR))
DEBUG_FLAGS = -Og -g
RELEASE_FLAGS = -O3

# Target File
TARGET = blink

$(TARGET): main.c led.c
        $(CC) $(COMPILER_FLAGS) $(LINKER_FLAGS) main.c led.c -o $(TARGET)

clean:
        rm -rf *.out $(TARGET)
```

### Improvement by seperating Compilation and Linking Stage

If you run the Makefile above, it will compile all .c files everytime the code is being build. Though it may not be an issue with small projects, as the project grows with multiple files, the compilation time will increase. If you are building modular code, then you don’t need to compile every file everytime since most of them will remain unchanged. You can do this but modifying the Makefile like this:

```
# Directories
MSP430_ROOT_DIR = /home/ws/dev/tools/msp430-gcc
MSP430_INCLUDE_DIR = $(MSP430_ROOT_DIR)/include/
MSP430_LINKER_DIR = $(MSP430_ROOT_DIR)/include/
INCLUDE_DIR = $(MSP430_INCLUDE_DIR)
LIB_DIR = $(MSP430_LINKER_DIR)

# Compiler
CC = $(MSP430_ROOT_DIR)/bin/msp430-elf-gcc

# Base Flags
MCU_FLAG = msp430g2553
WARNING_FLAGS = -Wall -Wshadow -Werror -Wextra
COMPILER_FLAGS = -mmcu=$(MCU_FLAG) $(WARNING_FLAGS) $(addprefix -I, $(INCLUDE_DIR))
LINKER_FLAGS = -mmcu=$(MCU_FLAG) $(addprefix -L, $(LIB_DIR))
DEBUG_FLAGS = -Og -g
RELEASE_FLAGS = -O3

# Target File
TARGET = blink

# Build
$(TARGET): main.o led.o
        $(CC) $(LINKER_FLAGS) main.o led.o -o $(TARGET)

# Compiling
main.o: main.c
        $(CC) $(COMPILER_FLAGS) -c main.c -o main.o

led.o: led.c
        $(CC) $(COMPILER_FLAGS) -c led.c -o led.o

clean:
        rm -rf *.out $(TARGET)
```

### Improvement by reducing Repetition

You might have noticed that we need to rewrite identical code to compile `main.c` and `led.c`. This is cumbersome when there are multiple files. To automate this process, we use `Automatic variables` available for makefile which can do this repetitive task for us.

- `$^` indicates the input file. The file that is needed to be processed with the name provided in `%.c`.
- `$@` indicates the output file. The file will be generated with the name provided in `%.o`.

```
# Build
$(TARGET): main.o led.o
        $(CC) $(LINKER_FLAGS) $^ -o $@

# Compiling
%.o: %.c
        $(CC) $(COMPILER_FLAGS) -c $^ -o $@
```

### Improvement by Improving File Organization

If you take a look in the project directory, you will notice that the `.o` files are mixed with the project files. This makes file navigation and management difficult. This can be improved by moving the build files into its own folder:

- `mkdir` creates a new directory. `@` in front of it supresses the output in terminal and `p` allows for making a multi-depth folder.

```
# Directories
MSP430_ROOT_DIR = /home/ws/dev/tools/msp430-gcc
MSP430_INCLUDE_DIR = $(MSP430_ROOT_DIR)/include/
MSP430_LINKER_DIR = $(MSP430_ROOT_DIR)/include/
INCLUDE_DIR = $(MSP430_INCLUDE_DIR)
LIB_DIR = $(MSP430_LINKER_DIR)
BUILD_DIR = build
OBJ_DIR = $(BUILD_DIR)/obj
BIN_DIR = $(BUILD_DIR)/bin

# Compiler
CC = $(MSP430_ROOT_DIR)/bin/msp430-elf-gcc

# Base Flags
MCU_FLAG = msp430g2553
WARNING_FLAGS = -Wall -Wshadow -Werror -Wextra
COMPILER_FLAGS = -mmcu=$(MCU_FLAG) $(WARNING_FLAGS) $(addprefix -I, $(INCLUDE_DIR))
LINKER_FLAGS = -mmcu=$(MCU_FLAG) $(addprefix -L, $(LIB_DIR))
DEBUG_FLAGS = -Og -g
RELEASE_FLAGS = -O3

# Target File
TARGET = $(BIN_DIR)/blink

SOURCES = main.c\
            led.c
OBJECTS = $(OBJ_DIR)/main.o\
                  $(OBJ_DIR)/led.o

# Build
$(TARGET): $(OBJECTS)
        @mkdir -p $(dir $@)
        $(CC) $(LINKER_FLAGS) $^ -o $@

# Compiling
$(OBJ_DIR)/%.o: %.c
        @mkdir -p $(dir $@)
        $(CC) $(COMPILER_FLAGS) -c $^ -o $@

clean:
        rm -rf $(BUILD_DIR) $(TARGET)
```

This would improve the hierarchy and make the files in following order:

```
.
├── blink
├── build
│   ├── bin
│   │   └── blink
│   └── obj
│       ├── led.o
│       └── main.o
├── Debug
│   ├── BlinkLED_MSP-EXP430G2ET_linkInfo.xml
│   ├── BlinkLED_MSP-EXP430G2ET.map
│   ├── BlinkLED_MSP-EXP430G2ET.out
│   ├── BlinkLED_MSP-EXP430G2ET.txt
│   ├── ccsObjs.opt
│   ├── main.d
│   ├── main.obj
│   ├── makefile
│   ├── objects.mk
│   ├── sources.mk
│   ├── subdir_rules.mk
│   └── subdir_vars.mk
├── led.c
├── led.h
├── lnk_msp430g2553.cmd
├── main.c
├── Makefile
└── targetConfigs
    ├── MSP430G2553.ccxml
    └── readme.txt
```

### Improvement by Reducing File Repitition

You might have noticed that everytime we need to add a new .c file, we need to make sure that we add it in both `SOURCES` and `OBJECTS` variable. To avoid repetition, we can add a substitution function there which can automatically make new `OBJECTS` variables when new `SOURCES` are added. This can be done by

```
SOURCES = main.c\
                  led.c
OBJECT_NAMES = $(SOURCES:.c=.o)

OBJECTS = $(patsubst %,$(OBJ_DIR)/%,$(OBJECT_NAMES))
```

- `OBJECT_NAMES = $(SOURCES:.c=.o)` stores the names for the .o files by looking at the list of files available in the `SOURCES` variable with .c extension.
- `OBJECTS = $(patsubst %,$(OBJ_DIR)/%,$(OBJECT_NAMES))` creates a path substitution where it looks for files in `OBJECT_NAMES` variables and places them in `OBJ_DIR` folder.

### Improvement by improving safety

While writing code, you might come across a situation where you might
try to create a file by name maybe `clean.c`, if you run
`make clean`, it will throw an error as the Makefile will
assume you are trying to compile the `clean.c` file rather
than clearing the compilation files. This can be avoided by adding [`Phonies`](https://www.gnu.org/software/make/manual/html_node/Phony-Targets.html).
They are name to recipe that can be executed when made a request. They
can be specified as follows:

```
# Phonies
.PHONY: all clean

all: $(TARGET)

clean:
        rm -rf $(BUILD_DIR) $(TARGET)
```

### Improvement on Flashing MCU

You can create a Makefile recipe to flash the MCU as follows:

```
# Directories

## Build Directories
MSP430_ROOT_DIR = /home/ws/dev/tools/msp430-gcc
MSP430_INCLUDE_DIR = $(MSP430_ROOT_DIR)/include/
MSP430_LINKER_DIR = $(MSP430_ROOT_DIR)/include/
INCLUDE_DIR = $(MSP430_INCLUDE_DIR)
LIB_DIR = $(MSP430_LINKER_DIR)
BUILD_DIR = build
OBJ_DIR = $(BUILD_DIR)/obj
BIN_DIR = $(BUILD_DIR)/bin

## Debug and Flashing Directories
TI_CCS_DIR = /home/ws/dev/ide/CCStudio/ccs1230/ccs
DEBUG_BIN_DIR = $(TI_CCS_DIR)/ccs_base/DebugServer/bin
DEBUG_DRIVER_DIR = $(TI_CCS_DIR)/ccs_base/DebugServer/drivers

# Toolchain
CC = $(MSP430_ROOT_DIR)/bin/msp430-elf-gcc
DEBUG = LD_LIBRARY_PATH=$(DEBUG_DRIVER_DIR) $(DEBUG_BIN_DIR)/mspdebug

# Base Flags
MCU_FLAG = msp430g2553
WARNING_FLAGS = -Wall -Wshadow -Werror -Wextra
COMPILER_FLAGS = -mmcu=$(MCU_FLAG) $(WARNING_FLAGS) $(addprefix -I, $(INCLUDE_DIR))
LINKER_FLAGS = -mmcu=$(MCU_FLAG) $(addprefix -L, $(LIB_DIR))
DEBUG_FLAGS = -Og -g
RELEASE_FLAGS = -O3

# Target File
TARGET = $(BIN_DIR)/blink

SOURCES = main.c\
          led.c
OBJECT_NAMES = $(SOURCES:.c=.o)

OBJECTS = $(patsubst %,$(OBJ_DIR)/%,$(OBJECT_NAMES))

# Build
$(TARGET): $(OBJECTS)
    @mkdir -p $(dir $@)
    $(CC) $(LINKER_FLAGS) $^ -o $@

# Compiling
$(OBJ_DIR)/%.o: %.c
    @mkdir -p $(dir $@)
    $(CC) $(COMPILER_FLAGS) -c $^ -o $@

# Phonies
.PHONY: all clean flash

all: $(TARGET)

clean:
    rm -rf $(BUILD_DIR) $(TARGET)

flash: $(TARGET)
    $(DEBUG) tilib "prog $(TARGET)"
```

### Tips

- Essential Compilation Flags: [The
Best and Worst GCC Compiler Flags for Embedded](https://interrupt.memfault.com/blog/best-and-worst-gcc-clang-compiler-flags)

