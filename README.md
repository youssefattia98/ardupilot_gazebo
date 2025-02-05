# Ardupilot Gazebo plugin 

## Requirements :
Native Ubuntu able to run full 3D graphics.
Gazebo 11.0


## Installation :
After cloning this pkg:

````
cd ardupilot_gazebo
mkdir build
cd build
sudo make install
````

````
cd 
nano .bashrc
source /usr/share/gazebo/setup.sh
export GAZEBO_PLUGIN_PATH=~{path to ardupilot_gazebo}/ardupilot_gazebo/build:${GAZEBO_PLUGIN_PATH}
````
