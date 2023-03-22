---
description: Tracking Bernoulli's Lemniscate (Figure 8) using MAVROS
---

# Complex Trajectory Tracking

<figure><img src="../../../.gitbook/assets/Lemniscate_of_Bernoulli (1).gif" alt=""><figcaption><p>Graphical representation of Bernoulli's Lamniscate</p></figcaption></figure>

Smooth analog trajectory can be discretized at a high enough rate to make PX4 follow smoothly. The first and second derivatives of position must be evaluated and sent to the PX4 for precise flight. The parametric equations of this trajectory can be found [here](https://en.wikipedia.org/wiki/Lemniscate\_of\_Bernoulli), and illustrated below:

$$
x = \frac{r \cdot cos(a)}{1+sin^2(a)}; \ \ \ y = \frac{r \cdot sin(a) \cdot cos(a)}{1+sin^2(a)}
$$

For our application we need to calculate the velocity and acceleration of this smooth trajectory for feedforward tracking with the PX4 (this improves performance). This is determined with the following:

$$
\frac{dx}{dt} = \frac{dx}{da}\cdot \frac{\Delta a}{\Delta t}; \ \ \ \frac{dy}{dt} = \frac{dy}{da}\cdot \frac{\Delta a}{\Delta t}
$$

Where a constant angular rate of change was set, the acceleration is also given:



$$
\frac{d^2x}{dt^2} = \frac{d^2x}{da^2}\cdot \left(\frac{\Delta a}{\Delta t}\right)^2; \ \ \ \frac{d^2y}{dt^2} = \frac{d^2y}{da^2}\cdot \left(\frac{\Delta a}{\Delta t}\right)^2
$$

{% hint style="info" %}
You can send only position setpoints and tracking will take place although it will happen poorly because of control limitations. The Clover will consistently lag behind the position setpoints provided because it is unable to react quick enough for this trajectory. This can be noticed using Clovers navigate function as well, where it may only publish position setpoints, although for that general purpose it is fine. Modifications to this have been made to include more extensive feedforward application although because it uses linear waypoint tracking, there are discontinuities in the velocity when it reaches its destination (the trajectory is not smooth). Therefore the Clover will overshoot the waypoint some depending on its current speed.
{% endhint %}

This method is adapted from the "Advanced Mapping and Planning in Dynamic Environments with PX4 offboard mode" provided by PX4. The detailed video can be seen:

{% embed url="https://www.youtube.com/watch?v=tV8jm8UKyPE&ab_channel=PX4Autopilot-OpenSourceFlightControl." %}

The above example uses MAVSDK for offboard control, where the method with the Clover uses MAVROS with the Clover environment. MAVROS is a ROS node that uses a set of plugins to communicate with the flight controller via the MAVLink protocol. This allows Clover users who are familiar with ROS to easily communicate with the flight controller in a user friendly way.

### SITL Example with PX4 powered COEX Clover gazebo simulator

{% embed url="https://www.youtube.com/watch?v=Zam59C7jYm8&ab_channel=SeanSmith" %}

### Hardware example with COEX Clover in OptiTrack motion capture volume

{% embed url="https://www.youtube.com/watch?v=Is1K0qvCZms" %}
