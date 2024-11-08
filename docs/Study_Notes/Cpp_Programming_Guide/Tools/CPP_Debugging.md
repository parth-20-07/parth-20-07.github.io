---
id: CPP_Debugging
aliases:
  - Debugging
tags: []
---

# Debugging


## Compiler Based Debugging (G++)

### Visual Debugging

- **Wall** Flag: show all warnings. It turns on all standard C++ warnings about code that might cause unexpected or undefined behaviour.
- **Wextra** flag: enables some extra warnings not turned on by *Wall*. These include warnings for the bad pointer to integer zero comparisons, base class not initialized in the copy constructor of the derived class, etc.
- **Wfatal-errors** flag: is similar to *Wall* but treats an error as fatal and stops before dumping a long list of errors into the terminal. This flag is useful when you want to sequentially solve the bugs as only one warning error pops up at a time.

### Manual Debugging

- *-D _DEBUG** macro: Define the *DEBUG* Preprocessor while compiling with G++ which can be used to execute specific lines of code only when the -D Flag is passed.

```cpp
float PI = 3.1415;
#ifdef DEBUG
std::cout<< "Pi is: "<< PI<<"\n";
#endif
```

This portion of the code will output the value of PI only when the DEBUG Macro is defined using flags in g++.

## Interactive GDB Based Debugging

### Manual Debugging

- **g** flag: Compile the program first using this flag alongside others.

```bash
g++ -g prog.cpp -o prog
```

- Once the compilation completes, run your code with gdb using

```bash
gdb ./prog
```

- Once the initialization is completed, type *run*
- *Ctrl+L* clears the terminal.
- Type *start* to initialize debugging.
- Type *next* to go to the next line.

### IDE Based Debugging

- Better and interactive debugging can be done using VSCode
- Set up the *launch.json* with the correct flags for debugging rules.

## Resources

- [G++ Cheatsheet](https://bytes.usc.edu/cs104/wiki/gcc/)
