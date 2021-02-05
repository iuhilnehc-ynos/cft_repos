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

NOTE: we expect the listener to recevie "Hello world: 5" and "Hello world: 10" by [custom filter](https://github.com/iuhilnehc-ynos/ros2-demos/blob/65db6c4105f424c42a18fe535c14c8b5ca5590d8/demo_nodes_cpp/src/topics/listener.cpp#L46-L48), but using expression_filter seems not working.

```
chenlh@OptiPlex-7080:~/Projects/ROS2/cft_ws$ RMW_IMPLEMENTATION=rmw_connextdds ros2 launch demo_nodes_cpp talker_listener.launch.py
[INFO] [launch]: All log files can be found below /home/chenlh/.ros/log/2021-02-05-16-22-10-035403-OptiPlex-7080-78758
[INFO] [launch]: Default logging verbosity is set to INFO
[INFO] [talker-1]: process started with pid [78762]
[INFO] [listener-2]: process started with pid [78764]
[listener-2] RTI Data Distribution Service Non-commercial license is for academic, research, evaluation and personal use only. USE FOR COMMERCIAL PURPOSES IS PROHIBITED. See RTI_LICENSE.TXT for terms. Download free tools at rti.com/ncl. License issued to Non-Commercial User license@rti.com For non-production use only.
[listener-2] Expires on 00-jan-00 See www.rti.com for more information.
[talker-1] RTI Data Distribution Service Non-commercial license is for academic, research, evaluation and personal use only. USE FOR COMMERCIAL PURPOSES IS PROHIBITED. See RTI_LICENSE.TXT for terms. Download free tools at rti.com/ncl. License issued to Non-Commercial User license@rti.com For non-production use only.
[talker-1] Expires on 00-jan-00 See www.rti.com for more information.
[talker-1] [INFO] [1612513331.103610014] [talker]: Publishing: 'Hello World: 1'
[talker-1] [INFO] [1612513332.103369326] [talker]: Publishing: 'Hello World: 2'
[talker-1] [INFO] [1612513333.103458269] [talker]: Publishing: 'Hello World: 3'
[talker-1] [INFO] [1612513334.103372624] [talker]: Publishing: 'Hello World: 4'
[talker-1] [INFO] [1612513335.103371600] [talker]: Publishing: 'Hello World: 5'
[listener-2] [INFO] [1612513335.104995153] [listener]: I heard: [Hello World: 5]
[talker-1] [INFO] [1612513336.103363083] [talker]: Publishing: 'Hello World: 6'
[talker-1] [INFO] [1612513337.103449618] [talker]: Publishing: 'Hello World: 7'
[talker-1] [INFO] [1612513338.103433908] [talker]: Publishing: 'Hello World: 8'
[talker-1] [INFO] [1612513339.103410382] [talker]: Publishing: 'Hello World: 9'
[talker-1] [INFO] [1612513340.103445562] [talker]: Publishing: 'Hello World: 10'
[talker-1] [INFO] [1612513341.103375569] [talker]: Publishing: 'Hello World: 11'
[talker-1] [INFO] [1612513342.103414672] [talker]: Publishing: 'Hello World: 12'
[talker-1] [INFO] [1612513343.103420058] [talker]: Publishing: 'Hello World: 13'
^C[WARNING] [launch]: user interrupted with ctrl-c (SIGINT)
[listener-2] [INFO] [1612513343.109583942] [rclcpp]: signal_handler(signal_value=2)
[talker-1] [INFO] [1612513343.109584217] [rclcpp]: signal_handler(signal_value=2)
[INFO] [listener-2]: process has finished cleanly [pid 78764]
[INFO] [talker-1]: process has finished cleanly [pid 78762]
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
