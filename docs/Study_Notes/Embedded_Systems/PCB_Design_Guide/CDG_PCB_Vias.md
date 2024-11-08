---
id: PCB Vias
aliases:
  - PCB Vias
tags: []
---

# PCB Vias

# Basics

**Vias are Vertical connections in the third dimension(Z) of a PCB**

- Used for connection between layers allowing traces to ‘jump’ between layers in PCB.
- Can be used to improve air circulation around PCB for better thermals.
- Can be used to reduce eddy-current in the circuit if it is an issue.
- Generally ‘tiny’ through holes.

## Parameters

![[pcb_via.png]]

- Minimum Drill Size and Annular Ring Size are provided by the manufacturer based on their machine specifications.

It is always suggested to set the PCB software minimum tolerances higher than manufacturer tolerances to avoid the risk of error in manufacturing.

## Current Handling

For a general rule of thumb:

1 X Via → 1.5 Amps (~regardless of size)

For higher currents, simply use multiple vias in parallel.

Use Saturn Toolkit to choose the right via for current handling.

## Terminologies

- Pad Size: Total Diameter of the Via
- Drill Size: Size of the hole via
- Annular Ring: The Metal Pad around the Via
- Tenting: Covering your ‘via' using a Solder Mask. Uncovered Via can be used as Test Points or Cooling. Cover Vias provide better temper protection and waterproofing.
- Filling:

---

# Placement

**TO-DO:**

- Keep adequate distance between vias and traces to avoid manufacturing failure. Minimum Distance is provided by the Manufacturer's Guideline for PCB.
- Keep Different signals vias as far as possible to avoid coupling issues between signals.

**Power/GND Vias**

- Keep as close to the IC Pad as Possible
- Keep the Width of Via the same as the trace width
- Keep Ground and Power Pad close to each other to reduce inductance.

**Differential Pair Vias**

- Keep them as close as possible keeping the manufacturing tolerance in mind.
- Keep in mind that close vias don’t cut the other layers into different groups.

## Voiding

- This is the issue caused because vias have an isolation ring around the annular ring to separate a layer from the via. This can result in the issue of cutting a plane into parts or causing small island formation if vias are kept too close to each other.
- Voiding can result in high-impedance routes and EMI introduction in high-frequency circuits.

---

# Transfer Vias

Transfer Vias are needed when we are using Vias for high-frequency signals ( >20 kHz ) which can result in the introduction of noise if not coupled well Ground Plane.

**TO-DO:**

- Place a Ground Via near a Signal Vias to provide better coupling.
- Multiple Transfer Vias can share a common ground ‘via’ if they are nearby.

---

# Stitching Vias

Vias are used to connect similar types of planes and create them as a single unit.

**Reasons:**

- Connecting multiple GND/PWR layers has the following benefits:
    - Reduces Inductances
    - Ensures Copper Islands don’t act as antennae, resonate, and radiate.
    - Reduces Eddy Current due to reduced surface area.
- **Shielding**: Stitching vias can be used for shielding to suppress the energy of electromagnetic waves up to a certain frequency from entering/leaving a section of the PCB. The stitching via distance can be calculated using:
    
    $$
    L = \frac{1}{20}*\frac{c}{\sqrt{\epsilon}f_{max}} 
    $$
    

**TO-DO:**

- Stich the Power Places at certain intervals.
- To reduce shielding, calculate minimum distance and place the vias around high frequency circuit areas.

---

# Resources

[PCB Vias 101 - Phil's Lab #77](https://youtu.be/WPT96w3eLAM?si=YvLe6UW6pzL2pxtq)

[Saturn PCB Design Toolkit Version 8.37](https://saturnpcb.com/saturn-pcb-toolkit/)
