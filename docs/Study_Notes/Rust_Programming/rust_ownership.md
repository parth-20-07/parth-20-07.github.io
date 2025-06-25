---
id: rust
title: Rust Ownership
created: 2025-06-25
---

# Memory Ownership

```
Ownership is Rust’s most unique feature and has deep implications for the rest of the language. It enables Rust to make memory safety guarantees without needing a garbage collector, so it’s important to understand how ownership works.

`Ownership` is a set of rules that govern how a Rust program manages memory. All programs have to manage the way they use a computer’s memory while running. Some languages have garbage collection that regularly looks for no-longer-used memory as the program runs; in other languages, the programmer must explicitly allocate and free the memory. Rust uses a third approach: memory is managed through a system of ownership with a set of rules that the compiler checks. If any of the rules are violated, the program won’t compile. None of the features of ownership will slow down your program while it’s running.

Python -> Garbage Collector
C/C++ -> User Managed for Heap Memory
Rust -> Compiler Managed
```

---

# Rust Ownership Rules
- Each value in Rust has an _owner_.
- There can only be one owner at a time.
- When the owner goes out of scope, the value will be dropped.

---


## Deep Copy

- Copying the structure of the data and the complete underlying data.
    
## Shallow Copy / Move

- Copy the data structure info but not the underlying data.
