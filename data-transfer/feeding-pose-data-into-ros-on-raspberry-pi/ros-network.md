---
description: This section outlines the ROS network setup
---

# ROS Network

The next task is to relay the pose data to the RPi. This can be done using a ROS network; by following the steps in [communication](https://www.intorobotics.com/how-to-setup-ros-kinetic-to-communicate-between-raspberry-pi-3-and-a-remote-linux-pc/) which makes reference to the [ROS networking guide](http://wiki.ros.org/ROS/Tutorials/MultipleMachines). These steps are outlined for this system setup:

1. Ensure the master is running on the RPi
2. Configure ROS\_MASTER\_URI on the RPi so that the master on the RPi is used:&#x20;
   *   Set the RPi as the ROS master for this setup.&#x20;

       <pre><code><strong>sudo nano .bashrc
       </strong></code></pre>
   *   Navigate to the end of the <mark style="color:red;">.bashrc</mark> file and add the following:&#x20;

       ```bash
       export ROS_MASTER_URI=http://RPi_ip:11311
       ```
3. Configure ROS\_MASTER\_URI in the Ubuntu streaming the MoCap data so the master in the RPi is used:
   *   Set the RPi as the ROS master for this setup.&#x20;

       ```
       sudo nano .bashrc
       ```
   *   Navigate to the end of the file and add the following:&#x20;

       ```bash
       export ROS_MASTER_URI=http://RPi_ip:11311
       ```
4. This is already set in both machines, but as per the [reference](https://www.intorobotics.com/how-to-setup-ros-kinetic-to-communicate-between-raspberry-pi-3-and-a-remote-linux-pc/) the hostname must be defined in the <mark style="color:red;">.bashrc</mark> for both machines.

{% hint style="info" %}
The RPi\_ip was simply determined in section [Network Topology](network-topology-and-raspberry-pi-configuration.md).&#x20;
{% endhint %}

{% hint style="info" %}
When the roslaunch command is run to launch the vrpn ROS driver, if it is not connected to the RPi ROS master then the topic will only be connected to the Ubuntu ROS master and can't be accessed on the RPi.&#x20;
{% endhint %}

The RPi now has access to the Clover pose data streamed through Ubuntu on the topic <mark style="color:blue;">/vrpn\_client\_node/clover1/pose</mark>.&#x20;
