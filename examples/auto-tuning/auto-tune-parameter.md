---
description: >-
  This section gives the reader a basic understanding of the tunable auto-tune
  module parameter.
---

# Auto-Tune Parameter

## Tunable PX4 Meta-Parameters for Auto-Tune Module

The main parameter that needs adjustment when setting the auto-tune module is the <mark style="color:purple;">MC\_AT\_RISE\_TIME</mark> parameter. This can simply be described as:

> In PID control, the rise time refers to the time it takes for the system to reach its desired setpoint from its initial state. Specifically, it is the time required for the system output to rise from a specified percentage (often 10% or 90%) of the steady-state value to the setpoint value for the first time after a step change in the input. A shorter rise time indicates a faster response and a more agile system. However, too short of a rise time can result in overshooting and oscillations, which can lead to instability and poor control performance. Balancing the rise time with other control objectives, such as overshoot and settling time, is an important aspect of designing a PID control system.

Further understanding can be interpreted from this graph:

<figure><img src="../../.gitbook/assets/pid (1).png" alt=""><figcaption></figcaption></figure>

This is the systems closed loop unit step response. The system of interest is the attitude dynamics. As said previously, a shorter defined rise time indicates a faster response, this may be good when using small racer drones where fast adjustments are beneficial and possible. However a quick risetime may not be feasible for certain drones where it will attempt but overshoot and oscillate with poor control. This may be the case for larger drones.&#x20;

Upon auto-tuning the Clover, run an autonomous flight and analyze how well it maintains its level horizontal level and tracks setpoints. While tracking can be improved with the outer control module (position and velocity) its orientation/attitude control will have a larger impact on the Clovers overall flight performance.
