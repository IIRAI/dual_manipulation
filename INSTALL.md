# DualManipulation Building Tips

## Common catkin_make Error Solutions

### ROS Packages Missing

**Error Type**
```
Could not find a package configuration file provided by "XXXXX" with any of
  the following names:

    XXXXXConfig.cmake
    xxxxx-config.cmake
```

**Suggested Solution**
Try `sudo apt install ros-<ros-distro>-xxxxx` and do follow some tutorials on ROS and Cmake because this is basics.


### Pacman Vision Communications
**Error Type**

```
Could not find a package configuration file provided by "pacman_vision_comm"

```

**Suggested Solution**
Clone and make the following package: [pacman_vision_communications](https://github.com/CentroEPiaggio/pacman_vision_communications)

### Qt4 Not Found

**Error Type**
```
CMake Error at /usr/share/cmake-3.10/Modules/FindQt4.cmake:1320 (message):
  Found unsuitable Qt version "" from NOTFOUND, this code requires Qt 4.x
```

**Suggested Solution**
Try `sudo apt install qt4-default`.
If the previous does not work, try the solutions in the following page: [Stack Overflow Solution](https://stackoverflow.com/questions/22113575/cmake-doesnt-know-where-is-qt4-qmake)


