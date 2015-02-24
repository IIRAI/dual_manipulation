DualManipulation software framework
==============================

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

- head: https://github.com/CentroEPiaggio/kit-head
- arms: https://github.com/CentroEPiaggio/kuka-lwr
- hands: https://github.com/CentroEPiaggio/pisa-iit-soft-hand
- simulations: https://github.com/CentroEPiaggio/vito_robot, https://github.com/CentroEPiaggio/ros_control, https://github.com/CentroEPiaggio/gazebo_ros_pkgs

You also need to use Gazebo4 for the simulations, and install accordingly ros-{rosdistro}-gazebo4-ros-control (and related) packages, plus any other dependency that will come up for the forked gazebo_ros_pkgs package (e.g. pcl_conversions if you did not have it before).

For the head, arms, hands, ros_control, and gazebo_ros_pkgs, you need to checkout the multi-robot-test branch

`cd {each package folder}`

`git checkout multi-robot-test`


Then, to compile:

`cd ~/catkin_ws`

`catkin_make dual_manipulation_shared`

`cd build/dualmanipulation/shared`

`make install`

`cd ~/catkin_ws`

`catkin_make`