---
description: This page describes the steps required in the motive software.
---

# Motive Software Configuration

### Creating a Rigid Body

The steps needed to create rigid body are:

* Align the Clovers forward direction with the [system +x-axis](https://v20.wiki.optitrack.com/index.php?title=Template:Coordinate\_System).
* Define a rigid body in the Motive software:&#x20;

{% embed url="https://www.youtube.com/watch?ab_channel=AcuityInc&v=1e6Qqxqe-k0" %}
OptiTrack Motive: Intro to Rigid Bodies
{% endembed %}

* This is simply done by highlighting all of the markers in motive then right clicking and selecting created rigid body ...?? Give the Clover a name that does not contain spaces, e.g. clover1 instead of clover 1. Rigidbody1 works fine too.
* [Enable Frame Broadcast and VRPN streaming](https://www.youtube.com/watch?v=yYRNG58zPFo\&ab\_channel=IKINEMA): inser pic
* Note down the IP address given to the OptiTrack PC as it will be used when feeding pose data to the Clover.
* In the Advanced Network Options in the Data Streaming pane set the Up axis to be the Z-axis (the default is Y). Insert pic:

{% hint style="warning" %}
It is very important that you align the forward direction of the Clover with the x-axis of your motion capture volume before creating the rigid body in motive. This x-axis was previously defined when setting the ground plane during calibration (it can simply be viewed in the motive software). If this is not done, the external vision data may not fuse with the onboard pose estimate i.e. fusion errors. If it does fuse, the yaw offset between the drones body frame and motion capture system reference frame will result in the Clover being unable to follow set-points i.e. uncontrollable.
{% endhint %}
