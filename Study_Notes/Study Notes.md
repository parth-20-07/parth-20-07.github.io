
# Software Engineering Essentials
## Version Control and Pipelines
- [[version_control|Version Control]]
- [[ci-cd|CI/CD Pipelines]]

## Algorithms
- [[dsa|Data Structures and Algorithms]]

## Programming Methods
- [[monads|Monads]]

## Build Systems
- [[cmake|CMake: A Meta Build System]]

## Containers
- [[docker_basics|Docker Basics]]
- [[docker_networking|Docker Networking]]

## Linux
- [[embedded_linux|Embedded Linux]]
- [[linux_file_system|File System]]

## Networking
- [[Wireshark]]

## Communication Methods
- [[rest_api|REST API]]

## Textbooks

[[cs_textbooks|Computer Science and Software Development]]

---
---
# Programming Languages
## Rust Essential Notes

- [[rust_structs|Structs]]
- [[rust_ownership|Rust Memory Ownership]]
---
## C++ Essential Notes
### Essentials

#### Tools

- [[CPP_Project_Setup|C++ Project Setup]]
- [[CPP_Debugging|Debugging]]
- [[CPP_Testing|Testing]]
- [[CPP_Static_Analysis|Static Analysis]]
- [[CPP_Makefile|Makefile]]
- [[cpp_modern_environment|Setting up Modern Environment]]
- [[CPP_clang_format|Clang Format]]

#### Concepts

- [[CPP_Fundamentals_of_Cpp|Fundamentals of C++]]
- [[CPP_Multithreading|Multithreading]]
- [[CPP_Object_Oriented_Programming|Object Oriented Programming]]
- [[CPP_Function_Overloading|Function Overloading]]
- [[CPP_Function_Pointers_and_Lambdas|Function Pointers and Lambdas]]

#### Implementation Specifics

- [[CPP_Strings|Strings]]

### Keywords

- [[CPP_new|new]]
- [[CPP_auto|auto]]
- [[CPP_Exit_Status|exit Status]]

### Resources

- [[Computer Science |Good Books]]
- [[CPP_Tips|Tips]]
- [Borrow Checker, Lifetimes and Destructor Arguments in C++](https://a10nw01f.github.io/post/advanced_compile_time_validation/)
- [C++ Coding with Neovim - Prateek Raman - CppCon 2022](https://youtu.be/nzRnWUjGJl8?si=d5jGNUrS-7IFn56F)
---
## Lua Essential Notes

### Lua Embeddeding

### Resources
- [Embedding Lua in C - Lucas Klassmann](https://lucasklassmann.com/blog/2019-02-02-embedding-lua-in-c/)
- [YouTube:  Embedding Lua in C++ #1](https://youtu.be/4l5HdmPoynw?si=IktyQUSzSc_PX2-Z)
- [YouTube:  Embedding Lua in C++ Part 3: Meh... Just use Sol...](https://youtu.be/ONywOoJC4TQ?si=YTgvVm5dIAMhXFY1)
 - [YouTube:  CppCon 2017: Andreas Weis “Howling at the Moon: Lua for C++ Programmers”](https://youtu.be/pfwHCiP1HFM?si=l0Jye1itNFBO4_qq)
---
## Python Essential Notes
### Tools
- [[python_project_stup|Project Setup]]
- [[python_testing|Python Testing]]

---
---
# Embedded Systems

- `.elf` → (Executable and Linkable Format) Good for debugging. Do not use for production
- `.bin/.hex` → (Binary/Hexadecimal) Good for Production

## Design Guides

- [parth-20-07/Embedded-Engineering-Roadmap](https://github.com/parth-20-07/Embedded-Engineering-Roadmap)
- [[0_Complete_Guide | Embedded Systems Design Guide]]
- [[custom_makefile_and_register_programming|Design from Scratch using Custom Makefiles and Register Programming]]
- [[programming_embedded_systems_in_c_cpp|Programming Embedded Systems In C and C++]]

--- 

## Embedded Software Development

### Frameworks

- [[freertos|freeRTOS]]
- [[platformio]]

### Bare Metal Programming

- [[embedded_project_setup| Project Setup]]
- [[bare_metal_avr_c| AVR-C]]

### Communication Protocols

- [[SW_EtherCAT_Protocol | EtherCAT Protocol]]
- [[SW_Controller_Area_Network_Bus_Protocol | CAN Bus Protocol]]

### FPGA Programming

- [[fpga_programming_index | FPGA Programming]]

---
## Embedded Hardware Development

### Design Software
- [[altium_designer|Altium Designer]]
- [[kicad|KiCAD]]

### Communication Protocols

- [[HW_I2C | I2C: Two-Wire Serial Communication]]
- [[HW_CAN|CAN: Controller Area Network]]

### Circuit Design Concepts
- [[esd_protection|ESD Protection]]
- [[pcb_silkscreen_and_soldermask|PCB Silkscreen and Solder Mask]]
- [[pcb_vias|PCB Vias]]
- [[pcb_grounding|Proper Grounding]]
- [[pcb_buildup_and_stackup|PCB Build-up and Stack-Up]]

---
## Component Design Guide

- [[switching_regulators|Switching Regulators]]
- [Mastering motor control: implementation in C++ - Embedded](https://www.embedded.com/mastering-motor-control-implementation-in-c/)
## Power System Design
- [[power_distribution_board|Power Distribution System]]
- [[battery_design|Battery Design]]

---

## Debugging Protocols

- [[jtag|JTAG (Joint Test Action Group)]]
- [[openocd|OpenOCD Open On-Chip Debugger]]
- [[swd|SWD Serial Wire Debug]]

---

## Open Source Resources
- [Quetzal-1 Satellite Goes Open Source](https://hackaday.com/2023/07/04/quetzal-1-satellite-goes-open-source/)
- [F´ A Flight Software and Embedded Systems Framework](https://nasa.github.io/fprime/)
- [Complete Course on Embedded System for Medical Devices](https://mlp6.github.io/Embedded-Medical-Devices/)
- [How to Present Your Upskilling Journey to Land a Senior Embedded Role](https://shawnhymel.com/2802/how-to-present-your-upskilling-journey-to-land-a-senior-embedded-role/)
- [knmcguire/best-of-robot-simulators](https://github.com/knmcguire/best-of-robot-simulators)
	- Pegasus Simulator (🥈15 · ⭐ 480) - A framework built on top of NVIDIA Isaac Sim for simulating drones with PX4 support and much more. BSD-3

---

## Textbooks
- [[embedded_textbooks|Embedded Systems Textbooks]]

---
---
# Robotics
## Basics
- [ROB 530 W22 - Mobile Robotics - uMich](https://youtube.com/playlist?list=PLdMorpQLjeXmbFaVku4JdjmQByHHqTd1F&si=9wCuuhWIk-P6_o--)

---

## ROS Resources

- [ROSBag Resources](https://gist.github.com/kscottz/bf38cfd7508928c5bed0c07ecc326d5f)

---
## SLAM
### Resources
- [MIT16.485 - Visual Navigation for Autonomous Vehicles](https://vnav.mit.edu/)
- [Taeyoung96/SLAM-Resources-for-Beginner](https://github.com/Taeyoung96/SLAM-Resources-for-Beginner)
- [[slam_for_dummies.pdf|SLAM for Dummies - Søren Riisgaard and Morten Rufus Blas]]
- [SLAM Resources](https://gist.github.com/kscottz/9c787afebea8429c4d396a170042362e)
---
## Motion Planning
### Basics

- [[northwestern_motion_planning| Modern Robotics - Northwestern Robotics: Basics of Motion Planning]]
---
## Control Systems

### Lecture Series

- [[Controls_Bootcamp_Steve_Brunton |Controls Bootcamp - Steve Brunton]]
- [Understanding PID Controllers - Michael Hart](https://mikelikesrobots.github.io/blog/understand-pid-controllers/)
---
## Autonomous Aerial Vehicles

### Math Concepts
- [[Interpolation]]

### Sensor Concepts
- [[IMU_Basics_and_Attitude_Estimation|IMU Basics and Attitude Estimation]]

## Resources
- [Demystifying Drone Dynamics!](https://towardsdatascience.com/demystifying-drone-dynamics-ee98b1ba882f)
---
## Essential Textbooks
- [[robotics_textbooks|Robotics Textbooks]]
- [[control_systems_textbooks|Control Systems]]
- [[maths_textbooks|Mathematics]]
