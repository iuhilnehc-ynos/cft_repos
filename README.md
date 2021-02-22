# Overview

to reproduce same issue in development phase.

# Steps

## Download source code

```
$ mkdir -p cft_ws/src
$ cd cft_ws
$ git clone https://github.com/iuhilnehc-ynos/cft_repos.git
$ vcs import src < cft_repos/cft.repos
```

NOTE: to rebase from master for these repositories in the future.

## Build

```
$ source ${YOUR_ROS2_MASTER}  # rcl_logging_interfaces is not provided in foxy
$ source /opt/rti.com/rti_connext_dds-5.3.1/resource/scripts/rtisetenv_x64Linux3gcc5.4.0.bash
$ colcon build --symlink-install --merge-install --cmake-args -DBUILD_TESTING:BOOL=OFF -DCMAKE_BUILD_TYPE=Debug
```

## Run

```
$ source install/setup.bash
$ RMW_IMPLEMENTATION=rmw_connextdds ros2 launch demo_nodes_cpp talker_listener.launch.py
```

NOTE: expect that listener can recevie "Hello world: 5" and "Hello world: 10" by [custom filter](https://github.com/iuhilnehc-ynos/ros2-demos/blob/6339664b34e52e67f0e87fb771fe938d46fadb4f/demo_nodes_cpp/src/topics/listener.cpp#L69-L71) from talker.

```
chenlh@OptiPlex-7080:~/Projects/ROS2/cft_ws$ RMW_IMPLEMENTATION=rmw_connextdds ros2 launch demo_nodes_cpp talker_listener.launch.py
[INFO] [launch]: All log files can be found below /home/chenlh/.ros/log/2021-02-22-15-56-17-059652-Kingston-76609
[INFO] [launch]: Default logging verbosity is set to INFO
[INFO] [talker-1]: process started with pid [76611]
[INFO] [listener-2]: process started with pid [76613]
[listener-2] RTI Data Distribution Service Non-commercial license is for academic, research, evaluation and personal use only. USE FOR COMMERCIAL PURPOSES IS PROHIBITED. See RTI_LICENSE.TXT for terms. Download free tools at rti.com/ncl. License issued to Non-Commercial User license@rti.com For non-production use only.
[listener-2] Expires on 00-jan-00 See www.rti.com for more information.
[talker-1] RTI Data Distribution Service Non-commercial license is for academic, research, evaluation and personal use only. USE FOR COMMERCIAL PURPOSES IS PROHIBITED. See RTI_LICENSE.TXT for terms. Download free tools at rti.com/ncl. License issued to Non-Commercial User license@rti.com For non-production use only.
[talker-1] Expires on 00-jan-00 See www.rti.com for more information.
[talker-1] [INFO] [1613980578.123260368] [talker]: Publishing: 'Hello World: 1'
[talker-1] [INFO] [1613980579.123387044] [talker]: Publishing: 'Hello World: 2'
[talker-1] [INFO] [1613980580.123283493] [talker]: Publishing: 'Hello World: 3'
[talker-1] [INFO] [1613980581.123249055] [talker]: Publishing: 'Hello World: 4'
[talker-1] [INFO] [1613980582.123446897] [talker]: Publishing: 'Hello World: 5'
[listener-2] [INFO] [1613980582.125212730] [listener]: I heard: [Hello World: 5]
[talker-1] [INFO] [1613980583.123170318] [talker]: Publishing: 'Hello World: 6'
[talker-1] [INFO] [1613980584.123255076] [talker]: Publishing: 'Hello World: 7'
[talker-1] [INFO] [1613980585.123093930] [talker]: Publishing: 'Hello World: 8'
[talker-1] [INFO] [1613980586.123367558] [talker]: Publishing: 'Hello World: 9'
[talker-1] [INFO] [1613980587.123284734] [talker]: Publishing: 'Hello World: 10'
[listener-2] [INFO] [1613980587.123719683] [listener]: I heard: [Hello World: 10]
[talker-1] [INFO] [1613980588.123272767] [talker]: Publishing: 'Hello World: 11'
[talker-1] [INFO] [1613980589.123373365] [talker]: Publishing: 'Hello World: 12'
[talker-1] [INFO] [1613980590.123393797] [talker]: Publishing: 'Hello World: 13'
^C[WARNING] [launch]: user interrupted with ctrl-c (SIGINT)
[listener-2] [INFO] [1613980590.795635417] [rclcpp]: signal_handler(signal_value=2)
[talker-1] [INFO] [1613980590.795633839] [rclcpp]: signal_handler(signal_value=2)
[INFO] [talker-1]: process has finished cleanly [pid 76611]
[INFO] [listener-2]: process has finished cleanly [pid 76613]
```

# Extra Info

- rmw_fastrtps_cpp

```
$ RMW_IMPLEMENTATION=rmw_fastrtps_cpp ros2 launch demo_nodes_cpp talker_listener.launch.py
[INFO] [launch]: All log files can be found below /home/chenlh/.ros/log/2021-02-05-16-56-05-125709-OptiPlex-7080-160998
[INFO] [launch]: Default logging verbosity is set to INFO
[INFO] [talker-1]: process started with pid [161002]
[INFO] [listener-2]: process started with pid [161004]
[talker-1] [INFO] [1612515366.194569334] [talker]: Publishing: 'Hello World: 1'
[listener-2] [INFO] [1612515366.196899645] [listener]: I heard: [Hello World: 1]
[talker-1] [INFO] [1612515367.194299911] [talker]: Publishing: 'Hello World: 2'
[listener-2] [INFO] [1612515367.196026748] [listener]: I heard: [Hello World: 2]
[talker-1] [INFO] [1612515368.194507833] [talker]: Publishing: 'Hello World: 3'
[listener-2] [INFO] [1612515368.196146419] [listener]: I heard: [Hello World: 3]
[talker-1] [INFO] [1612515369.194508059] [talker]: Publishing: 'Hello World: 4'
[listener-2] [INFO] [1612515369.196129661] [listener]: I heard: [Hello World: 4]
[talker-1] [INFO] [1612515370.194345531] [talker]: Publishing: 'Hello World: 5'
[listener-2] [INFO] [1612515370.196010683] [listener]: I heard: [Hello World: 5]
[talker-1] [INFO] [1612515371.194341950] [talker]: Publishing: 'Hello World: 6'
[listener-2] [INFO] [1612515371.195995876] [listener]: I heard: [Hello World: 6]
^C[WARNING] [launch]: user interrupted with ctrl-c (SIGINT)
[listener-2] [INFO] [1612515371.584547706] [rclcpp]: signal_handler(signal_value=2)
[talker-1] [INFO] [1612515371.584563702] [rclcpp]: signal_handler(signal_value=2)
[INFO] [listener-2]: process has finished cleanly [pid 161004]
[INFO] [talker-1]: process has finished cleanly [pid 161002]
```

- rmw_cyclonedds_cpp

```
$ RMW_IMPLEMENTATION=rmw_cyclonedds_cpp ros2 launch demo_nodes_cpp talker_listener.launch.py
[INFO] [launch]: All log files can be found below /home/chenlh/.ros/log/2021-02-05-16-56-45-638806-OptiPlex-7080-161062
[INFO] [launch]: Default logging verbosity is set to INFO
[INFO] [talker-1]: process started with pid [161066]
[INFO] [listener-2]: process started with pid [161068]
[listener-2] 1612515405.760062 [28]   listener: using network interface enp5s0 (udp/192.168.0.50) selected arbitrarily from: enp5s0, docker0
[talker-1] 1612515405.760063 [28]     talker: using network interface enp5s0 (udp/192.168.0.50) selected arbitrarily from: enp5s0, docker0
[talker-1] [INFO] [1612515406.768323857] [talker]: Publishing: 'Hello World: 1'
[listener-2] [INFO] [1612515406.769672223] [listener]: I heard: [Hello World: 1]
[talker-1] [INFO] [1612515407.768097312] [talker]: Publishing: 'Hello World: 2'
[listener-2] [INFO] [1612515407.768934786] [listener]: I heard: [Hello World: 2]
[talker-1] [INFO] [1612515408.767960296] [talker]: Publishing: 'Hello World: 3'
[listener-2] [INFO] [1612515408.768301469] [listener]: I heard: [Hello World: 3]
[talker-1] [INFO] [1612515409.768175761] [talker]: Publishing: 'Hello World: 4'
[listener-2] [INFO] [1612515409.769012558] [listener]: I heard: [Hello World: 4]
[talker-1] [INFO] [1612515410.768182076] [talker]: Publishing: 'Hello World: 5'
[listener-2] [INFO] [1612515410.769017712] [listener]: I heard: [Hello World: 5]
[talker-1] [INFO] [1612515411.768100566] [talker]: Publishing: 'Hello World: 6'
[listener-2] [INFO] [1612515411.768958365] [listener]: I heard: [Hello World: 6]
^C[WARNING] [launch]: user interrupted with ctrl-c (SIGINT)
[talker-1] [INFO] [1612515412.608803845] [rclcpp]: signal_handler(signal_value=2)
[listener-2] [INFO] [1612515412.608800410] [rclcpp]: signal_handler(signal_value=2)
[INFO] [listener-2]: process has finished cleanly [pid 161068]
[INFO] [talker-1]: process has finished cleanly [pid 161066]
```
