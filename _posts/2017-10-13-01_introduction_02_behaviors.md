---
layout: task
title: Behaviors of core agent
category: lab
lab: 1
task: 2
brief: Specification of behaviors of subsystems of core agent.
---

## Behaviors

Each subsystem realizes one or multiple behaviors.
The behavior switching is governed by a Finite State Machine (FSM), so that one behavior can be executed in one or multiple states.

## Virtual effector *velma_core_ve_body*
FSM of velma_core_ve_body subsystem:

![]({{site.baseurl}}/public/img/velma_core_ve_body_fsm.png)

The conditions are:

* $$\scriptsize \sigma_{\texttt{safe},\texttt{safe_st}} = \texttt{recvStatus} \wedge \neg \texttt{allHwOk}$$
* $$\scriptsize \sigma_{\texttt{safe},\texttt{safe_st_ok}} = \texttt{recvStatus} \wedge \texttt{allHwOk}$$
* $$\scriptsize \sigma_{\texttt{safe},\texttt{idle}} = \neg \texttt{recvStatus}$$
* $$\scriptsize \tau_{\texttt{safe}} = \texttt{TRUE}$$
* $$\scriptsize \epsilon_{\texttt{safe}} = \texttt{FALSE}$$
* $$\scriptsize \sigma_{\texttt{safe_st},\texttt{safe}} = \texttt{TRUE} $$
* $$\scriptsize \tau_{\texttt{safe_st}} = \texttt{TRUE}$$
* $$\scriptsize \epsilon_{\texttt{safe_st}} = \texttt{FALSE}$$
* $$\scriptsize \sigma_{\texttt{safe_st_ok},\texttt{safe}} = \neg \texttt{recvCommand} \vee \neg \texttt{allCmdOk} \vee \neg \texttt{cmdExitSafeState} \vee \neg \texttt{safeItPassed500} $$
* $$\scriptsize \sigma_{\texttt{safe_st_ok},\texttt{transp}} = \texttt{recvCommand} \wedge \texttt{allCmdOk} \wedge \texttt{cmdExitSafeState} \wedge \texttt{safeItPassed500} $$
* $$\scriptsize \tau_{\texttt{safe_st_ok}} = \texttt{TRUE}$$
* $$\scriptsize \epsilon_{\texttt{safe_st_ok}} = \texttt{FALSE}$$
* $$\scriptsize \sigma_{\texttt{transp},\texttt{safe_st}} = \texttt{recvStatus} \wedge \neg \texttt{allHwOk}$$
* $$\scriptsize \sigma_{\texttt{transp},\texttt{transp_st}} = \texttt{recvStatus} \wedge \texttt{allHwOk} $$
* $$\scriptsize \sigma_{\texttt{transp},\texttt{idle}} = \neg \texttt{recvStatus}$$
* $$\scriptsize \tau_{\texttt{transp}} = \texttt{TRUE}$$
* $$\scriptsize \epsilon_{\texttt{transp}} = \texttt{FALSE}$$
* $$\scriptsize \sigma_{\texttt{transp_st},\texttt{transp}} = \texttt{recvCommand} \wedge \texttt{allCmdOk}$$
* $$\scriptsize \sigma_{\texttt{transp_st},\texttt{safe}} = \neg \texttt{recvCommand} \vee \neg \texttt{allCmdOk}$$
* $$\scriptsize \tau_{\texttt{transp_st}} = \texttt{TRUE}$$
* $$\scriptsize \epsilon_{\texttt{transp_st}} = \texttt{FALSE}$$
* $$\scriptsize \sigma_{\texttt{idle},\texttt{safe_st}} = \texttt{recvStatus}$$
* $$\scriptsize \tau_{\texttt{idle}} = \texttt{recvStatus}$$
* $$\scriptsize \epsilon_{\texttt{idle}} = \texttt{FALSE}$$

With predicates:

* $$\scriptsize \texttt{recvStatus}$$ - new status data was received from real effectors,
* $$\scriptsize \texttt{recvCommand}$$ - new command data was received from velma_core_cs,
* $$\scriptsize \texttt{safeItPassed500}$$ - the current number of non-interrupted iterations in state safe or safe_st or safe_st_ok is over 500,
* $$\scriptsize \texttt{cmdExitSafeState}$$ - new command was received from velma_core_cs: exit safe state
* $$\scriptsize \texttt{allHwOk} = \texttt{rLwrOk} \wedge \texttt{lLwrOk} \wedge \texttt{rLwrInCmdState} \wedge \texttt{lLwrInCmdState} \wedge \texttt{tMotorOk} \wedge \texttt{hpMotorOk} \wedge \texttt{htMotorOk}$$ - all real
effectors are working properly
* $$\scriptsize \texttt{allCmdOk} = \texttt{rLwrCmdOk} \wedge \texttt{lLwrCmdOk} \wedge \texttt{tCmdOk}$$ - all received commands are valid

### Control subsystem *velma_core_cs*

FSM of velma_core_cs subsystem:

![]({{site.baseurl}}/public/img/velma_core_cs_fsm.png)

The conditions are:

* $$\scriptsize \tau_{\texttt{idle}} = \texttt{veBodyStatusValid} \wedge \neg \texttt{veBodyInSafeState} $$
* $$\scriptsize \epsilon_{\texttt{idle}} = \texttt{FALSE} $$
* $$\scriptsize \sigma_{\texttt{idle},\texttt{safe}} = \texttt{veBodyStatusValid} \wedge \neg \texttt{veBodyInSafeState} $$

* $$\scriptsize \tau_{\texttt{safe}} = \texttt{recvOneCmd} \wedge ((( \texttt{recvCartImpCmd} \vee \texttt{recvJntImpCmd} ) \wedge \neg \texttt{inSelfCollision} ) \vee \texttt{recvSafeColCmd} ) \wedge \texttt{motorsReady} $$
* $$\scriptsize \epsilon_{\texttt{safe}} = \neg \texttt{CURRENT_BEHAVIOR_OK} \vee \neg \texttt{veBodyStatusValid} \vee \texttt{veBodyInSafeState} $$
* $$\scriptsize \sigma_{\texttt{safe},\texttt{idle}} = \texttt{IN_ERROR} $$
* $$\scriptsize \sigma_{\texttt{safe},\texttt{cart_imp}} = \neg \texttt{IN_ERROR} \wedge \texttt{recvOneCmd} \wedge \texttt{recvCartImpCmd} $$
* $$\scriptsize \sigma_{\texttt{safe},\texttt{jnt_imp}} = \neg \texttt{IN_ERROR} \wedge \texttt{recvOneCmd} \wedge \texttt{recvJntImpCmd} $$
* $$\scriptsize \sigma_{\texttt{safe},\texttt{safe_col}} = \neg \texttt{IN_ERROR} \wedge \texttt{recvOneCmd} \wedge \texttt{recvSafeColCmd} $$

* $$\scriptsize \tau_{\texttt{safe_col}} = \texttt{recvOneCmd} \wedge ( \texttt{recvCartImpCmd} \vee \texttt{recvJntImpCmd} ) \wedge \neg \texttt{inSelfCollision} $$
* $$\scriptsize \epsilon_{\texttt{safe_col}} = \neg \texttt{CURRENT_BEHAVIOR_OK} \vee \neg \texttt{veBodyStatusValid} \vee \texttt{veBodyInSafeState}$$
* $$\scriptsize \sigma_{\texttt{safe_col},\texttt{idle}} = \texttt{IN_ERROR} $$
* $$\scriptsize \sigma_{\texttt{safe_col},\texttt{cart_imp}} = \neg \texttt{IN_ERROR} \wedge \texttt{recvOneCmd} \wedge \texttt{recvCartImpCmd} $$
* $$\scriptsize \sigma_{\texttt{safe_col},\texttt{jnt_imp}} = \neg \texttt{IN_ERROR} \wedge \texttt{recvOneCmd} \wedge \texttt{recvJntImpCmd} $$

* $$\scriptsize \tau_{\texttt{cart_imp}} = ( \texttt{recvOneCmd} \wedge ( \texttt{recvJntImpCmd} \vee \texttt{recvSafeColCmd} )) \vee \texttt{inSelfCollision} $$
* $$\scriptsize \epsilon_{\texttt{cart_imp}} = \neg \texttt{CURRENT_BEHAVIOR_OK} \vee \neg \texttt{veBodyStatusValid} \vee ( \texttt{veBodyInSafeState} )$$
* $$\scriptsize \sigma_{\texttt{cart_imp},\texttt{idle}} = \texttt{IN_ERROR} $$
* $$\scriptsize \sigma_{\texttt{cart_imp},\texttt{safe}} = \neg \texttt{IN_ERROR} \wedge \neg \texttt{recvOneCmd} $$
* $$\scriptsize \sigma_{\texttt{cart_imp},\texttt{jnt_imp}} = \neg \texttt{IN_ERROR} \wedge \texttt{recvOneCmd} \wedge \texttt{recvJntImpCmd} $$
* $$\scriptsize \sigma_{\texttt{cart_imp},\texttt{safe_col}} = \neg \texttt{IN_ERROR} \wedge \texttt{recvOneCmd} \wedge \texttt{recvSafeColCmd}$$ 

* $$\scriptsize \tau_{\texttt{jnt_imp}} = ( \texttt{recvOneCmd} \wedge ( \texttt{recvCartImpCmd} \vee \texttt{recvSafeColCmd} )) \vee \texttt{inSelfCollision} $$
* $$\scriptsize \epsilon_{\texttt{jnt_imp}} = \neg \texttt{CURRENT_BEHAVIOR_OK} \vee \neg \texttt{veBodyStatusValid} \vee ( \texttt{veBodyInSafeState} )$$
* $$\scriptsize \sigma_{\texttt{jnt_imp},\texttt{idle}} = \texttt{IN_ERROR} $$
* $$\scriptsize \sigma_{\texttt{jnt_imp},\texttt{safe}} = \neg \texttt{IN_ERROR} \wedge \neg \texttt{recvOneCmd} $$
* $$\scriptsize \sigma_{\texttt{jnt_imp},\texttt{cart_imp}} = \neg \texttt{IN_ERROR} \wedge \texttt{recvOneCmd} \wedge \texttt{recvCartImpCmd} $$
* $$\scriptsize \sigma_{\texttt{jnt_imp},\texttt{safe_col}} = \neg \texttt{IN_ERROR} \wedge \texttt{recvOneCmd} \wedge \texttt{recvSafeColCmd} $$


TODO: text

| TODO: table    | TODO   |
| TODO: table    | TODO   |
| TODO: table    | TODO   |

## TODO

 * **TODO** - list 1
 * **TODO** - list 2

[comment]: ![]({{site.baseurl}}/public/l1/pattern_explained.png)

## Do poczytania
TODO

[comment]: * Rozdział 11: _Camera models and calibration_ (str. 270-404) z książki _Learning OpenCV. Computer Vision with the OpenCV Library_, Gary Bradski and Adrian Kaehler, O'Reilly Media, 2008
[comment]: * OpenCV reference manual: [Camera Calibration and 3D Reconstruction](http://docs.opencv.org/2.4/modules/calib3d/doc/camera_calibration_and_3d_reconstruction.html)
[comment]:  * [findChessboardCorners](http://docs.opencv.org/2.4/modules/calib3d/doc/camera_calibration_and_3d_reconstruction.html#findchessboardcorners)
[comment]:  * [findCirclesGrid](http://docs.opencv.org/2.4/modules/calib3d/doc/camera_calibration_and_3d_reconstruction.html#findcirclesgrid)
[comment]:  * [calibrateCamera](http://docs.opencv.org/2.4/modules/calib3d/doc/camera_calibration_and_3d_reconstruction.html#calibratecamera)
[comment]:  * [undistort](http://docs.opencv.org/2.4/modules/imgproc/doc/geometric_transformations.html#undistort)

