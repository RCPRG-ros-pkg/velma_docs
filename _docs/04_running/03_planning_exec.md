---
title: Other
category: Running
order: 3
brief: 
---

This section contains information about various tools for task execution.






# Visual markers tracking

**Command**

```bash
roslaunch velma_common ar_track_kinect_50.launch
```

**Description**

This launch file creates a ROS node that recognizes and tracks visual markers in camera image.







# Running online octomap server

**Command**

```bash
roslaunch velma_common octomap_server.launch
```

**Description**

This launchfile runs octomap node, which gathers information about occupancy of environment.
Also, unknown (unexplored) space can be obtained.

Example of octomap of known, occupied space - floor is visible (ROS topic: */occupied_cells_vis_array*):
![]({{ site.baseurl }}/images/velma_octomap_occupied.png)

Example of octomap of unknown space (ROS topic: */unknown_cells_vis_array*):
![]({{ site.baseurl }}/images/velma_octomap_unknown.png)

Example of octomap of known, free space (ROS topic: */free_cells_vis_array*):
![]({{ site.baseurl }}/images/velma_octomap_free.png)

Example of merged octomap, i.e. sum of occupied space and unknown space (ROS topic: */merged_cells_vis_array*):
![]({{ site.baseurl }}/images/velma_octomap_merged.png)
Planner uses the merged octomap to generate trajectories that avoid both occupied and unknown space.





# Running offline octomap server

**Command**

```bash
roslaunch velma_common octomap_offline_server.launch
```

**Description**

This launch file runs octomap server that uses saved octomap. The octomap in offline server cannot be updated with new observations.







# Moving the end effectors using interactive markers

**Command**

```bash
rosrun velma_common int_markers_cimp.py
```

**Description**

This script publishes interactive 6 DOF markers that can be used inside *rviz*  to move around the end effectors of Velma.
ROS topics for interactive markers are:
 * for the right end effector: `/int_markers_cimp_right/update`
 * for the left end effector: `/int_markers_cimp_left/update`

![]({{ site.baseurl }}/images/velma_int_markers_cimp.png)







# Moving an object in Gazebo using interactive 6 DOF marker

**Example command**

```bash
rosrun rcprg_gazebo_utils gazebo_move_object.py jar_01::link
```

**Description**

This script publishes interactive 6 DOF marker that can be used in *rviz* to move a specified object in Gazebo.
Please note that motion of the object should not be constrained by any kinematics bonds, i.e. it should be a free-floating object.


