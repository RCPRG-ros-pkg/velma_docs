---
layout: task
title: Other
category: lab
lab: 3
task: 3
brief: 
---

This section contains information about various tools for task execution.






# Visual markers tracking

### Location

 * launch file: `ar_track_kinect_50.launch` in package `velma_common`

### Command

```bash
roslaunch velma_common ar_track_kinect_50.launch
```

### Description

This launch file creates a ROS node that recognizes and tracks visual markers in camera image.







# Running online octomap server

### Location

 * launch file: `octomap_server.launch` in package `velma_common`

### Command

```bash
roslaunch velma_common octomap_server.launch
```

### Description
TODO







# Running offline octomap server

### Location

 * launch file: `octomap_offline_server.launch` in package `velma_common`

### Command

```bash
roslaunch velma_common octomap_offline_server.launch
```

### Description
TODO










# Moving the end effectors using interactive markers

### Location

 * script: `int_markers_cimp` in package `velma_common`

### Command

```bash
rosrun velma_common int_markers_cimp
```

### Description

This script publishes interactive 6 DOF markers that can be used inside *rviz*  to move around the end effectors of Velma.
ROS topics for interactive markers are:
 * for the right end effector: `/int_markers_cimp_right/update`
 * for the left end effector: `/int_markers_cimp_left/update`

![]({{site.baseurl}}/public/img/velma_int_markers_cimp.png)







# Moving an object in Gazebo using interactive 6 DOF marker

### Location

 * script: `gazebo_move_object` in package `rcprg_gazebo_utils`

### Example command

```bash
rosrun rcprg_gazebo_utils gazebo_move_object jar_01::link
```

### Description

This script publishes interactive 6 DOF marker that can be used in *rviz* to move a specified object in Gazebo.
Please note that motion of the object should not be constrained by any kinematics bonds, i.e. it should be a free-floating object.


