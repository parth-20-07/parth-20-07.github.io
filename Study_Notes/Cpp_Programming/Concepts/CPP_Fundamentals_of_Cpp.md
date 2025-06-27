---
id: cpp
aliases:
  - Fundamentals of C++
tags:
---

# Fundamentals of C++

> This complete guide on these fundamentals is built on:
> - A Touf of C++ (Second Edition) by Bjarne Stroustrup

## 3. Modularity
### 3.5 Error Handling
#### 3.5.3 Error Handling Alternatives

- C++ is a language where exceptions are designed to be used to report failure to complete a given task. Exceptions are integrated with constructors and destructors to provide a coherent framework for error handling and resource management.

- A function can indiciate that it cannot perform its alloted task by:
	- Throwing an exception 
	- Somehow return a value indicating failure
	- Terminating the program by invoking a function like `terminate()`, `exit()`, Or `abort()`.
##### Return an Error Indicator

> This is useful when you are building an application where errors are often expected. These are situations like interacting with user input or filesystems or data communication protocols where corruption is possible.

- A failure is normal and expected For example, it is quite normal for a request to open a file to fail (maybe there is no file of that name or maybe the file cannot be opened with the Pemissions requested).
- An immediate caller can reasonably be expected to handle the failure
##### We throw an exception
- when: An error is so rare that a programmer is likely to forget to check for it. For example, when did you last check the retumn value of printf(? An error cannot be handled by an immediate caller. Instead, the error has to percolate back to an ultimate caller. For example, it is infeasible to have every function in reliably handle every allocation failure or network outage an application New kinds of errors can be added in lower-modules of an application SO that higher-level • modules are not written to cope with such erTors. For example, when a previously single- threaded application 1S modified to use multiple threads or resources are placed remotely be accessed over to network. • No suitable return path for errors codes are available. For example, a constructor does not have a return value for a caller' to check. In particular, constructors may be invoked for several local variables or in a partially constructed complex object so that clean-up based on error codes would be quite complicated ◦ The return path of a function is made more complicated or expensive by a need to pass both value and an error indicator back (e.g., a pair; $13.4.3 ), possibly leading to the use of out- parameters, non-local error-status indicators, or other workarounds. • The error has to be transmitted up a call chain to an "ultimate caller." Repeatedly checking an error-code would be tedious, expensive, and error-prone • The recovery from errors depends on the results of several function calls, leading to the need to maintain local state between calls and complicated control structures. • The function that found the error was a callback (a function argument), so the immediate caller may not even know what function was called. An error implies that some "undo action" is needed. We terminate when An error is of a kind from which we cannot recover. For example, for many - but not all • systems there is no reasonable way to recover from memory exhaustion. The system is one where error-handling is based on restarting a thread, process, or computer • whenever a non-trivial error is detected. 
