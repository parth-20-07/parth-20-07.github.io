---
id: papers
aliases:
  - SLAM and Map Merging
---

# SLAM and Map Merging

>Created: March 13, 2024 10:06 AM

>Paper Link: https://www.researchgate.net/profile/Luis-Bergasa/publication/237149207_Multi-robot_SLAM_and_map_merging/links/0a85e5320ca3b8fb5a000000/Multi-robot-SLAM-and-map-merging.pdf

>Status: Done

**Table of Contents**

## Introduction

The aim of the paper is to try ways to implement SLAM in multi-robot situation in decentralized manner.

## Outline

### 1. MultiRobot Slam

**Centralized vs Decentralized SLAM**

Centralized System:

Pros:

- Solutions close to optimal

Cons:

- Computationally Intensive
- Needs a central Module to compute

Decentralized System:

Pros:

- Flexible and Robust

Cons:

- Results in Suboptimal Solution
- Difficulty in Coordinating the Tasks
- Can cause issues when multiple robots scan the same area

### 2. Potential Issues in SLAM

- Loop Closure is difficult when the single loop is large
- Choosing the right sensors suite to get better quality map but not be too computationally intensive.

### 3. Rao-Blackwellized Particle Filters

It uses particle filter based approach to build a probablistic slam map which is based on Bayes Theorem.

The objective is to build up a highly accurate map of an unknown environment with unknown initial position of robots using Rao-Blackwellized particle filtering and scan matching.

### 4. Grid-Based SLAM Algorithm Idea

The ideology for this slam can be implemented as follows for a single robot:

1. Build the Map using *Grid-based Fast-SLAM*
2. Localize using *Rao-Blackwellized Mapping*
3. Perform Map building and Localization simulationeously using *Scan-Matching SLAM*.

---

**Scan-Matching SLAM**

This algorithm is used where the problem is of concurrent mapping and localization and it can be treated as maximum likelihood estimation problem.

The map can be represented as:

$$
m_{t} = \{< o_{t}, x_{t} >\}_{t = 0, ..., T}
$$

where:

- $m\rightarrow$ Map
- $t\rightarrow$ Time Index
- $o_{T}\rightarrow$ Laser scan
- $x_{T}\rightarrow$Pose

The goal of mapping is to find the most likely map given the data with the cost function:

$$
argmax_{m} P(m|d_{t})
$$

Detailed math can be found from equation `1`.

---

**Grid-based Fast-SLAM**

This algorithm adapts Fast-SLAM algorithm to occupancy grid map to generate volumetric representation of the environment that does not require any predefined landmark and it can therefore model arbitary types of environment.

The pseudocode is available in the paper.

---

**Rao-Blackwellized mapping**

Idea of Rao-Blackwellized particle filter for slam is to estimate a posterior $p(x_{1:t} | o_{1:t},a_{0:t})$ about potential trajectories $x_{1:t}$ of the robot given its observation $o_{1:t}$ and its odometry measurements $a_{0:t}$ and to use this afterwords to compute a posterior over maps and trajectories.

### 5. Performance Comparision

A performance comparision is done between all three algorithms. Following results were seen:

- Detected errors were small in small-dimension environments.
- Best results in terms of accuracy can be obtained in grid-based Fast-SLAM algorithm but creates a really sparse map.
- The corridors with less points of interest tend to be measured smaller than real ones for scan-matching because of maximum likelihood estimation.

### 6. Multi-robot Implementation

- The Global Map is generated using **CARMEN (Montecarlo localization)** approach.
- Robots generate their local maps and when they meet, robot A uses Robot B’s partial map and detection model to generate the global map
- Since this is grid based mapping, overlaying the partial maps can help with map merging considering robot A position as global position and relation position of Robot B to Robot A during data exchange to reorient local map of Robot B and update the global map.

## Anotated Paper

[[SLAM_and_Map_Merging.pdf]]

## Citation

```
@article{leon2009slam,
  title={SLAM and map merging},
  author={Le{\'o}n, A and Barea, Rafael and Bergasa, Luis M and L{\'o}pez, Elena and Oca{\~n}a, Manuel and Schleicher, David},
  journal={Journal of Physical Agents},
  volume={3},
  number={1},
  pages={13--23},
  year={2009}
}
```
