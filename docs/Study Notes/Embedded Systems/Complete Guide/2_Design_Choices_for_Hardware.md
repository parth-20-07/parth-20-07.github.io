---
id: 2_Design_Choices_for_Hardware
aliases:
  - 2. Design Choices for Hardware
tags: []
---

# 2. Design Choices for Hardware

> Guide: PCB Design Walkthrough Sumo Robot

## Organization

A good practise to organize the CAD Files for Hardware can be like this

```
.
|-- imgs
    |-- Manufactured PCB.jpg
    |-- Board.jpg
    |-- Schematics.pdf
    |-- Schematic.png
|-- libs
    |-- 3d Models
    |-- Footprints
    |-- Symbols
|-- manufacture
    |-- Version
        |-- Assembly
            |-- BOM.csv
            |-- CPL.csv
        |-- Gerber
|-- Design Files
    |-- Schematics
    |-- Board
|-- .gitignore
|-- LICENSE.md
|-- README.md
```

## Design Choices

### Time vs Money

You have to choose prototyping the design. What are you more short on? Time or Money? If

- *Money*: You cannot go through many iterations, order every component you need and create a hand soldered version of the final design first to ensure minimal wastage of funds on redesign of PCB and reordering the manufactured parts.

- *Time*: If you are short on time, iterate through new designs as quick as possible. Make a design, order it pre-assembled from company and test it out. If something does not function properly, patch it manually and order a new design. You do not want to waste a lot of time tinkering. Use ready made versions of modules if allowed to reduce chances of failure. Focus your time between placing order and getting the assembled PCB on writing and improving firmware.
    

### Pre-assembled Modules vs Custom Design

Making a choice between designing everything in a single board vs using ready-made modules can be daunting.

- Pre-assembled:
    - Go for this approach if you are short on time.
    - You have relaxed restrictions on size of the hardware. With relaxed size, a component hanging in air might not be an issue.
    - You are not using a component module which relies on sturdy communication like I2C sensors which often tend to work even in a somewhat loose Header Situation unlike SPI based SD Modules.
- Custom Design:
    - When customer/client asks for a custom design to avoid any sort of legal issues.
    - You have tight board dimension restrictions
    - Weak connections can cause severe points of failures.
    - You have sufficient development time on hand.

> Tips: Despite any choice you make while design, make sure to order the development board first before starting with any manufacturing, it allows you to have a first hand experience for trying out your firmware to ensure compatibility and noise mitigation required between MCU and peripherals.
