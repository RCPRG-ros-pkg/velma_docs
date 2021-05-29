---
title: FABRIC
category: Implementation
order: 1
brief: Documentation of WUT Velma
---

The *core* agent and *ros_interface* agent are implemented with
[FABRIC](http://dx.doi.org/10.1109/RoMoCo.2019.8787370).

Each subsystem is a separate process. The IPC is realized with synchronized shared memory buffers,
and the data format is specified with ROS message. Transition function is realized with a graph
of Orocos components.

Some parts of source codes are automatically generated from specification written in xml.
FABRIC subsystems are organized in catkin packages as follows:
* a file *spec/subsystem.xml* contains specification used for automatic code generation
  (behaviours, FSM, definitions of buffers)
* xml files in *config* folder contain run-time configurations for subsystems (graph of Orocos
  components)
* launchfiles in *launch* folder are used for deployment
* source code files are in folder *src* (code for predicates and for some subsystem-specific
  components)

The packages for FABRIC subsystems are:
* [velma_task_cs_ros_interface](https://github.com/RCPRG-ros-pkg/velma_system/tree/melodic-devel/velma_task_cs_ros_interface)
* [velma_core_cs](https://github.com/RCPRG-ros-pkg/velma_system/tree/melodic-devel/velma_core_cs)
* [velma_core_ve_body](https://github.com/RCPRG-ros-pkg/velma_system/tree/melodic-devel/velma_core_ve_body)
* [velma_sim_gazebo](https://github.com/RCPRG-ros-pkg/velma_sim_gazebo) - subsystem for simulated
  robot in simulated environment (Gazebo)
* [velma_sim](https://github.com/RCPRG-ros-pkg/velma_system/tree/melodic-devel/velma_sim) - subsystem
  for simulated torso and head, for tests in real world (not Gazebo)
* [velma_lwr_sim](https://github.com/RCPRG-ros-pkg/velma_system/tree/melodic-devel/velma_lwr_sim) -
  subsystems for simulated arms (left, right), for tests in real world (not Gazebo)
* [velma_core_re_lwr](https://github.com/dseredyn/velma_core_re_lwr) - subsystems for H/W arms (left, right)

There is also a package for subsystem for EtherCAT devices (torso, neck, grippers and some sensors):
[velma_ec_driver](https://github.com/RCPRG-ros-pkg/velma_ec_driver), but it does not use FABRIC.
