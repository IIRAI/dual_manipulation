DualManipulation software framework   {#mainpage}
==============================

[API index](http://dualManipulation.bitbucket.org)

This repository is a container for all packages needed for the dualManipulation task.
To download everything at once, simply use the command

`git clone --recursive git@bitbucket.org:dualmanipulation/dualmanipulation.git`

Refer to [this page](https://confluence.atlassian.com/pages/viewpage.action?pageId=270827678) in order to set-up an ssh-key connected to a bitbucket account.

Dependencies
------------------------------
This package also requires the packages for `vito-robot`: follow the specific instructions at the [vito-robot page](https://github.com/CentroEPiaggio/vito-robot.git).

For the real scenario, you need to perform camera-robot calibration. A [calibration](https://github.com/CentroEPiaggio/calibration.git) package that can help you to do that. However, other method that provides where the cameras (asus and two eyes of the KIT head) are w.r.t. the world is valid.

Updating submodules
------------------------------
When you clone this repo you will have all the submodules fixed at a certain version (that can be updated just committing on this repo when you update the submodule). To have the last version of each submodule you should run:

```git submodule foreach git checkout master
git submodule foreach git pull```

Documentation
------------------------------
To generate the documentation of the project you have to run in the project folder:

`doxygen documentation.doxygen`

and then open the file index.html in the generated html folder with a browser.

Multi-Object Demo
------------------------------
In order to run the multi-object demo which also uses mobile robots, you will need the additional packages that can be found in the [DualManipulation project page](https://bitbucket.org/account/user/dualmanipulation/projects/PROJ).

After downloading the `time_dependent_a_star` package, and any robot moveit configuration you might want to use, you need to change branch in two of the packages, which have changes not supported from the master branch

```
roscd dual_manipulation_state_manager
git checkout multi_object_new
roscd dual_manipulation_planner
git checkout remote_planner_new
```

Running a full DualManipulation demo
==============================
At this stage, having everything mentioned above installed, a full demo can be run with only kinematics (moveit and rviz only), simulation, or real robot. The main difference is the launch command needed to load either of those possibilities.

Let's explore useful parameters in more details

Configurations
------------------------------
Change in `dualmanipulation/ik_control/config/${ROBOT_NAME}_ik_control.yaml` file the parameter

`kinematics_only: true`

in order to avoid waiting for controller responses, i.e. when using kinematics only execution, and

`kinematics_only: false`

when using simulation or the real robot.

Change in `dualmanipulation/shared/config/${ROBOT_NAME}_dual_manipulation.yaml`

`use_vision: false`

to allow for positioning the source object via the GUI.

Kinematics only launch
------------------------------
In this way, the planning procedure can be tested and, assuming controllers will do their job, the whole demo can be performed.

Load the robot model, moveit and rviz using the command

`roslaunch vito_moveit_configuration demo.launch`

Simulation launch
------------------------------
In this way, the planning procedure can be tested also considering response from the controllers, assuming controllers will work similarly in the simulated and real robot; the whole demo can be performed, apart from not having the object in the simulated scene.

Load the simulated robot, moveit and rviz using the command

`roslaunch vito_description display.launch`

Real robot launch
------------------------------
In this way, the physical robot can be used. 

- `roslaunch vito_description display.launch use_robot_sim:=false  load_moveit:=false  right_arm_enabled:=true  left_arm_enabled:=true right_hand_enabled:=true left_hand_enabled:=true use_rviz:=false`
- `roslaunch vito_moveit_configuration pacman_demo.launch` 


Common procedure
------------------------------
Once the robot_model has been loaded in one or the other way, and parameters have been set appropriately, *THREE* more shells are needed for `dualmanipulation`. The commands to run are

- `rosrun dual_manipulation_ik_control dual_manipulation_ik_control`
- `rosrun dual_manipulation_state_manager dual_manipulation_state_manager`
- `rosrun dual_manipulation_gui dual_manipulation_gui`

Then, after everything is steady (`ik_control` will take some time to load, still not much) you can start the experiment. All control is done via the GUI, where the sequence of actions to be performed is

- select an object (if using the GUI for setting the target), publish markers and move them where source and target should be in the experiment (only target if the vision is used)
- `get info` button: information will be given about which objects have been found in the scene
- `set target` button: to be pressed after selecting the target position for the object; a graph will be shown in the GUI representing all possible states and transitions of the high level planner
- `plan` button: a tentative plan will be computed, and `state_manager` will check for kinematics feasibility; few iterations could be performed before finding a viable option from source to target; a red line will show the planned high level sequence of actions
- after a plan has been found, use `start moving` button to iterate between cartesian planning and command execution, which will all be automated.

If, for any reason, you want the robot to stop at a certain point, press the `stop` button and the trajectory will halt; you can also use the `home` function to move the whole robot back home.

`exit` closes the state machine, `reset` clears all states and allow for a fresh start. `abort plan` can be used if a high-level planned path is not what we wanted, to plan again (if no condition is changed, anyway, the planning result will be the same!), and `abort move` is used to abort planning-moving for the robot, without interrupting the current motion.
