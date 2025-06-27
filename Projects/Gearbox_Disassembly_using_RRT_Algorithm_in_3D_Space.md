---
id: project
aliases:
  - Gearbox Disassembly using RRT Algorithm in 3D Space
tags:
---


# Gearbox Disassembly using RRT Algorithm in 3D Space

## Simulation
### Setup
![[gd-1.jpg]]

### Motion
![[gd-2.jpg]]
![[gd-Animation.gif]]

### Results
![[gd-rrtNodes.png]]
![[gd-rrtPath.png]]

> Developed at: Worcester Polytechnic Institute

> Project date: April, 2023

> GitHub URL: [parth-20-07/Gearbox-Disassembly-using-RRT-Algorithm-in-3D-Space](https://github.com/parth-20-07/Gearbox-Disassembly-using-RRT-Algorithm-in-3D-Space)

## Brief Introduction on Project

The objective of this task is to disassemble a gearbox with no collisions using RRT Algorithm. We are using the gearbox called SM-465.

Restriction based Experimentation:

- Restricting the Planner to only 2 axes improves the speed to find the path.
- Restricting the domain of the new node search reduces the path planning time.
- Too small of a search domain satisfies the position requirement but fails the orientation requirement for motion.

Search Based Experimentation

- Setting the node search radius too small makes the path smooth but takes a lot of time.
- Setting the node search radius too big results in the shaft not being able to get out of the Gearbox due to collisions while motioning.

