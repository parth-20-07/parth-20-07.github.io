---
id: cpp
aliases:
  - Exit Status
tags:
---

# Exit Status

## How is an exit_status code useful?

In C and C++, the `main` function is the entry point of a program. When a program is executed, the operating system calls the `main` function, and its return value plays a significant role in the communication between the program and the operating system.

The `main` function in C and C++ has a return type of `int`, which allows it to return an integer value. The return value serves as an indication of the status or outcome of the program’s execution to the operating system. Here’s how Unix (and most modern operating systems) make use of the return value from the `main` function:

### Exit Status

The return value of the `main` function is often referred to as the “exit status” of the program. It is an integer that represents the program’s termination status. By convention, a return value of `0` typically indicates successful execution, while a non-zero value (usually positive) indicates an error or some specific issue that occurred during the program’s execution.

### Accessing the Exit Status

In Unix, after a program finishes execution, the exit status can be accessed by the parent process (typically the shell) that initiated the program. This exit status can be checked using the special shell variable `$?`. For example, after running a program, you can check the exit status in a Unix shell like this:

```bash
./my_programecho $?
```

### Signal Status

Besides using specific return values, the operating system can also communicate with the program through signals. Signals are used to notify processes about various events, such as errors, termination requests, or other conditions. A program can set up signal handlers to handle specific signals and take appropriate actions.

### Error Handling

The `main` function can return different non-zero values to indicate specific error conditions. For example, returning `1` might indicate a file not found error, while returning `2` could indicate insufficient memory. The choice of error codes and their meanings is up to the developer.

### Batch Processing

In Unix systems, the exit status is useful when scripting or using batch processing. A batch script can check the exit status of various programs executed within it and take different actions based on the returned values.

## Various Types of exit_status

In C and C++, the exit status is an integer value that can be returned by the `main` function to indicate the status or outcome of the program’s execution. While there are no strict rules or predefined constants for exit status values, certain conventions are often followed to convey specific information about the program’s termination. Here are some common exit status conventions:

### Success

Typically, an exit status of `0` is used to indicate the successful execution of the program. This is considered the “success” status, and it indicates that the program completed its task without any errors.

### General Errors

Exit status values greater than `0` (usually positive) are used to indicate different types of errors. The specific values and their meanings can vary based on the program’s design and the developer’s choice. For example:

- `1`: General error code.
- `2`: Misuse of shell commands (e.g., incorrect command-line arguments).
- `3`: An input file couldn’t be opened.
- `4`: An output file couldn’t be written.
- `5`: A required library or resource is missing.

### Signals

Apart from explicitly returning an integer value, the operating system can also terminate a program due to receiving certain signals. In Unix-like systems, signals are represented by small positive integers. For example:

- `SIGINT`(2): Termination signal from the keyboard (e.g., when the user presses Ctrl+C).
- `SIGSEGV`(11): Invalid memory access (segmentation fault).
- `SIGTERM`(15): Termination signal.

### Reserved Values

Some operating systems may have reserved exit status values for special purposes or to represent specific conditions. These values are not part of the official C/C++ standard but might be used for specific applications or platforms.

It’s important to note that the specific exit status values and their meanings are not standardized across all programs or operating systems. Developers are free to choose exit status values that best represent the outcome of their program’s execution. However, adhering to common conventions and using meaningful exit status values can help other developers, scripts, and tools understand the program’s status more easily.
