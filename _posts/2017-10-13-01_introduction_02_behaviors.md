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
The behavior switching is governed by a Finite State Machine (FSM), so that one behavior is executed in one state of the FSM.

## *velma_core_ve_body*
FSM of *velma_core_ve_body* subsystem:

![]({{site.baseurl}}/public/img/velma_core_ve_body_fsm.png)

All states, except *idle* last exactly one iteration, so their terminal conditions are:

$$\scriptsize \tau_{\texttt{safe}} = \tau_{\texttt{safe_st}} = \tau_{\texttt{safe_st_ok}} = \tau_{\texttt{transp}} = \tau_{\texttt{transp_st}} = \texttt{TRUE}$$

The only state that can last for more than one iteration is *idle*, and its terminal condition is
$$\scriptsize \tau_{\texttt{idle}} = \texttt{recvStatus}$$, so it is switched upon successful reception
of status data from real effectors. Then, the FSM is switched to *safe_st*:

$$\scriptsize \sigma_{\texttt{idle},\texttt{safe_st}} = \texttt{recvStatus}$$


Only one transition is possible from state *safe_st*, i.e. $$\scriptsize \sigma_{\texttt{safe_st},\texttt{safe}} = \texttt{TRUE} $$.


The transition conditions for state *safe* are:

$$\scriptsize \sigma_{\texttt{safe},\texttt{safe_st}} = \texttt{recvStatus} \wedge \neg \texttt{allHwOk}$$

$$\scriptsize \sigma_{\texttt{safe},\texttt{safe_st_ok}} = \texttt{recvStatus} \wedge \texttt{allHwOk}$$

$$\scriptsize \sigma_{\texttt{safe},\texttt{idle}} = \neg \texttt{recvStatus}$$


The transition conditions for state *safe_st_ok* are defined as:

$$\scriptsize \sigma_{\texttt{safe_st_ok},\texttt{safe}} = \neg \texttt{recvCommand} \vee \neg \texttt{allCmdOk} \vee \neg \texttt{cmdExitSafeState} \vee \neg \texttt{safeItPassed500} $$

$$\scriptsize \sigma_{\texttt{safe_st_ok},\texttt{transp}} = \texttt{recvCommand} \wedge \texttt{allCmdOk} \wedge \texttt{cmdExitSafeState} \wedge \texttt{safeItPassed500} $$


For state *transp* one transition is choosen using conditions:

$$\scriptsize \sigma_{\texttt{transp},\texttt{safe_st}} = \texttt{recvStatus} \wedge \neg \texttt{allHwOk}$$

$$\scriptsize \sigma_{\texttt{transp},\texttt{transp_st}} = \texttt{recvStatus} \wedge \texttt{allHwOk} $$

$$\scriptsize \sigma_{\texttt{transp},\texttt{idle}} = \neg \texttt{recvStatus}$$


The transtion conditions for state *transp* are:

$$\scriptsize \sigma_{\texttt{transp_st},\texttt{transp}} = \texttt{recvCommand} \wedge \texttt{allCmdOk}$$

$$\scriptsize \sigma_{\texttt{transp_st},\texttt{safe}} = \neg \texttt{recvCommand} \vee \neg \texttt{allCmdOk}$$


As error handling in this subsystem is not supported, error conditions for all states are defined as:

$$\scriptsize \epsilon_{\texttt{safe}} = \epsilon_{\texttt{safe_st}} = \epsilon_{\texttt{transp}} = \epsilon_{\texttt{transp_st}} = \epsilon_{\texttt{idle}} = \epsilon_{\texttt{safe_st_ok}} = \texttt{FALSE}$$

With predicates defined as:

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

