# 2D LiDAR SLAM using GMapping (ROS) for Indoor Occupancy Grid Mapping

This project implements a **laser-based SLAM system using the GMapping algorithm in ROS** to generate a 2D occupancy grid map of an indoor environment.

The goal of this project is to **analyze map quality before and after parameter tuning** using LiDAR scan data and evaluate how tuning improves SLAM performance.

---

## Project Overview

Simultaneous Localization and Mapping (SLAM) enables a robot to **estimate its pose while building a map of an unknown environment**.

In this project:

- A **2D SLAM pipeline** was implemented using ROS and GMapping
- Laser scan and odometry data were processed from an offline dataset
- A map was generated using **baseline parameters**
- Mapping performance was improved by **tuning SLAM parameters**
- Results were analyzed using **visual comparison of maps**

---

## Dataset

The dataset consists of recorded sensor data provided as CSV files and converted into ROS bag format.

### Data Includes:

- **Laser Scan Data** (`/front_scan`)
- **Odometry Data** (`/odom`)
- (Optional) Rear laser scan data (`/rear_scan`, not used in final mapping)

### Preprocessing Steps:

- CSV sensor data was converted into a ROS bag (`output.bag`)
- Data was published to ROS topics
- Static transforms were defined between robot frames and sensors

👉 The SLAM implementation uses **only front-facing LiDAR data** for consistent mapping performance :contentReference[oaicite:1]{index=1}  

---

## Implementation Workflow

1. ROS environment initialized (`roscore`)
2. Dataset replayed using `rosbag play --clock`
3. SLAM executed using `slam_gmapping`
4. Laser scan + odometry used for:
   - scan matching
   - pose estimation
   - map generation
5. Results visualized in RViz

---

## Results

### Map Before Parameter Tuning

![Before Tuning](Front_laser_scan.png)

This map is generated using **default GMapping parameters**.

**Observed Issues:**

- Blurred wall boundaries
- Misalignment in corners and intersections
- Cone-shaped distortions during robot rotation
- Reduced scan overlap between consecutive frames
- Accumulated pose drift over long trajectories

👉 These effects occur due to:
- limited update frequency
- odometry noise
- insufficient scan alignment

---

### Map After Parameter Tuning

![After Tuning](Tuned_front_laser.png)

This map is generated after tuning SLAM parameters.

**Key Improvement:**

- Parameter tuned: `linearUpdate = 0.5 m`

**Observed Improvements:**

- Sharper and well-defined wall boundaries
- Improved alignment of structural features
- Reduced distortion during rotations
- Better scan overlap between consecutive measurements
- More stable and consistent pose estimation

👉 Increasing update frequency allows:
- more frequent corrections
- improved scan matching
- reduced drift

---

## Parameter Tuning Strategy

The tuning focused on improving:

- Scan integration frequency
- Pose correction rate
- Scan overlap

### Key Parameter Modified:

- `linearUpdate = 0.5 m`

This ensures the map updates after shorter robot movement, improving accuracy.

---

## Key Concepts Demonstrated

- 2D LiDAR-based SLAM
- GMapping (RBPF-based SLAM)
- Laser scan processing
- Occupancy grid mapping
- ROS-based robotics pipeline
- SLAM parameter tuning
- Map quality evaluation

---

## Applications

- Autonomous mobile robots
- Indoor navigation systems
- Warehouse robotics
- Service robots
- Autonomous vehicles

---

## Tools & Environment

- Ubuntu 20.04
- ROS (Robot Operating System)
- GMapping package
- RViz (visualization)
- python
- ROS bag processing

---

## Files Description

### README.md
Project overview, methodology, and results visualization.

### CPE-521 final project.pdf
Detailed report including:
- SLAM algorithm explanation
- Implementation workflow
- Parameter tuning strategy
- Performance analysis

### CPE-521-Data.zip
Dataset containing:
- Laser scan data
- Odometry data
- Mapping inputs used in the experiment

---

## Conclusion

This project demonstrates that **parameter tuning plays a critical role in SLAM performance**.

- Baseline mapping results contain distortions and noise
- After tuning, maps become significantly:
  - clearer
  - more accurate
  - structurally consistent

👉 Even without changing the algorithm, **proper parameter selection greatly improves mapping quality**

---

## Author

Bhagyath Badduri  
Master of Engineering in Robotics  
Stevens Institute of Technology  
