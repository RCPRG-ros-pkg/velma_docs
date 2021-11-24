---
title: Introduction
category: Specification
order: 1
brief: Introduction to control system, agents, subsystems, basics of operation. The scope of this section is crucial for even basic understanding of the system.
---

## About

The control system of Velma robot is a complex, hierarchical structure.
It is based on the Embodied Agent concept developed at
[Institute of Control & Computation Engineering at WARSAW UNIVERSITY OF TECHNOLOGY](https://www.robotyka.ia.pw.edu.pl).

Specification is expressed using [EARL](https://www.robotyka.ia.pw.edu.pl/projects/earl),
a SysML based language for robotic systems based on Embodied Agent concept.

WUT Velma robot is unique, but it is built out of standard items:

  * KUKA LWR 4+ manipulators
  * Elmo servo controllers
  * cameras: MS Kinect, BlackFly
  * 6-axis force/torque sensors (ATI)
  * BarrettHand grippers

It is implemented using popular open source libraries and frameworks:

  * [ROS 1](https://www.ros.org)
  * [Orocos](https://orocos.org)
  * [Gazebo](http://gazebosim.org)

and it is specified using formal languages:

  * SysML, EARL
  * Embodied Agent concept
  * doxygen (Python API docs)
