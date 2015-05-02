DualManipulation software framework   {#mainpage}
==============================

[API index] (dualManipulation.bitbucket.org)

This repository is a container for all (or most of the) packages needed for the dualManipulation task.
To download everything at once, simply use the command

`git clone --recursive https://<your-user-name>@bitbucket.org/dualmanipulation/dualManipulation.git`

or, if you already set-up an ssh-key for you account

`git clone --recursive git@bitbucket.org:dualmanipulation/dualmanipulation.git`

Refer to [this page](https://confluence.atlassian.com/pages/viewpage.action?pageId=270827678) in order to set-up an ssh-key connected to a bitbucket account.

When you clone this repo you will have all the submodules fixed at a certain version (that can be updated just committing on this repo when you update the submodule). To have the last version of each submodule you should run:

`git submodule foreach git checkout master`

to switch from the fixed version and then:

`git submodule foreach git pull`


To generate the documentation of the project you have to run in the project folder:

`doxygen documentation.doxygen`

and then open the file index.html in the generated html folder with a browser.

To use all the simulation and control suite you have to download these packages:

Clone recursively:

`git clone --recursive https://github.com/CentroEPiaggio/vito-robot.git`

And checkout development branch for all of then:

`cd vito-robot && git submodule foreach git checkout multi-robot-test`

- For simulation, you need [Gazebo4](http://gazebosim.org/tutorials?tut=install_ubuntu&ver=4.0&cat=install) or later to install `sudo apt-get install ros-indigo-gazebo4-ros` and all ['ros-controls' framework](https://github.com/ros-controls) (from `synaptic`/`apt-get` is ok as well).

- For the real scenario, you need to perform camera-robot calibration. We provide a [calibration](https://github.com/CentroEPiaggio/calibration.git) package that can help you to do that. However, other method that provides where the cameras (asus and two eyes of the KIT head) are w.r.t. the world is valid.


Then, to compile:

`roscd && cd ..`

`catkin_make`



Running a full DualManipulation demo
==============================
At this stage, having everything mentioned above installed, a full demo can be run with only kinematics (moveit and rviz only), simulation, or real robot. The main difference is the launch command needed to load either of those possibilities.

Let's explore useful parameters in more details

Configurations
------------------------------
Change in `dualmanipulation/ik_control/config/ik_control.yaml` file the parameter

`kinematics_only: true`

in order to avoid waiting for controller responses, i.e. when using kinematics only execution, and

`kinematics_only: false`

when using simulation or the real robot.

Change in `dualmanipulation/gui/config/gui.yaml`

`setting_source_position: true`

to allow for positioning the source object via the GUI (kinematics only, and simulation), or

`setting_source_position: false`

when wanting to use the vision module for object identification and pose estimation.

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


Common procedure
------------------------------
Once the robot_model has been loaded in one or the other way, and parameters have been set appropriately, *FOUR* more shells are needed for `dualmanipulation`. The commands to run are

- `roslaunch dual_manipulation_ik_control ik_control.launch`
- `rosrun dual_manipulation_planner dual_manipulation_planner`
- `rosrun dual_manipulation_state_manager dual_manipulation_state_manager`
- `roslaunch dual_manipulation_gui gui.launch`

Then, after everything is steady (`ik_control` will take some time to load, still not much) you can start the experiment. All control is done via the GUI, where the sequence of actions to be performed is

- select an object (if using the GUI for setting the target), publish markers and move them where source and target should be in the experiment (only target if the vision is used)
- `set target` button
- `get info` button: the `state_manager` shell will say which object has been found in the scene, and which are the table grasps more closely related to its source and target poses; moreover, a graph will be shown in the GUI representing all possible states and transitions of the high level planner
- `plan` button: the `planner` shell will give a tentative plan, and `state_manager` will check for kinematics feasibility; few iterations could be performed before finding a viable option from source to target; a red line will show the planned high level sequence of actions
- after a plan has been found, use `start moving` button to iterate between cartesian planning and command execution, which will all be automated.

If, for any reason, you want the robot to stop at a certain point, press the `stop` button and the trajectory will halt; you can also use the `home` function to move the whole robot back home.

`exit` closes the state machine, `reset` needs improvements, but it's supposed to clear all states and allow for a fresh start. `abort plan` can be used if a high-level planned path is not what we wanted, to plan again (if no condition is changed, anyway, the planning result will be the same!), and `abort move` is used to abort planning-moving for the robot, without interrupting the current motion.