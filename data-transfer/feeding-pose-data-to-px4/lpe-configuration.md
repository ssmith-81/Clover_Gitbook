---
description: This page gives a brief outline on parameter setting for the LPE estimator.
---

# LPE Configuration

A basic outline for the parameter setup required to use the LPE estimator with the MoCap will be presented in this section.&#x20;

{% hint style="info" %}
It is said that LPE can accept MoCap data directly on the mocap mavros topics, although it will be remapped the same way as the ekf2 where it is accepted on the mavros vision topic.
{% endhint %}

* First choose LPE as the estimator from the Systems tab by setting <mark style="color:orange;">SYS\_MC\_EST\_GROUP = local\_position\_estimator</mark>&#x20;
* In the LPE parameter tab, set <mark style="color:red;">LPE\_FUSION</mark> to vision position where the MoCap will be the Clovers source for position estimates. No other source is needed in this setup.
* Use the heading from the MoCap by setting the <mark style="color:red;">ATT\_EXT\_HDG\_M</mark> to 'vision' where the mocap data is streamed on the mavros vision topic.
* It is a good idea to disable the use of the magnetometer when using the LPE for indoor flight as it may interfere with the external vision pose provided by the MoCap. This can be done by setting the <mark style="color:red;">ATT\_W\_MAG</mark> parameter to 0 bit which disables magnetometer use for orientation estimation.
* Another sensor to be aware of is the laser rangefinder. The altitude will be provided by the MoCap so this can be disabled in the clover.launch file or just disable altitude sensors such as LIDAR with the following PX4 parameters:
