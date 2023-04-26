---
description: Make table in gitbook  here
---

# MAVLink Interface Definitions

The definitions that are used through the MAVlink protocol for the complex trajectory tracking are defined in the [SET\_POSITION\_TARGET\_LOCAL\_NED](https://mavlink.io/en/messages/common.html#SET\_POSITION\_TARGET\_LOCAL\_NED) message and can be seen in the following table:

| Field Name        | Type      | Units | Value                                                                                               | Description                                                                                                                    |
| ----------------- | --------- | ----- | --------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| time\_boot\_ms    | uint32\_t | ms    |                                                                                                     | Timestamp (time since system boot)                                                                                             |
| coordinate\_frame | uint8\_t  |       | [MAV\_FRAME](https://mavlink.io/en/messages/common.html#MAV\_FRAME)                                 | Variety of valid options are available listed [here](https://mavlink.io/en/messages/common.html#POSITION\_TARGET\_LOCAL\_NED). |
| type\_mask        | uint16\_t |       | [POSITION\_TARGET\_TYPEMASK](https://mavlink.io/en/messages/common.html#POSITION\_TARGET\_TYPEMASK) |                                                                                                                                |
| x                 | float     | m     |                                                                                                     | X Position in NED frame                                                                                                        |
| y                 | float     | m     |                                                                                                     | Y Position in NED frame                                                                                                        |
| z                 | float     | m     |                                                                                                     | Z Position in NED frame (note, altitude is negative in NED)                                                                    |
| vx                | float     | m/s   |                                                                                                     | X velocity in NED frame                                                                                                        |
| vy                | float     | m/s   |                                                                                                     | Y velocity in NED frame                                                                                                        |
| vz                | float     | m/s   |                                                                                                     | Z velocity in NED frame                                                                                                        |
| afx               | float     | m/s/s |                                                                                                     | X acceleration or force (if bit 10 of type\_mask is set) in NED frame in meter / s^2 or N                                      |
| afy               | float     | m/s/s |                                                                                                     | Y acceleration or force (if bit 10 of type\_mask is set) in NED frame in meter / s^2 or N                                      |
| afz               | float     | m/s/s |                                                                                                     | Z acceleration or force (if bit 10 of type\_mask is set) in NED frame in meter / s^2 or N                                      |
| yaw               | float     | rad   |                                                                                                     | yaw setpoint                                                                                                                   |
| yaw\_rate         | float     | rad/s |                                                                                                     | yaw rate setpoint                                                                                                              |

From this list of definition, all of them can be set or a select few, it all depends on what the user desires before setting the type\_mask field appropriately. For the tracking in this example, we will set every field to ensure ideal tracking of the trajectory.&#x20;

{% hint style="info" %}
One thing to take note is the MAVlink message above receives values assuming they are set in the North East Down (NED) reference frame which is the reference frame PX4 operates in. The reference frame ROS uses, and one many users are more intuitively familiar with, is the East North Up (ENU) frame where positive altitude is upwards. Fortunately this is the reference frame we will work in when publishing the setpoints using MAVROS, this is because the MAVROS node takes care of the reference frame conversion for us as can be seen in the [setpoint\_raw](https://github.com/mavlink/mavros/blob/ros2/mavros/src/plugins/setpoint\_raw.cpp) plugin. This is the plugin that will be used for publishing setpoints discussed in the [MAVROS Topics](mavlink-interface-definitions.md#mavros-topics) section.
{% endhint %}

### MAVROS Topics

A complete list of MAVROS topics can be found:

{% embed url="http://wiki.ros.org/mavros" %}

The important MAVROS topics and services used with Clover can be referenced from their website:

{% embed url="https://clover.coex.tech/en/mavros.html#mavros" %}

Where the information is repeated here:

### Main services <a href="#main-services" id="main-services"></a>

`/mavros/set_mode` — set [flight mode](https://clover.coex.tech/en/modes.html) of the controller. Most often used to set the OFFBOARD mode to accept commands from Raspberry Pi.

`/mavros/cmd/arming` — arm or disarm drone motors (change arming status).

### Main published topics <a href="#main-published-topics" id="main-published-topics"></a>

`/mavros/state` — status of connection to the flight controller and flight controller mode.

`/mavros/local_position/pose` — local position and orientation of the copter in the ENU coordinate system.

`/mavros/local_position/velocity` — current speed in local coordinates and angular velocities.

`/mavros/global_position/global` — current global position (latitude, longitude, altitude).

`/mavros/global_position/local` — the global position in the [UTM](https://en.wikipedia.org/wiki/Universal\_Transverse\_Mercator\_coordinate\_system) coordinate system.

`/mavros/global_position/rel_alt` — relative altitude (relative to the arming altitude).

Messages published in the topics may be viewed with the `rostopic` utility, e.g., `rostopic echo /mavros/state`. See more in [working with ROS](https://clover.coex.tech/en/ros.html).

### Main topics for publication <a href="#main-topics-for-publication" id="main-topics-for-publication"></a>

`/mavros/setpoint_position/local` — set target position and yaw of the drone (in the ENU coordinate system).

`/mavros/setpoint_position/global` – set target position in global coordinates (latitude, longitude, altitude) and yaw of the drone.

`/mavros/setpoint_position/cmd_vel` — set target linear velocity of the drone.

`/mavros/setpoint_attitude/attitude` and `/mavros/setpoint_attitude/att_throttle` — set target attitude and throttle level.

`/mavros/setpoint_attitude/cmd_vel` and `/mavros/setpoint_attitude/att_throttle` — set target angular velocity and throttle level.

#### Topics for sending raw packets <a href="#topics-for-sending-raw-packets" id="topics-for-sending-raw-packets"></a>

`/mavros/setpoint_raw/local` — sends [SET\_POSITION\_TARGET\_LOCAL\_NED](https://mavlink.io/en/messages/common.html#SET\_POSITION\_TARGET\_LOCAL\_NED) message. Allows setting target position/target speed and target yaw/angular yaw velocity. The values to be set are selected using the `type_mask` field.

`/mavros/setpoint_raw/attitude` — sends [SET\_ATTITUDE\_TARGET](https://mavlink.io/en/messages/common.html#SET\_ATTITUDE\_TARGET) message. Allows setting the target attitude /angular velocity and throttle level. The values to be set are selected using the `type_mask` field

`/mavros/setpoint_raw/global` — sends [SET\_POSITION\_TARGET\_GLOBAL\_INT](https://mavlink.io/en/messages/common.html#SET\_POSITION\_TARGET\_GLOBAL\_INT). Allows setting the target attitude in global coordinates (latitude, longitude, altitude) and flight speed.
