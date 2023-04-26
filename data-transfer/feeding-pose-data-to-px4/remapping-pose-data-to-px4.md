---
description: >-
  This page gives details on how to remap the streamed Clover pose data to the
  PX4 for localization and control.
---

# Remapping Pose Data to PX4

MAVROS provides a plugin to relay pose data published on <mark style="color:blue;">/mavros/vision\_pose/pose</mark> to PX4. Assuming that MAVROS is running, you just need to remap the pose topic that you get from the MoCap <mark style="color:blue;">/vrpn\_client\_node/clover1/pose</mark> directly to <mark style="color:blue;">/mavros/vision\_pose/pose</mark>. The Clover packages already have a MAVROS launch file setup to connect the raspberry Pi to the flight controller using MAVROS.

The method presented involves using an application called topic\_tools and is discussed further.

### Topic Tools

The method of choice gives you the ability to turn the remap function on and off after launch. This functionality is useful because it allows you to view the <mark style="color:blue;">/vrpn\_client\_node/clover1/pose</mark> topic and <mark style="color:blue;">/mavros/vision\_pose/pose</mark> topic separately in [RViz](https://clover.coex.tech/en/rviz.html#using-rviz-and-rqt) to ensure the yaw offset is minimal before remapping and fusing the external vision pose and onboard pose estimation. A ROS package called [topic\_tools](http://wiki.ros.org/topic\_tools) can be used to remap the topic where the package source can be found in its github [here](https://github.com/ros/ros\_comm). The steps to build from source for ROS Noetic are shown:&#x20;

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

{% hint style="info" %}
topic\_tools also provides another useful function named [throttle](http://wiki.ros.org/action/fullsearch/topic\_tools/throttle?action=fullsearch\&context=180\&value=linkto%3A%22topic\_tools%2Fthrottle%22). This allows you to adjust the bandwidth and message rate at whih the messages are being published or reemaped to a certain topic. I played around with this when I was debugging data fusion and timesync issues with MAVROS and the Clover.
{% endhint %}

### Additional considerations

Another consideration when remapping the vrpn topic to the mavros vision topic is listed below, which involves coding a remap command into the <mark style="color:orange;">sample.launch</mark> file.

From the description [remap](http://wiki.ros.org/roslaunch/XML/remap), the following command can be coded in vrpn launch file or <mark style="color:orange;">sample.launch</mark> file to remap the vrpn topic to the vision\_pose topic (assuming a rigid body name of clover1):&#x20;

```xml
<remap from="/vrpn_client_node/clover1/pose" to="/mavros/vision_pose/pose"/>
```

{% hint style="info" %}
Remapping applies to the lines following the remap. Nodes that are launched before any remap lines are not affected therefore insert the remap command before the <mark style="color:red;">vrpn\_client\_ros</mark> node in the <mark style="color:orange;">sample.launch</mark> file.
{% endhint %}
