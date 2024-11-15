# Ardupilot Gazebo plugin 

## Requirements :
Native Ubuntu able to run full 3D graphics.
(Virtual Machine such as VMWare Player does not support full 3D graphics.0)
Gazebo version 7.x or 8.x.
Gazebo >= 9.x not supported.

## Disclamer : 
This is a playground until I get some time to push the correct patch to gazebo master (I got hard time to work with mercurial..)!  
So you can expect things to not be up-to-date.  
This assume that your are using Ubuntu 16.04

## Usage :
I assume you already have Gazebo installed with ROS (or without).  
If you don't have it yet, install ROS with sudo apt install ros-kinetic-desktop-full 
(follow instruction here http://wiki.ros.org/kinetic/Installation/Ubuntu).  
Due to a bug in current gazebo release from ROS, please update gazebo with OSRF version from http://gazebosim.org/tutorials?tut=install_ubuntu
libgazebo7-dev or libgazebo8-dev must be installed.

Dependencies: 
```
sudo apt install git cmake build-essential
```

For Gazebo 7
````
sudo apt install gazebo7 libgazebo7-dev
````
OR  
For Gazebo 8
````
sudo apt install gazebo8 libgazebo8-dev
````
````
git clone https://github.com/patrickelectric/ardupilot_gazebo
cd ardupilot_gazebo
git checkout add_link
mkdir build
cd build
cmake ..
make -j4
sudo make install
````

````
echo 'source /usr/share/gazebo/setup.sh' >> ~/.bashrc
````

Set Path of Gazebo Models (Adapt the path to where to clone the repo)
````
echo 'export GAZEBO_MODEL_PATH=~/ardupilot_gazebo/models' >> ~/.bashrc
````

Set Path of Gazebo Worlds (Adapt the path to where to clone the repo)
````
echo 'export GAZEBO_RESOURCE_PATH=~/ardupilot_gazebo/worlds:${GAZEBO_RESOURCE_PATH}' >> ~/.bashrc
````

````
source ~/.bashrc
````

DONE !

Now launch a world file with a copter/rover/plane and ardupilot plugin, and it should work! 
(I will try to add some world file and model later)

## HELP

How to Launch :  
Launch Ardupilot Software In the Loop Simulation for each vehicle.
On new terminal, Launch Gazebo with basic demo world.
ROVER

On 1st Terminal(Launch Ardupilot SITL)
````
sim_vehicle.py -v APMrover2 -f gazebo-rover -m --mav10 --map --console -I0
````

On 2nd Termianal(Launch Gazebo with demo Rover model)
````
gazebo --verbose worlds/ (Please Add if there is one.)
````

##### COPTER

On 1st Terminal(Launch Ardupilot SITL)
````
sim_vehicle.py -v ArduCopter -f gazebo-iris -m --mav10 --map --console -I0
````

On 2nd Terminal(Launch Gazebo with demo 3DR Iris model)
````
gazebo --verbose worlds/iris_arduco_demo.world
````

##### PLANE

On 1st Terminal(Launch Ardupilot SITL)
````
sim_vehicle.py -v ArduPlane -f gazebo-zephyr -m --mav10 --map --console -I0
````

On 2nd Terminal(Launch Gazebo with demo Zephyr flying wing model)
````
gazebo --verbose worlds/zephyr_ardupilot_demo.world
````

In addition, you can use any GCS of Ardupilot locally or remotely (will require connection setup).
If MAVProxy Developer GCS is uncomportable. Omit --map --console arguments out of SITL launch. and Use APMPlanner 2 or QGroundControl instead.
Local connection with APMPlanner2/QGroundControl is automatic, and recommended.

For APMPlanner2

Download it here http://firmware.eu.ardupilot.org/Tools/APMPlanner/
and launch it in terminal

````
apmplanner2
````

For QGroundControl

Download it here and follow the installation guide.

https://donlakeflyer.gitbooks.io/qgroundcontrol-user-guide/en/download_and_install.html


## Troubleshooting

### Missing libArduPilotPlugin.so... etc 

In case you see this message when you launch gazebo with demo worlds, check you have no error after sudo make install.  
If no error use "ls" on the install path given to see if the plugin is really here.  
If this is correct, check with "cat /usr/share/gazebo/setup.sh" the variable GAZEBO_PLUGIN_PATH. It should be the same as the install path. If not use "cp" to copy the lib to right path. 

For Example

````
sudo cp -a /usr/lib/x86_64-linux-gnu/gazebo-7.0/plugins/ /usr/lib/x86_64-linux-gnu/gazebo-7/
````

path mismatch is confirmed as ROS's glitch. It'll be fixed.

### Future(not activated yet)
To use Gazebo gps, you must offset the heading of +90° as gazebo gps is NWU and ardupilot is NED 
(I don't use GPS altitude for now)  
example : for SITL default location
````
    <spherical_coordinates>
      <surface_model>EARTH_WGS84</surface_model>
      <latitude_deg>-35.363261</latitude_deg>
      <longitude_deg>149.165230</longitude_deg>
      <elevation>584</elevation>
      <heading_deg>87</heading_deg>
    </spherical_coordinates>
````

