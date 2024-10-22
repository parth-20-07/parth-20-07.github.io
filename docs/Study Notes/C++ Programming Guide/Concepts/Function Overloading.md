---
id: Function Overloading
aliases:
  - Function Overloading
tags: []
---

# Function Overloading

## Notes

Function overloading is a feature of C++ where you can have multiple functions with same name performing different functions or taking/returning different parameters. The compiler can choose which function to call based on the type of input provided to the function.

e.g.

```cpp
void print(int);    // Takes an Integer as input
void print(double); // Takes a floating-point as input
void print(string); // Takes a string as input
void user()
{
    print(42);      // calls print(int)
    print(9.65);    // calls print(double)
    print("Hello"); // calls print(string)
}
```

## Ambiguous Nature

If two alternative functions could be called, but neither is better than the other, the called is deemed ambiguous and the compiler gives and error.

```cpp
void print(double, int);
void print(int, double);
void user()
{
    print(0, 0); // error: ambiguous
}
```

## Resources
