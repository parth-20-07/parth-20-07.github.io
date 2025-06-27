---
id: embedded
aliases:
  - SWD Serial Wire Debug
tags:
---

# SWD Serial Wire Debug

**Table of Contents**

# About

- 2 wire protocol to access the ARM debug interface. Alternate to JTAG.
- Defines in ARM Debug Interface Specification V5
- 2 Lines + 1 (Optional):
    - SWDIO: Bidirectional Data Line
    - SWCLK: Clock driven by the host
    - SWDO: Trace Pin for Communication
- Useful to program MCUs internal Flash, Access Memory Regions, Add Breakpoints, stop/run CPU

# Important Information

## STM32 Family

- They have a FIFO inside their ITM unit which serializes the data and sends it out via the SWO Pin.
- Make this change in the `` to enable the `printf` via SWO pin.|  Change: https://github.com/niekiran/Embedded-C/blob/master/All_source_codes/target/itm_send_data.c | Guide: https://www.udemy.com/course/microcontroller-embedded-c-programming/learn/lecture/16546068#overview

# References

https://developer.arm.com/documentation/101636/0100/Debug-and-Trace/JTAG-SWD-Interface

https://community.silabs.com/s/article/serial-wire-debug-swd-x?language=en_US

https://wiki.segger.com/Target_Interface_SWD

https://infocenter.nordicsemi.com/index.jsp?topic=/nwp_034/WP/nwp_034/nwp_034_swd_if.html
