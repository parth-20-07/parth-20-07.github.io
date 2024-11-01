---
id: PID based SCARA Robot Position and Velocity Control And Manipulation
aliases:
  - PID based SCARA Robot Position and Velocity Control And Manipulation
tags: []
---

# PID based SCARA Robot Position and Velocity Control And Manipulation

> Developed at: Worcester Polytechnic Institute

> Project date: December, 2022

> GitHub URL: [parth-20-07/PID-based-SCARA-Robot-Position-and-Velocity-Control-And-Manipulation](https://github.com/parth-20-07/PID-based-SCARA-Robot-Position-and-Velocity-Control-And-Manipulation) 

## Introduction on Project

The aim of the assignment is meant to get a better understanding of basic concepts of Robotics using tools like ROS2 Humble Hawksbill, Gazebo Sim, RVIZ and MathWorks® MATLAB The final project is divided into three seperate assignments:

### Assignment 1: Build Model URDF, Forward Kinematics Node and Inverse Kinematics Node

- Setup the dynamically accurate model of robot in Gazebo by editing the model URDF.
- Calculate the DH Parameters of the Robot to build a node that:
    - Subscribes to the joint states of the robot and calculates the Pose of the robot by using Forward Kinematics.
    - Publishes the Pose of the Robot to a new topic using a publisher.
- Create an Inverse Kinematics Calculation Node that:
    - Uses a custom service to take input of the (x,y,z) coordinates of the robot end-effector.
    - Calculate the Joint States from the end-effector position and return it as a response to the service.

    ![[Gazebo Model-pid.png]]

    ![[PlotJugger Set Canvas.png]]

### Assignment 2: Build a node to control Joint States

- Create a node that takes Reference Values for Joint Position as input through a service.
- Build a Proportional-Derivative Controller that takes the current Joint States and Reference joint state to publish the control torque values to the `/forward_effort_controller/commands` topic.
- Ensure that the model reaches the reference joint states in Gazebo.

![[vel.gif]]

### Assignment 3: Build a node to control Joint or End-Effector Velocity

- Create a node with 2 services:
    - Service 1 takes Joint Velocity as input to convert it to End-Effector Velocity as output.
    - Service 2 takes End-Effector Velocity as input to convert it to Joint Velocity as ouput.
- Based on the input, build a Proportional-Derivative controller that takes the current Joint Velocity and the Reference Joint Velocity to publish the torque values to the `forward_effort_controller/commands` topic.
- Ensure that the model reaches the desired end-effector or joint velocity in Gazebo.

![[eff.gif]]


