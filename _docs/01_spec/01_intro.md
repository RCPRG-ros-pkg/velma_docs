---
title: Introduction
category: Specification
order: 1
brief: Introduction to control system, agents, subsystems, basics of operation. The scope of this section is crucial for even basic understanding of the system.
---

## About

The control system of Velma robot is a complex, hierarchical structure.
It is based on the Embodied Agent Specification Method developed at
[Institute of Control & Computation Engineering at WARSAW UNIVERSITY OF TECHNOLOGY](https://www.robotyka.ia.pw.edu.pl/).

**Please note:** The scope of this section is crucial for even basic understanding of the system.

## Structure of the system

The structure of the control system is dependent on tasks realized by robot.
However, some parts are common for different tasks.
Two common agents can be distinguished in the control system:

* *velma_core* - embodied agent, the robot hardware and its low-level controller,
* *velma_ros_interface* - computational agent; ROS interface for *velma_core*.

![Simulated system]({{ site.baseurl }}/images/agents.dot.png "Simulated system")

*Common agents in the control system*

Realization of a task require another agents to be included into the system.
They can communicate with *velma_ros_interface* using ROS communication methods (i.e. topics, actions and services).
Direct, bidirectional communication with *velma_core* agent must be avoided,
however it is possible to exchange *velma_ros_interface* with another agent that
can serve as interface between *velma_core* and computational agents.

![Simulated system]({{ site.baseurl }}/images/agents_extended.dot.png)

*Example of the control system of Velma with common agents (velma_core, velma_ros_interface) and computational agents (A,B,C)*

## Deeper look into the system

The agent *velma_core* consists of three abstraction levels.
These levels correspond to types of subsystems of an embodied agent distinguished in the Specification Method:

* control subsystem,
* virtual effectors and receptors,
* real effectors and receptors.

In *velma_core* agent, each level contains at least one subsystem.
Additionally, each subsystem is implemented as a separate process in operating system.
Such distributed structure has some advantages:

* subsystems can be easily replaced, if the replacements have the same communication buffers;
in particular, hardware-operating subsystems can be replaced with simulation subsystem,
thus both real-hardware and simulated systems share utmost part,
* critical error in one subsystem does not affect other subsystems,
so they can react adequately to keep proper operation of the system,
* lower subsystems in the hierarchy are simpler and thus more error proof,
they can react immediately to errors in upper subsystems.

The common structure of the simulated system is:

![Simulated system]({{ site.baseurl }}/images/system.dot.png)

*Simulated system*

The system consists of at least two agents: *velma_core* and *velma_ros_interface*.
The embodied agent *velma_core* is build of three subsystems: *velma_core_cs*, *velma_core_ve_body* and *velma_sim_gazebo*.
The computational agent *velma_ros_interface* consists of one subsystem: *velma_task_cs_ros_interface*.
The latter provides interfaces to ROS, thus other computational, ROS-based agents can be utilized in the system.

The common structure of the real-hardware system is similar:

![]({{ site.baseurl }}/images/system_hw.dot.png)

*HW system*

The only difference between them is in the lowest level of abstraction.
Subsystems that communicate with hardware are exchangeable with one subsystem that simulates the whole robot.
So basically, in the simulated system, the simulator (Gazebo) runs in the process of *velma_sim_gazebo* subsystem, while in the real-hardware system
*velma_ec_driver*, *velma_core_re_lwr_r* and *velma_core_re_lwr_l* communicate with hardware devices of robot.

## Basics of operation

Real-time (RT) control loop is implemented in *velma_core* agent.
In each iteration of the control loop the following sequence is executed:

[comment]: * the state of the robot (i.e. its effectors and receptors) is read in real effectors/receptors
[comment]: (i.e. *velma_sim_gazebo* in simulated system or *velma_ec_driver*, *velma_core_re_lwr_r* and *velma_core_re_lwr_l* in HW system)
[comment]: and then, it is sent to virtual effector *velma_core_ve_body*; additionally the last read commands from virtual effector *velma_core_ve_body* are
[comment]: sent to HW devices (or to the simulated robot),

1. *velma_core_ve_body* reads status from real effectors/receptors
(i.e. *velma_sim_gazebo* in simulated system or *velma_ec_driver*, *velma_core_re_lwr_r* and *velma_core_re_lwr_l* in HW system)
and sends it immediately to control subsystem *velma_core_cs*,
1. the control subsystem *velma_core_cs* reads commands from task agent *velma_ros_interface* and closes the control loop by calculating desired control, and sends it back to virtual effector *velma_core_ve_body*,
1. the virtual effector *velma_core_ve_body* is executed second time in the current iteration, but this time it does not read new status from real effectors/receptors;
instead, it reads commands from *velma_core_cs*,
1. then, *velma_core_ve_body* verifies the last read status of real effectors/receptors and the current commands from *velma_core_cs*, process them and sends commands to
real effectors.

### *velma_core_cs*

The most computation in the RT control loop is done in *velma_core_cs*.
This subsystem realizes complex operations, such as:

* trajectory generation,
* control law implementation (i.e. computation of low-level controls),
* collision checking.

It implements most of high-level behaviors, i.e.:

* *cart_imp* - impedance control in the joint space,
* *jnt_imp* - impedance control in the operational space,
* *safe_col* - self collision repulsive behavior,
* *safe* - safe behavior: impedance control in joint space with very low stiffness and high damping,
* *idle* - do nothing and wait until virtual effector exits safe state.

Those behaviors involve control of whole body and takes into account dependencies between links and joints of the robot.
Each of these behaviors is realized in exactly one state of Finite State Machine (FSM) of the subsystem.
The behaviors and FSM is described in more detail in [next section]({{ site.baseurl }}/01_spec/03_behaviors).

### *velma_core_ve_body*

Virtual effector *velma_core_ve_body* has much simpler functionality.
It does not take into account all dependencies between parts of the robot, but it controls single joints separately.
It basically realizes two main modes of operation:

* *transparent* - i.e. pass status from real effectors to control subsystem and pass commands in the opposite direction,
* *safe* - apply high damping for each joint separately; when virtual effector is in this state, the control subsystem is in *idle* state.

Those two *modes of operation* should not be confused with behaviors of the virtual effector defined using the specification method.
They are realized by five behaviors in five states of the FSM,
described in [next section]({{ site.baseurl }}/01_spec/03_behaviors).

[comment]: ## Bibliography
[comment]: TODO

[comment]: * Rozdział 11: _Camera models and calibration_ (str. 270-404) z książki _Learning OpenCV. Computer Vision with the OpenCV Library_, Gary Bradski and Adrian Kaehler, O'Reilly Media, 2008
[comment]: * OpenCV reference manual: [Camera Calibration and 3D Reconstruction](http://docs.opencv.org/2.4/modules/calib3d/doc/camera_calibration_and_3d_reconstruction.html)
[comment]:  * [findChessboardCorners](http://docs.opencv.org/2.4/modules/calib3d/doc/camera_calibration_and_3d_reconstruction.html#findchessboardcorners)
[comment]:  * [findCirclesGrid](http://docs.opencv.org/2.4/modules/calib3d/doc/camera_calibration_and_3d_reconstruction.html#findcirclesgrid)
[comment]:  * [calibrateCamera](http://docs.opencv.org/2.4/modules/calib3d/doc/camera_calibration_and_3d_reconstruction.html#calibratecamera)
[comment]:  * [undistort](http://docs.opencv.org/2.4/modules/imgproc/doc/geometric_transformations.html#undistort)

