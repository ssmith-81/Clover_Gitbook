---
description: This section outlines the steps in sending the pose data to the PX4.
---

# Feeding Pose Data to PX4

With the pose data being streamed on an individual topic within ROS on the raspberry Pi, the flight controller must be set so it can accept the the external pose data. PX4 has two state estimators, **EKF2** (default estimator) an extended Kalman filter, and **LPE** which is a local position estimator.

At this moment the Clover is configured to stream pose data on the mavros vision topic (<mark style="color:blue;">/mavros/vision\_pose/pose</mark>) being the same one used for the ArUco marker system. The mocap topic (<mark style="color:blue;">/mavros/mocap/pose</mark>) is not active in the [mavros.launch](https://github.com/CopterExpress/clover/blob/master/clover/launch/mavros.launch) from Clover. This is fine as both estimators can use the vision pipeline where EKF2 strictly uses the vision one and LPE can use both mocap or vision pipelines. More details can be found [here](https://docs.px4.io/main/en/ros/external\_position\_estimation.html#px4-mavlink-integration).

{% hint style="info" %}
Some MAVROS plugins are disabled by default in the `clover` package in order to save resources. For more information, see the `plugin_blacklist` parameter in `/home/pi/catkin_ws/src/clover/clover/launch/mavros.launch`.
{% endhint %}

It is recommended in the [PX4 website](https://docs.px4.io/main/en/ros/external\_position\_estimation.html#px4-mavlink-integration) to use the EKF2 estimator with the PX4 instead of the LPE as it is better tested and supported therefore that is the estimator that will be used.

{% hint style="warning" %}
It is very important to note that the PX4 firmware version used for this setup is the general PX4 v1.13.0 or higher. This 'new' version of PX4 firmware contains the COEX Pix patches upstream and therefore works for the Clover without parameter and setup modifications. The older Clover specific PX4 firmware v1.8.2-clover13 uses LPE because the EKF2 is very buggy. LPE was tested to work in both PX4 versions and will be briefly presented in the one of the following sections if for some reason you prefer using that estimator. Although, this article mainly focuses on using the stable v1.13.0 firmware along with the EKF2 estimator.
{% endhint %}

A generalized parameter setup for both firmware versions can be found on the [Clover website](https://clover.coex.tech/en/parameters.html), where LPE parameters are set for the older firmware and the EKF2 parameters are set for the new PX4 firmware.&#x20;
