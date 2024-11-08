---
id: CPP_new
aliases:
  - new keyword
tags: []
---

# new keyword

## Notes

The `new` keyword is C++ equivalent of `malloc()` from C along with the added benefit of calling the constructor when used with a class. Using new allows you to allocate memory on Heap.

### How the new keyword works?

1. On calling `new`, the new keyword takes the size needed to be loaded in the memory as input.
2. It searches through the heap to find a continuous block of memory as required.
3. After successfully finding the block of memory, it returns the pointer to the memory.

### How to load a variable on heap?

```cpp
int *var = new int;
```

The `var` will store the pointer to the heap memory where the value can be stored. It will be of 4 bytes. (size of int = 4 bytes)

### How to load a block of memory?

```cpp
int *b = new int[50]; //200 bytes
```

Using `[]` brackets will allocate 50 block of memory with the size of int. This will be 200 bytes. (50 blocks of size of int = 50 x 4 = 200 bytes)

### Important Things to know before using new

- Always clear the memory after the scope of `new` is complete. Since the memory is allocated in heap, it does not auto delete. Clear the memory using `delete` keyword as follows:
    
    ```cpp
    int *a = new int;
    delete a;//Since A single block of memory is allocated//clear directly using delete keyword
    
    int *b = new int[50];
    delete[] b;// Remember to use [] next to delete if you want to clear all the memory blocks.// If you don't use [], it will only clear the first block of memory
    ```
    
- For a class, though `new` in C++ it equivalent to `malloc` in C, do not use them interchangeably. `malloc()` just returns pointer to the memory block when you use it with a class. `new` returns the pointer and also initializes the object for the class.

## Resources
