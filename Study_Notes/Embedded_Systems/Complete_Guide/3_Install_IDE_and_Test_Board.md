---
id: embedded
aliases:
  - 3. Install an IDE and Perform Hello World
tags:
---


# 3. Install an IDE and Perform Hello World

> Guide: Install and IDE and Blink an LED

> As much as you might want to jump into IDE-less development, test your board first. Hardware Assembly is bug prone, manufacturing is bug prone. You might have an issue with a broken board where something might not be working and IDE makes it easier to find that out using software due to plenty of pre-built examples for your target hardware and lot of heavy lifting related to setup and deployment handled by your IDE.

While developing Software for embedded systems, you need to handle a lot of different programs to convert the `.cpp` file into binary suitable for target hardware. This work can be avoided if you use an IDE. An IDE provides a GUI based interface (generally) which will allow you to focus more on programming and less on background processes which might be great for someone who just wants to focus on Firmware and not on optimzation of backend compilation.

Every brand of MCU tends to have its own IDEs like:

- STM32CubeMX/STM32CubeIDE for STM32 Boards
- Arduino IDE for Arduino Boards
- CCStudio for TI Boards
- Keil Studio is a general IDE which supports many platforms for development

Test out the development board before moving forward to ensure basic peripherals of the MCU are functional using a simple blink sketch.

