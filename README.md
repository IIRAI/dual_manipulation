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

Clone recursively:

`git clone --recursive https://github.com/CentroEPiaggio/vito-robot.git`

And checkout development branch for all of then:

`cd vito-robot && git submodule foreach git checkout multi-robot-test`

- For simulation, you need [Gazebo4](http://gazebosim.org/tutorials?tut=install_ubuntu&ver=4.0&cat=install) or later to install `sudo apt-get install ros-indigo-gazebo4-ros` and all ['ros-controls' framework](https://github.com/ros-controls) (from `synaptic`/`apt-get` is ok as well).

- For the real scenario, you need to perform camera-robot calibration. We provide a [calibration](https://github.com/CentroEPiaggio/calibration.git) package that can help you to do that. However, other method that provides where the cameras (asus and two eyes of the KIT head) are w.r.t. the world is valid.


Then, to compile:

`cd ~/catkin_ws`

`catkin_make dual_manipulation_shared`

`cd build/dualmanipulation/shared`

`make install`

`cd ~/catkin_ws`

`catkin_make`