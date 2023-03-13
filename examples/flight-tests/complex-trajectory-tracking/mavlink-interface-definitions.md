---
description: Make table in gitbook  here
---

# MAVLink Interface Definitions

The definitions that are used through the MAVlink protocol can be seen in the following table:

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
One thing to take note is the MAVlink message above receives values assuming they are set in the North East Down (NED) reference frame which is the reference frame PX4 operates in. The reference frame ROS uses, and one many users are more intuitively familiar with, is the East North Up (ENU) frame where positive altitude is upwards. Fortunately this is the reference frame we will work in when publishing the setpoints using MAVROS, this is because the MAVROS node takes care of the reference frame conversion for us as can be seen in the [setpoint\_raw](https://github.com/mavlink/mavros/blob/ros2/mavros/src/plugins/setpoint\_raw.cpp) plugin. This is the plugin that will be used for publising setpoints discussed in the [MAVROS Topics](mavlink-interface-definitions.md#mavros-topics) section.
{% endhint %}

### MAVROS Topics

yyy
