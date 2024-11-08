---
id: CPP_auto
aliases:
  - auto
tags: []
---

# auto


> Complete Documentation from: [cppreference.com](https://en.cppreference.com/w/cpp/language/auto)

We use **auto** where we don’t have a specific reason to mention the type explicitly. “Specific Reasons” include:

- The definition is in a large scope where we want to make the type clearly visible to readers of our code.
- We want to be explicit about a variable’s range or precision. (e.g. double rather than float)
- We want to avoid redundancy and writing long type names. (e.g. std::chrono has really long type names)
- When we want to write in generic programming mode where the exact type of an object can be hard for programmers to know.

**Special Note:** `auto` is a feature that helps you to improve readibility. Use it wisely and only when needed. Don’t use auto everywhere as it will make it difficult for people to understand data types for simple things too.
