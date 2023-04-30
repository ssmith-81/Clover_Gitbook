---
description: Tracking Bernoulli's Lemniscate (Figure 8) using MAVROS
---

# Complex Trajectory Tracking

<figure><img src="../../../.gitbook/assets/Lemniscate_of_Bernoulli (1).gif" alt=""><figcaption><p>Graphical representation of Bernoulli's Lamniscate [<a href="https://upload.wikimedia.org/wikipedia/commons/f/f1/Lemniscate_of_Bernoulli.gif">reference</a>].</p></figcaption></figure>

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

## Using the complex trajectory tracking script for SITL

The complex trajectory tracking python script ([figure8\_com.py](http://localhost:5000/s/q0NsGVgxmRD8c4yuqaAR/use-cases/for-support/intercom-integration)) can be downloaded from the following repository:

{% embed url="https://github.com/ssmith-81/MoCap_Clover/blob/master/figure8_com.py" %}

with the code details explained in section [Code Details](code-details.md). Once downloaded and properly configured for SITL simulations by the user, it can be easily run in the [Clover Gazebo simulator](https://clover.coex.tech/en/simulation.html) with the following:

```bash
python3 figure8_com.py
```

Setting up the Clover Gazebo simulator can be done with either of the following options (Native or using a virtual machine with the preconfigured image):

{% embed url="https://clover.coex.tech/en/simulation_native.html" %}

{% embed url="https://clover.coex.tech/en/simulation_vm.html" %}

### SITL Example with PX4 powered COEX Clover gazebo simulator

{% embed url="https://www.youtube.com/watch?v=Zam59C7jYm8&ab_channel=SeanSmith" %}

## Using the complex trajectory tracking script for experiment

#### Raspberry Pi using external network with Motion Capture System Setup

Using this script onboard the Clover in hardware applications requires transferring the file to the Clover environment on the Raspberry Pi. The fastest way to copy files to your Raspberry Pi is with SCP, which stands for “secure copy”.&#x20;

1. Download the [figure8\_com.py](https://github.com/ssmith-81/MoCap\_Clover/blob/master/figure8\_com.py) script onto your computer from the following repository:

{% embed url="https://github.com/ssmith-81/MoCap_Clover" %}

2. &#x20;The Clover is connected to an external network with internet access as described in Section [Network Topology and Raspberry Pi Configuration](../../../data-transfer/feeding-pose-data-into-ros-on-raspberry-pi/network-topology-and-raspberry-pi-configuration.md). Therefore the following scp command can be used on the users computer (connected on the same network) to transfer the complex trajectory python script `figure8_com.py` from the current folder on your pc to the `examples` folder within the Clover workspace on the Raspberry Pi:

```bash
scp figure8_com.py pi@192.168.0.187:examples/
```

This is assuming `192.168.0.187` is the raspberry Pi IP address, replace it with your IP address once you [identify it](../../../data-transfer/feeding-pose-data-into-ros-on-raspberry-pi/network-topology-and-raspberry-pi-configuration.md). More details on the scp command can be found [here](https://howchoo.com/pi/how-to-transfer-files-to-the-raspberry-pi).

{% hint style="info" %}
These steps assume the user configured the raspberry Pi to connect to an external network with internet access. The [Clover image](https://clover.coex.tech/en/image.html) provided by COEX configures the Raspberry Pi to use its own network by default with the ArUco marker vision based navigation/localization. This trajectory tracking method can be used with this configuration as well (refer to [Code Details](code-details.md)).&#x20;
{% endhint %}

#### Raspberry Pi using its own network with preconfigured image

With the Clover image, the Raspberry Pi uses its own network i.e provides a preconfigured Wi-Fi access point with an SSID `clover-xxxx`, where `xxxx` are four random numbers that are assigned when your Raspberry Pi is run for the first time. The password is `cloverwifi`. More detailed can be found on the Clover website:

{% embed url="https://clover.coex.tech/en/wifi.html#connecting-to-clover-via-wi-fi" %}

Once connected to the Raspberry Pi's access point the following scp command can be used on the users computer in a new terminal to transfer the complex trajectory python script `figure8_com.py` from the current folder on your pc to the `examples` folder within the Clover workspace on the Raspberry Pi:

```bash
scp figure8_com.py pi@192.168.11.1:examples/
```

### Hardware example with COEX Clover in OptiTrack motion capture volume

{% embed url="https://www.youtube.com/watch?v=Is1K0qvCZms" %}
