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
