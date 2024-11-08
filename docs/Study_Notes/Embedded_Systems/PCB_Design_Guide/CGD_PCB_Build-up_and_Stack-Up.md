---
id: PCB Build-up and Stack-Up
aliases:
  - PCB Build-up and Stack-Up
tags: []
---

# PCB Build-up and Stack-Up

---

**Table of Contents**

---

# Concepts

## Build-Up

![Untitled](docs/Study%20Notes/Embedded%20Systems%20Hardware/PCB%20Build-up%20and%20Stack-Up%20090d39b4318f4be6aba429c006fd9f92/Untitled.png)

Build-up related to the physical construction of the board. Primary things to consider are:

- Layer Count
- Dielectric Materials
    - Prepreg
    - Core
- Copper foil
- Manufacturability
- IPC Class
- HDI
- Surface Finish

---

![Untitled](Untitled%201.png)

**Layer Count**

- Defined by Routing Constraints, Density, Package Types, Power Delivery
- Constrains overall minimum thickness of PCB
- Standard is 1.6mm PCB and limited to 10-12 Layers
- Depends on the PCB Stack-up

---

**Prepreg (Pre-impregnated)**

- Layer of fiberglass impregnated with resin (e.g. FR4)
- Sandwiched between layers of copper planes/traces
- Defined by dielectric constant ($\epsilon_{r}$) and thickness.
- Main Task is to provide isolation between two copper layers or traces. It is helpful in laying down copper traces in a layer where the copper is in form of traces and needs support.

---

**Core**

- Pre-pressed layers consisting of:

```jsx
Copper Foil
---------------
Core Dielectric
---------------
Copper Foil
```

- This is the innermost layer of the PCB and it’s main job is to provide rigidity to the PCB.

---

**Copper Foil**

- Defined primarily by copper weight/thickness.
- Two types:
    - Outer Layer: Typically thicker (1 oz is standard and max is ~ 8 oz)
    - Inner Layer: Typically thinner (1/2 oz is standard and max is ~ 1 oz)
- Main deciding factor is current handling capacity:
    - For Outer Layer, 1 mm traced (allowed 20 deg C temperature rise) → 1/2 oz: 1.9A, 1 oz: 3.2 A
    - Saturn PCB Toolkit can help in finding the appropriate layer width

---

**Manufacturability**

- Always talk to the PCB Manufacturer and inquire about the ‘standard N-Layer’ build-ups they can manufacture
- General rule of thumb is to make the design with manufacturability in mind and keep tolerances as lenient as possible till it does not hamper the design to have maximum chances of manufacturability and least possible error.
- Communicate with manufacturer if you need some special tolerances.

---

## Stack-Up

Board Stack-up focuses on the electrical type of each layer of PCB. It provides the basis for:

- Determining the number of layers in the board
- Assigning GND, PWR, or SIGNAL to the individual layers
- Layers can have a mix of SGN/PWR or PWR/GND or any possible combination unless it does not interfere with the circuit performance.

```jsx
     |-----------------------------------------------|
L1   |                    Signal                     |
     |-----------------------------------------------|
L2   |                    Ground                     |
     |-----------------------------------------------|
L3   |                    Ground                     |
     |-----------------------------------------------|
L4   |                    Signal                     |
     |-----------------------------------------------|
```

- Signal Layers are generally built using traces
- GND/PWR layers are generally large planes to provide low resistance pathways for current to flow through

---

**Layer Types**

- **GND**: Used as a reference plane/layer for signal traces an power pours/traces.
- **PWR**: For Power distribution, critical for high speed circuits. Together with GND layers forms ‘parallel plate capacitors’. Circuit often tend to have different volage planes on ‘PWR’ Layer.
- **SIG**: Predominantly trace layers (’forward path’) using a GND or PWR layer for reference(’return path’)

---

**Sensibly Assign Layer Types**

Goals of a good stack up are:

- Wanting a systematic approach to deciding stack up
- Ensure Good EMC/EMI performance
- Ensure good signal integrity
- Ensure good power delivery

---

## **Golden Rules**

- For AC or high frequency DC (above ~kHz), return path is directly underneath forward path. So a good idea idea is to have traces or GND Planes directly below the forward trace/plane.
- Signal Energy flows between copper layers in High Frequency DC/AC. i.e. keep these layers as close as possible.
- **Avoid Fields from Spreading**
    
    ![Untitled](Untitled%202.png)
    
    - Spreading fields means coupling from signal to signal → Crosstalk
    - Spreading fields means some form of unwanted radiation → EMI
    - Can be reduced by placing GND directly below signal lines
    - Shielding Signals from both sides using GND Planes/traces.
- **Power Planes:** Keeping PWR/GND planes close to each other ensures high capacitance and low inductance in the dielectric. This ensures better power delivery performance.
- Put PWR Layer close to high speed IC as it reduces inductances for improved power delivery
- Keep Signal Layers separated by GND layer to avoid SIG-SIG coupling field or cross talk.
- Use stitching vias between different GND planes to maintain similar potential
- Use GND Vias next to transfer vias to provide the signal transmission path.
- Keep One GND Layer close to any one PWR layer for good power delivery.

---

# Resources

[PCB Stack-Up and Build-Up - Phil's Lab #56](https://youtu.be/QAOEtfvCaMw?si=dD2rSHy26JQ1axHr)

[Ultimate Guide To PCB Prepregs: VS Core VS Laminate. - Jhdpcb](https://jhdpcb.com/blog/pcb-prepreg-guide/)

[Saturn PCB Design Toolkit Version 8.37](https://saturnpcb.com/saturn-pcb-toolkit/)

[Multilayer PCB Stack-up Basics | PCB Knowledge](https://youtu.be/ubE7r4gUWlA?si=cmPjhLcGBe645L2C)
