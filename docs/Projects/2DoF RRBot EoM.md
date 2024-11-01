---
id: 2DoF RRBot EoM
aliases:
  - 2DoF RRBot EoM
tags: []
---


# 2DoF RRBot EoM

> Developed at: Worcester Polytechnic Institute

> Project date: October, 2023

> GitHub URL: [parth-20-07/2-DoF-Revolute-Revolute-robot-arm-Equation-of-Motion](https://github.com/parth-20-07/2-DoF-Revolute-Revolute-robot-arm-Equation-of-Motion)

## Brief Introduction on Project

I worked on deriving the equation of motion by taking the derivatives of the Lagrangian Function. This method is called as Euler-Lagrange Method.

![[RRBot.png]]

The Lagrange Equation uses the terms Kinetic and Potential Energy of the system. Where: $LE = KE - PE$

The Euler-Lagrangian Equations can be derived by taking a derivative of the Lagrangian Equations. Where,␍

$$
\frac{d}{dt}\frac{\partial L}{\partial \dot{q_{i}}} - \frac{\partial L}{\partial{q_{i}}}= u_{i}
$$

This would result in an equation of form $a\ddot{q} + b\dot{q} + c{q} + d = 0$. We solve for $\ddot{q_{i}}$ which results in the equation of motion for the system. This system does not contain any form of input. Thus, $u_{i} = 0$ for all joints.


![[rrbot-eom.gif]]
