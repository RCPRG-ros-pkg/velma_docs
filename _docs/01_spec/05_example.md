---
title: Example system
category: Specification
order: 5
brief: An example system.
---

## About the example

The presented system consists of multiple agents and it can realize complex tasks,
such as environment exploration, exerting forces on environment, grasping and manipulation.
The presented structure is common for both simulated and real HW setup.

## Structure of the system

The structure of the control system is dependent on tasks realized by robot.
However, some parts are common for different tasks.
Agents used in this example system are:

* *core/a* - embodied agent, the robot hardware and its low-level controller.
* *ros_interface/a* - computational agent; ROS interface between *velma_core* and other agents.
* *vision/a* - computational agent that processes data acquired from kinect and virtual effector to recognize and localize objects in environment and builds occupancy map of environment represented as octomap; the map contains information about both explored and occupied space, and unexplored space.
* *planner/a* - computational agent used for geometric planning of robot motion in joint space.
* *task/a* - computational agent that acquires high level information about the state of robot and environment, and commands the robot to
realize a specified task.

![]({{ site.baseurl }}/images/example_system.png)


Communication between *ros_interface/a*, *vision/a*, *planner/a* and *task/a* agents is realized using ROS communication methods
(i.e. topics, actions and services).
Agents *ros_interface/a* and *planner/a* are implemented as ROS nodes and can be easily accessed from *task/a*:

* through Python API, if *task/a* is written in Python,
* through ROS topics and services, if *task/a* is written in another language.

Agent *vision* is strictly dependent on task, so it can be implemented in many ways.
In simulation, the agent can use exact pose data obtained from simulator to emulate visual localization algorithms.

## Documentation for API, tools and tutorials

Detailed documentation of *ros_interface/a*, *vision/a* and *planner/a* agents is presented in section
[Python interfaces]({{ site.baseurl }}/01_python_interface).

