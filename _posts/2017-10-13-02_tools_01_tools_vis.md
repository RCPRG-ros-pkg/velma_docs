---
layout: task
title: Tools for visualization
category: lab
lab: 3
task: 1
brief: 
---

This section contains information about various tools for visualization of Velma control system and state of simulation.




# Control system state and structure

### Location
 * script: `rqt_agent` in package `rqt_agent`

### Command
```bash
rosrun rqt_agent rqt_agent
```

### Description
*rqt_agent* is a GUI tool that shows structure and state of low-level subsystems of the control system.
It can be use to examine, for each subsystem:
 * structure of FSM (button *FSM graph*),
 * the current state of FSM and behaviour,
 * period,
 * history of state switches of FSM (button *State history*),
 * state of Orocos components (button *Components list*),
 * data flow diagram of Orocos components (button *Behavior graph*).

![]({{site.baseurl}}/public/img/velma_rqt_agent.png)





# Internal collisions

### Location
 * script: `show_collisions` in package `common_core_cs_components`
 * launch file for Velma: `show_collisions.launch` in package `velma_common`

### Command
```bash
roslaunch velma_common show_collisions.launch
```

### Description
It publishes visual markers that visualize collision geometries and internal collisions.
The markers can be visualized in ROS *rviz* using topic `/self_collisions`.
The green shapes are collision geometries and red arrows denote closeness of links.
Also, the script outputs to the terminal names of pairs of close links and their distance.

![]({{site.baseurl}}/public/img/velma_show_collisions.png)






# Camera frustum

### Location

 * script: `publish_camera_frustum` in package `rcprg_ros_utils`
 * launch file for Velma: `show_kinect_frustum.launch` in package `velma_common`

### Command
```bash
roslaunch velma_common show_kinect_frustum.launch
```

### Description

It publishes visual markers that visualize camera frustum of Kinect mounted on head of Velma.
The markers can be visualized in ROS *rviz* using topic `/camera_frustum`.

![]({{site.baseurl}}/public/img/velma_show_camera_frustum.png)




# Joints position and range

### Location

 * script: `publish_joints_visualization` in package `rcprg_ros_utils`
 * launch file for Velma: `show_joints.launch` in package `velma_common`

### Command
```bash
roslaunch velma_common show_joints.launch
```

### Description

It publishes visual markers that visualize position and range of joints of Velma.
The markers can be visualized in ROS *rviz* using topic `/joints_vis`.
Each red arrow represent axis of joint, blue arrow represent position of joint in range shown as a green plate.

![]({{site.baseurl}}/public/img/velma_show_joints.png)






# Range of the end effectors

### Location

 * script: `reachability_range` in package `rcprg_ros_utils`
 * launch file for Velma: `show_reachability_range.launch` in package `velma_common`

### Command
```bash
roslaunch velma_common show_reachability_range.launch
```

### Description

It publishes visual markers that visualize range (i.e. reachable space) of the end effectors of Velma.
The markers can be visualized in ROS *rviz* using topic `/reachability_range`.
Range of one end effector is visualized approximately as two coaxial spheres. The end effector can move in space between the spheres.
Ranges of the right and left end effectors are visualized in green and red colors respectively.

Please note that near the boundary of the reachable space (i.e. close to either inner or outer sphere),
the motion of end effector can be restricted and some orientations cannot be reached.

![]({{site.baseurl}}/public/img/velma_show_reachability_range.png)







# Pose of a frame

### Location

 * script: `pose_int_marker` in package `rcprg_ros_utils`
 * launch file: `pose_int_marker.launch` in package `rcprg_ros_utils`

### Example command

```bash
 roslaunch rcprg_ros_utils pose_int_marker.launch frame_id:=example_frame
```

### Description
This tool creates an interactive marker in ROS *rviz* with 6 DOF.
It can be moved in *rviz* and its pose (i.e. position and orientation) is printed in the terminal.

![]({{site.baseurl}}/public/img/pose_int_marker.png)





# Pose of an object in Gazebo simulation

### Location

 * script: `gazebo_publish_ros_tf_object` in package `rcprg_gazebo_utils`
 * launch file: `gazebo_publish_ros_tf_object.launch` in package `rcprg_gazebo_utils`

### Example command

```bash
roslaunch rcprg_gazebo_utils gazebo_publish_ros_tf_object.launch link_name:="velma::right_HandFingerOneKnuckleTwoLink" frame_id:=frame_01
```

### Description

It publishes ROS tf (i.e. transform on /tf topic) of an object in Gazebo simulation.
The launch file has two obligatory arguments:
 * *link_name* is the name of the object in simulation, e.g. if we want to publish pose of link *link_1* of model *robot_1*, the link name is *robot_1::link_1*,
 * *frame_id* is the unique name of the frame we want to publish to.






# Interactive visualization of kinematics

### Location

 * launch file: `description_test.launch` in package `velma_description`

### Command

```bash
roslaunch velma_description description_test.launch
```

### Description

This launch file should be executed when the system is NOT running.
It provides a control panel with slides that can move single joints of Velma.



