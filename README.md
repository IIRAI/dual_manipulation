DualManipulation
================

This repository is a container for all packages needed for the dualManipulation task. To download every required package at once use the following command:

`git clone --recursive https://github.com/CentroEPiaggio/dual_manipulation.git`

Refer to [this page](https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/) for setting up an ssh-key connected to your GitHub account.

Dependencies
------------
This package requires the repository `vito-robot` and its dependencies (`kuka-lwr` and `pisa-iit-soft-hand`): follow the instructions at the [vito-robot](https://github.com/CentroEPiaggio/vito-robot.git) page.

For a real scenario with the use of vision, you might need to perform camera-robot calibration. [This](https://github.com/CentroEPiaggio/calibration.git) calibration package can help you to do that..

Updating submodules
-------------------
When you clone this repo you will have all the submodules fixed at a certain version (that can be updated just committing on this repo when you update the submodule). First of all checkout the desired branch of `dual_manipulation`:

```
cd dual_manipulation
git checkout <branch-name>
git submodule update --remote
```

This will only update the branch registered in the .gitmodule, and by default, you will end up with a detached HEAD. Now you can `catkin_make` the packages to try out the planner.

If you wish to contribute to the project, befor editing the code of a `<branch>` in `<package-folder>`, you should run the following:

```
cd dual_manipulation/<package-folder>
git checkout <branch>
git pull
```

Documentation
-------------
To generate the documentation of the project you have to run the following series of commands:

```
cd dual_manipulation
doxygen documentation.doxygen
```

and then open the file `index.html` in the generated `html` folder with a browser.

Running a DualManipulation Demo
===============================
A full demo can be run with only kinematics (moveit and rviz only), simulation, or real robot. The main difference is the launch command needed to load either of those possibilities.

Let's explore useful parameters in more details

Configurations
--------------
Change in `dualmanipulation/ik_control/config/${ROBOT_NAME}_ik_control.yaml` file the parameter

`kinematics_only: true`

in order to avoid waiting for controller responses, i.e. when using kinematics only execution, and

`kinematics_only: false`

when using simulation or the real robot.

Change in `dualmanipulation/shared/config/${ROBOT_NAME}_dual_manipulation.yaml`

`use_vision: false`

to allow for positioning the source object via the GUI.

Kinematics only launch
----------------------
The planning procedure can be tested and, assuming controllers will do their job, the whole demo can be performed.

Load the robot model, moveit and rviz using the command

`roslaunch vito_moveit_configuration demo.launch`

Simulation launch
-----------------
Alternatively, the planning procedure can be tested also considering response from the controllers, assuming controllers will work similarly in the simulated and real robot; the whole demo can be performed, apart from not having the object in the simulated scene.

Load the simulated robot, moveit and rviz using the command

`roslaunch vito_description display.launch`

Real robot launch
-----------------
In this way, the physical robot can be used. 

- `roslaunch vito_description display.launch use_robot_sim:=false  load_moveit:=false  right_arm_enabled:=true  left_arm_enabled:=true right_hand_enabled:=true left_hand_enabled:=true use_rviz:=false`
- `roslaunch vito_moveit_configuration pacman_demo.launch` 


Common procedure
----------------
Once the robot_model has been loaded in one or the other way, and parameters have been set appropriately, *THREE* more shells are needed for `dualmanipulation`. The commands to run are

- `rosrun dual_manipulation_ik_control dual_manipulation_ik_control`
- `rosrun dual_manipulation_state_manager dual_manipulation_state_manager`
- `rosrun dual_manipulation_gui dual_manipulation_gui`

Then, after everything is steady (`ik_control` might take some time to load) you can start the experiment using the GUI, where the sequence of actions to be performed is

- select an object (if using the GUI for setting the target), publish markers and move them where source and target should be in the experiment (only target if the vision is used)
- `get info` button: information will be given about which objects have been found in the scene
- `set target` button: to be pressed after selecting the target position for the object; a graph will be shown in the GUI representing all possible states and transitions of the high level planner
- `plan` button: a tentative plan will be computed, and `state_manager` will check for kinematics feasibility; few iterations could be performed before finding a viable option from source to target; a red line will show the planned high level sequence of actions
- after a plan has been found, use `start moving` button to iterate between cartesian planning and command execution, which will all be automated.

If, for any reason, you want the robot to stop at a certain point, press the `stop` button and the trajectory will halt; you can also use the `home` function to move the whole robot back home.

`exit` closes the state machine, `reset` clears all states and allow for a fresh start. `abort plan` can be used if a high-level planned path is not what we wanted, to plan again (if no condition is changed, anyway, the planning result will be the same!), and `abort move` is used to abort planning-moving for the robot, without interrupting the current motion.
