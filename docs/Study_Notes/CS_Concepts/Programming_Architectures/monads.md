---
id: sw_essentials
aliases:
  - Monads
tags:
---

# Monads

## Basics

- In functional programming, a monad is a structure that combines program fragments (functions) and wraps their return values in a type with additional computation.
- In addition to defining a wrapping monadic type, monads define two operators: 
    - one to wrap a value in the monad type
    - another to compose together functions that output values of the monad type (these are known as monadic functions).
- General-purpose languages use monads to reduce boilerplate code needed for common operations (such as dealing with undefined values or fallible functions, or encapsulating bookkeeping code). 
- Functional languages use monads to turn complicated sequences of functions into succinct pipelines that abstract away control flow, and side-effects.

> Formal Definition: Monads is a monoid in a category of endofunctors.

Explanation:

- Functor: It is an object or value combined with a mechanism to do something with that value.
- Endofunctors: A subclass of functor which returns the object of same shape as the functor on which it is applied to.
    - Example:
      ```python
      class Functor:
          def __init__(self, value):
              self.value = value
          
          def map(self, func):
              return Functor(func(self.value))
      ```
    - Here, the map function is an endofunctor because it returns the same type Functor on which it was ran.
    - It preserves the structure after the operations.
- Monad: It is a specific type of endofunctor which is meant to preserve structure and composition.

## Resources

- [The Absolute Best Intro to Monads For Software Engineers](https://youtu.be/C2w45qRc3aU?si=dQ5TVmYOVneEmm1n)
- [Monads in Modern C++ - Georgi Koyrushki & Alistair Fisher - CppCon 2023](https://youtu.be/kZ8rbhGgtv4?si=IKziquVPDxqSah3z)
- [What the Heck are Monads?!](https://youtu.be/Q0aVbqim5pE?si=ElIsci9aV-BmG2gA)
