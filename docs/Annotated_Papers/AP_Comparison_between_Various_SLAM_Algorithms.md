---
id: AP_Comparison_between_Various_SLAM_Algorithms
aliases:
  - Comparison between Various SLAM Algorithms
tags: []
---

# Comparison between Various SLAM Algorithms

## LOAM: LIDAR Odometry and Mapping in Real-time

> Paper Link: [here](https://www.ri.cmu.edu/pub_files/2014/7/Ji_LidarMapping_RSS2014_v8.pdf)

> GitHub Repository Link: There is no direct implementation available online but an advanced version can be found at [HKUST-Aerial-Robotics/A-LOAM](https://github.com/HKUST-Aerial-Robotics/A-LOAM)

### Sensor Suite

- IMU
- LIDAR

### Cons

- Suffers from the issues of ghosting based on LIDAR Scan Error resulting in reduced dimensional accuracy.
- Pitch and Roll rotations tends to break the Mapping.
- Close Loop Algorithm (non-existent) suffers an issue with distortion in low feature areas such as hallways. Results in incorrect geometry.

### Pros

- Real-Time implementation due to small sensor suite and higher approximations for reduced sampling.


## LIOM: Tightly Coupled 3D LIDAR Inertial Odometry and Mapping

> Paper Link: [here](https://arxiv.org/abs/1904.06993)

> GitHub Repository Link: [hyye/lio-mapping](https://github.com/hyye/lio-mapping)

> Website: [here](https://sites.google.com/view/lio-mapping)

### Sensor Suite

- IMU
- LIDAR

### Cons

- Improved dimensional accuracy than LOAM but still contains significant ghosting.
- Improved Distortion in narrow hallways but still suffers from mapping error for long hallways which are featureless.

##  LIO-SAM: Tightly-coupled LIDAR Inertial Odometry via Smoothing and Mapping

> Paper Link: [here](https://ieeexplore.ieee.org/document/9341176)

> GitHub Repository Link: [TixiaoShan/LIO-SAM](https://github.com/TixiaoShan/LIO-SAM)

### Sensor Suite

- IMU
- LIDAR
- GPS

### Cons

- Still suffers from issue with featureless areas which uses GPS for correction. This results in failure in GPS constraint or noisy readings.

## VINS-Mono: A Robust and Versatile Monocular Visual-Inertial State Estimator

> Paper Link: [here](https://ieeexplore.ieee.org/document/8421746)

> GitHub Repository Link: [HKUST-Aerial-Robotics/VINS-Mono](https://github.com/HKUST-Aerial-Robotics/VINS-Mono)

### Sensor Suite

- IMU
- 1 x Monocular Camera

### Cons

- Since this algorithm depends on feature-based SLAM, it fails for long hallways or open areas which lacks features.
- High Paced Motion results in distortion and Mapping Failure due to limitation of the processor to find features in images.
- Dependence on camera requires heavy calibration for intrinsic and extrinsic parameters of sensors.

## EMV-LIO: An Efficient Multiple Vision aided LiDAR-Inertial Odometry

> Paper Link: [here](https://arxiv.org/abs/2302.00216)

> GitHub Repository Link: [BingqiShen/EMV-LIO](https://github.com/BingqiShen/EMV-LIO)

### Sensor Suite

- IMU
- Cameras
- LIDAR

### Cons

- Open-Source Codebase suffers from high number of Magic Numbers for transforms and values. Needs heavy modification to be used for your application.
- Algorithm Fails for Big Maps due to ROS Node not being able to handle data higher than 1 GB on a node. 
- Built using the combination of VINS-Mono and LIO-SAM which has a bad intertwined Transform Tree. Modifying the code requires a lot of effort.
