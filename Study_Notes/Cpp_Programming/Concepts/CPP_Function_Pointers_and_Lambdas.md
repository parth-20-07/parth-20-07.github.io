---
id: cpp
aliases:
  - Function Pointers and Lambdas
tags:
---

# Function Pointers and Lambdas


## Lambdas

- Lambdas are short inline functions that are going to be used only once and thus do not need to be written as independent functions.

## General Structure

```cpp
[captures]<tparams>(params) specs {body}
```

- `[captures]` → Interface to decide what variables are to be shared to the lambda’s body from the code scope.
- `tparams` →
- `params` → Parameters to pass in the function. Similar to capture but you can pass constant values without definition here.
- `{body}` → The body to write the the function implementation.

## Captures

## Function Pointers

- Function Pointers are a way to assign function to variables. It can be used to pass function as parameters to other functions.

## Resources

- [C++11 - Lambda Closures, the Definitive Guide - Cprogramming.com](https://www.cprogramming.com/c++11/c++11-lambda-closures.html)
- [Lambdas in C++](https://www.youtube.com/watch?v=mWgmBBz0y8c&ab_channel=TheCherno)
- [Lambda expressions (since C++11) - cppreference.com](https://en.cppreference.com/w/cpp/language/lambda)
