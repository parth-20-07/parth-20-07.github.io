---
id: cpp
aliases:
  - Static Analysis
tags:
---

# Static Analysis

## Static Analysis for C/C++

A static analysis acts like a proof-reader for your project. It performs various checks like grammer check in MS Word. It suggest ways to improve code flow, correct logical errors or reduce non-reachable parts of code. There are many options to integrate Static Analysis to your project. One of the opensource available option is `cppcheck`.

You can integrate `cppcheck` in your Makefile to automate the checking.

Install `cppcheck` on your system from a new terminal:

```bash
sudo apt-get install cppcheck
```

Amend the Makefile to integrate `cppcheck` as follows:

```
# CPPCHECK
CPPCHECK = cppcheck
CPPCHECK_FLAGS = --quiet --enable=all --error-exitcode=1 --inline-suppr

cppcheck:
    $(CPPCHECK) $(CPPCHECK_FLAGS)\
        -I $(INCLUDE_DIR)\
        $(SOURCES)
```

## Resources
