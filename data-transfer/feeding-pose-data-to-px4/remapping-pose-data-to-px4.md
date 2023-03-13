---
description: >-
  This page gives details on how to remap the streamed Clover pose data to the
  PX4 for localization and control.
---

# Remapping Pose Data to PX4

MAVROS provides a plugin to relay pose data published on <mark style="color:blue;">/mavros/vision\_pose/pose</mark> to PX4. Assuming that MAVROS is running, you just need to remap the pose topic that you get from the MoCap <mark style="color:blue;">/vrpn\_client\_node/clover1/pose</mark> directly to <mark style="color:blue;">/mavros/vision\_pose/pose</mark>. The Clover packages already have a MAVROS launch file setup to connect the RPi to the flight controller using MAVROS.

There are two general methods to remap the vrpn topic to the mavros vision topic, they are both listed but this setup uses the second one:

{% hint style="info" %}
For option 1, remapping applies to the lines following the remap. Nodes that are launched before any remap lines are not affected therefore insert the remap command before the <mark style="color:red;">vrpn\_client\_ros</mark> node in the <mark style="color:orange;">sample.launch</mark> file.
{% endhint %}

1.  From the description [remap](http://wiki.ros.org/roslaunch/XML/remap), the following command can be coded in vrpn launch file or <mark style="color:orange;">sample.launch</mark> file to remap the vrpn topic to the vision\_pose topic:&#x20;

    ```xml
    <remap from="/vrpn_client_node/clover1/pose" to="/mavros/vision_pose/pose"/>
    ```
2.  The method of choice allows you the the ability to turn the remap function on and off after launch. This functionality is useful because it allows you to view the <mark style="color:blue;">/vrpn\_client\_node/clover1/pose</mark> topic and <mark style="color:blue;">/mavros/vision\_pose/pose</mark> topic separately in RViz to ensure the yaw offset is minimal before remapping and fusing the external vision pose and onboard pose estimation. A ROS package called [topic\_tools](http://wiki.ros.org/topic\_tools) can be used to remap the topic where the package source can be found in its github [here](https://github.com/ros/ros\_comm). The steps to build from source for ROS Noetic are shown:&#x20;

    ```
    cd /path/to/your/catkin_ws/src
    git clone -b melodic-devel https://github.com/ros/ros_comm.git
    cd /path/to/your/catkin_ws
    rosdep update
    rosdep install --from-paths src/ --ignore-src --rosdistro noetic
    catkin_make
    ```

The following command can now be run on the Ubuntu Linux machine to remap the topic:

```
rosrun topic_tools relay /vrpn_client_node/clover1/pose /mavros/vision_pose/pose
```

{% hint style="info" %}
All the reference frame configurations earlier were done to make sure the pose data was published on the ENU frame as ROS uses ENU frame by convention. With the remapping of the published topic to vision\_pose topic, MAVROS will take care of the NED frame conversions and you don't have to manually do so.
{% endhint %}
