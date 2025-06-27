---
id: cpp
aliases:
  - Testing
tags:
---

# Testing

## Types of Testing

### Automated Testing

This is generally used to test the complete code at a single time.

### Unit Testing

Testing small chunks of code at individually to ensure that portions or amendements in code work properly

## Available Testing Frameworks

- Boost Test Library
- Catch 2
- CppUnit
- Google Test
- Microsoft Unit Testing Framework
- Parasoft C/C++ Test
- QtTest

and many more….

## Test Driven Development (TDD)

Test-driven development is a software development process relying on software requirements being converted to test cases before software is fully developed, and tracking all software development by repeatedly testing the software against all test cases.

### What is a standard TDD cycle?

- Before writing any new code, write a new test.
- Just write enough new code to make the new test compile and fail
(empty function).
- Run the test and watch it fail.
- Write new code required to make the test pass.
- Refactor the new code to prepare for the next cycle.
    - When you’re done, the test should still pass.

## Google Test FrameWork

### Google Test FrameWork Setup in VSCode

- Install the `C++ Extension Pack` and `CMake Tools` Extension in VSCode.
- Install CMake on device if it is not installed.
- Establish the new C++ Project.
- Open a new terminal in VSCode and clone the googletest framework using

```bash
git clone https://github.com/google/googletest.git
```

- Create a `CmakeLists.txt` file with the following setup

```makefile
cmake_minimum_required(VERSION 3.8)
set(This PROJECT_NAME)#Project Name hereproject(${This} C CXX)
set(CMAKE_C_STANDARD -99)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_POSITION_INDEPENDENT_CODE ON)
enable _testing()
add _subdirectory(googletest)
set(Headers
    Header_File.hpp #Header names here)
set(Source
    Header_File.cpp #Source names here)
add _library(${This} STATIC ${Sources} ${Headers})
add_subdirectory(test)
```

- Generate the header and source files along with main file.
- Create the `test` folder and inside it generate the `CMakeLists.txt` file with the following code

```makefile
cmake_minimum_required(VERSION 3.8)
set(This Tests)
set(Sources
    Tests.cpp
)
add_executable(${This} ${Sources})
target_link_libraries(${This} PUBLIC
    gtest_main
    PROJECT_NAME
)
add_test(
    NAME ${This}    COMMAND ${This})
```

- Generate `Tests.cpp` File in the `/test` folder in PROJECT DIRECTORY
- Reload VS Code.
- Select the default compiler for build system generation.
- Build the code and wait for it.
- After build is complete, look for file `/build/CMakeCache.txt` and find the tag `gtest_force_shared_crt:BOOL` and set the value to `ON`.
- From the CMake Tools Tab in the left, select `Clean Rebuild` and wait for no warnings.
- Click *Ctrl+Shift+P* and search for *CMake: Run Tests.* A default test will run and all will clear up.

## Resources
