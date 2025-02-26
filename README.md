# Visual SLAM & Odometry Using KITTI

## Project Overview
This project explores the use of visual odometry and Simultaneous Localization and Mapping (SLAM) using the KITTI dataset, focusing on vehicle pose estimation. By employing stereo image matching techniques, we aim to calculate the vehicle's trajectory over time and compare these estimations with ground truth data to assess accuracy. Additionally, we incorporate LIDAR data to refine pose estimates and improve overall precision.

## Dataset and Sensors
The KITTI dataset, created by the Karlsruhe Institute of Technology, provides a rich collection of data collected in urban environments. This dataset includes various sensors such as:
- Stereo cameras (grayscale and RGB)
- Velodyne LIDAR
- IMU/GPS

These sensors were mounted on an autonomous vehicle named Annieway. The dataset consists of 22 sequences, and for this project, we focus on **Sequence 02**, which includes reliable ground truth data for validation.

## Coordinate Transformation and Sensor Fusion
To combine data from multiple sensors effectively, all sensor readings are transformed into a unified coordinate system. For this project, the left grayscale camera's coordinate frame is chosen as the global reference. The transformation is achieved using the projection matrices provided by the KITTI dataset, ensuring that the sensor data aligns correctly for further analysis.

![Coordinate Frame](https://github.com/Advaith41/Visual_SLAM_odometry-using-Kitti/blob/main/src/img/img_2.png)


## Methodology
The core objective is to track changes in the vehicle's position over time using visual features detected in consecutive stereo frames. The process involves depth estimation, feature tracking, and pose estimation.

### 1. Depth Estimation through Stereo Matching
Stereo matching is used to derive depth information by comparing images from the left and right cameras. The **Semi-Global Block Matching (SGBM)** algorithm is employed to compute disparity maps, which are essential for estimating depth. These disparity maps play a crucial role in 3D scene reconstruction and are critical for visual odometry.

![Depth Estimation](https://github.com/Advaith41/Visual_SLAM_odometry-using-Kitti/blob/main/src/img/img_0.png)

![SGBM](https://github.com/Advaith41/Visual_SLAM_odometry-using-Kitti/blob/main/src/img/img_0.png)



### 2. Feature Detection and Tracking
To track feature movement across frames, we utilize the **SIFT (Scale Invariant Feature Transform)** algorithm. SIFT is robust to changes in scale, rotation, and illumination, making it suitable for identifying and matching distinctive features in stereo images. These features are then tracked across consecutive frames to estimate vehicle motion.

![SIFT](https://github.com/Advaith41/Visual_SLAM_odometry-using-Kitti/blob/main/src/img/img_3.png)


### 3. Pose Estimation
Vehicle pose estimation is achieved by analyzing the movement of the features detected in consecutive frames. The displacement of these features allows for the calculation of a transformation matrix that represents the vehicle's movement in 3D space, which provides an estimate of the vehicle's position and orientation.

![Disparity](https://github.com/Advaith41/Visual_SLAM_odometry-using-Kitti/blob/main/src/img/img_4.png)


### 4. LIDAR Data for Pose Correction
To mitigate the drift that can occur with visual odometry, **LIDAR data** is incorporated to refine the pose estimates. By aligning the visual odometry results with the precise distance measurements from LIDAR, we can improve the accuracy of the vehicle's trajectory estimation.

![Visual Odometry](https://github.com/Advaith41/Visual_SLAM_odometry-using-Kitti/blob/main/src/img/img_5.png)


## Evaluation and Results

### 1. Individual Frame Analysis
The first phase involves evaluating the performance of individual algorithms (SGBM for disparity and SIFT for feature matching). The stereo images from consecutive frames are processed to calculate disparity and depth. These depth values are then compared with the LIDAR data to measure the accuracy of the stereo matching approach. Early results show good alignment with LIDAR data, though some discrepancies remain.

![Visual SLAM Image](https://github.com/Advaith41/Visual_SLAM_odometry-using-Kitti/blob/main/src/img/img_0.png)

### 2. Pose Estimation with and without LIDAR Integration
Two different methods for pose estimation are compared:

- **Pose Estimation Without LIDAR**: This approach uses stereo matching alone to estimate the vehicle's pose. While functional, this method suffers from drift over time, with estimated pose deviations of around 60 meters.
  
- **Pose Estimation With LIDAR Integration**: By incorporating LIDAR data into the estimation process, pose estimation accuracy is significantly improved. The LIDAR data helps reduce the errors in the visual odometry, bringing the estimated trajectory closer to the ground truth.

### 3. Error Analysis and Pose Refinement
Even with LIDAR data integrated, errors still persist, particularly over long sequences. For instance, in extended sequences, the vehicle’s estimated position deviated by approximately 60 meters from the ground truth. While LIDAR helps reduce drift, additional techniques like **Particle Filtering** or **Extended Kalman Filtering (EKF)** could further improve pose estimation by mitigating error accumulation over time.

## Conclusion
This project demonstrates the feasibility of using stereo matching and LIDAR data for visual odometry and SLAM in autonomous vehicles. While LIDAR integration enhances pose estimation accuracy, the results indicate that further refinement is needed for real-world applications. Techniques such as advanced sensor fusion, particle filtering, and loop closure algorithms could be explored to improve the robustness and precision of visual odometry systems in autonomous navigation.

## Future Work
Future improvements could include:
- Implementing advanced filtering methods like **Particle Filtering** or **EKF** to reduce pose drift.
- Investigating **loop closure algorithms** for better global consistency.
- Extending the project to work with all 22 sequences in the KITTI dataset for more comprehensive evaluation.

## References
- [KITTI Vision Benchmark Suite](http://www.cvlibs.net/datasets/kitti/)
- [SIFT Algorithm](https://en.wikipedia.org/wiki/Scale-invariant_feature_transform)
- [SGBM Algorithm](https://docs.opencv.org/4.x/d2/d85/classcv_1_1StereoSGBM.html)
