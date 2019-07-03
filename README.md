# rosinstall
Installation instructions and rosinstall configuration for the SMARC software packages

## Installation instructions

Follow the instructions here to install the SMARC ros packages.
The instructions assume that you are running Ubuntu 16.04, and have already
installed ROS Kinetic by following the instructions at http://wiki.ros.org/kinetic/Installation/Ubuntu .

### Install binary packages

Just execute the following command to get all the necessary binary packages for the SMARC system:
```
sudo apt-get install python-wstool ros-kinetic-gazebo-msgs ros-kinetic-gazebo-plugins ros-kinetic-gazebo-ros ros-kinetic-gazebo-ros-pkgs ros-kinetic-ros-control ros-kinetic-gazebo-ros-control ros-kinetic-joint-state-controller ros-kinetic-effort-controllers python-pygame protobuf-c-compiler protobuf-compiler ros-kinetic-joy ros-kinetic-joy-teleop ros-kinetic-robot-state-publisher ros-kinetic-costmap-2d ros-kinetic-tf2-geometry-msgs ros-kinetic-geodesy tmux python-scipy ros-kinetic-move-base-msgs ros-kinetic-pid ros-kinetic-mongodb-store ros-kinetic-rospy-message-converter ros-kinetic-smach ros-kinetic-robot-localization
```

### Setup a catkin workspace

Start by setting up workspace in a directory of your choosing.
Note that you can name it something else than `catkin_ws` if you wish:
```
mkdir catkin_ws
cd catkin_ws
wstool init src
catkin_init_workspace src
```

Now let's add the SMARC software to the workspace using `wstool`:
```
cd src
wstool merge https://raw.githubusercontent.com/smarc-project/rosinstall/master/smarc_system.rosinstall
wstool update
```

Now we should have fetched all the packages from github. If you inspect
the current directory (`src`) it should contain a bunch of folders.

### (Optional) Clone additional packages

If you have access to the `smarc_private_auvs` package, you can clone
that with git in this step. Use the command:
```
git clone https://gitr.sys.kth.se/smarc-project/smarc_private_auvs.git
```
If you have a copy of the package, make
sure to paste it in the current directory (i.e. `catkin_ws/src`).

### Building the packages

Now we should be able to build the packages:
```
cd ..
catkin_make
```
## Running a simple example

When running the files, we first need to source the catkin workspace.
If you are in the `catkin_ws` folder, do this by:
```
source devel/setup.bash
```
You might want to add `source /path/to/catkin_ws/devel/setup.bash` to your `~/.bashrc`.

If you have not started a roscore, do that in a separate terminal:
```
roscore
```

You should now be able to run a simple simulator example.
First, let's launch a gazebo world in a separate terminal using:
```
roslaunch smarc_bringup auv_scenarios.launch
```
Now, let's launch an AUV in a separate terminal using:
```
roslaunch smarc_bringup auv_model.launch
```
For more info on these launch files, see https://github.com/smarc-project/smarc_utils/tree/master/smarc_bringup

You should now have a gazebo window and an AUV floating somewhere in the world!

## Updating the software packages

You can use wstool to update the SMARC packages of your workspace
to the newest version by running `wstool update` inside the `catkin_ws/src` folder.

## Installing packages on an AUV

### SAM AUV

Start by installing ROS and some dependencies needed for building and running.
NOTE: This is an incomplete list of dependencies for building on the vehicle, please fill in!
```
sudo apt-get install ros-melodic-rosbridge-suite ros-melodic-tf2-web-republisher
```

Then follow the instructions [here](https://github.com/smarc-project/uavcan_ros_bridge#dependencies--building)
to install libuavcan.

You can now proceed do create a catkin workspace anywhere you like.
Go into the `src` folder of the workspace and execute the following commands.

```
git clone https://github.com/smarc-project/uavcan_ros_bridge.git
git clone https://gitr.sys.kth.se/smarc-project/sam_common.git
git clone https://gitr.sys.kth.se/smarc-project/sam_drivers.git
git clone https://gitr.sys.kth.se/smarc-project/stim300_ros_driver.git
git clone https://github.com/nilsbore/flexxros.git
```

You should now be able to execute `catkin_make` in your workspace to build everything.

Follow the instructions [here](https://gitr.sys.kth.se/smarc-project/stim300_ros_driver#setup) to set up the IMU.
