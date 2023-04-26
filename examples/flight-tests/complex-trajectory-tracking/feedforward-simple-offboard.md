# Feedforward Simple Offboard

This section describes the changes made to the simple offboard setpoint publishing module provided by COEX to provide feedforward velocity and acceleration for a tighter reference trajectory. Due to the nature of the publishing (linear point tracking) there are pros and cons to this which will be discussed at the end of the section.&#x20;

{% hint style="info" %}
As a side note, the reason I implemented this is because my nonlinear control system in my research required velocity and acceleration publishing as it is model based. This is not completely necessary in most applications, but the feedforward idea is the same as the complex trajectory tracking example.
{% endhint %}

The simple offboard module can be found [here](https://github.com/CopterExpress/clover/blob/master/clover/src/simple\_offboard.cpp) while the modified feedforward version can be found [here](https://github.com/ssmith-81/MoCap\_Clover/blob/master/simple\_offboard.cpp). We will go through the specific modifications and discuss what they do in the next section.

## simple\_offboard.cpp feedforward modifications

The first function to look at is presented:

{% code lineNumbers="true" %}
```cpp
void getNavigateSetpoint(const ros::Time& stamp, float speed, Point& nav_setpoint)
{
	if (wait_armed) {
		// don't start navigating if we're waiting arming
		nav_start.header.stamp = stamp;
	}

	float distance = getDistance(nav_start.pose.position, setpoint_position_transformed.pose.position);
	float time = distance / speed;
	float passed = std::min((stamp - nav_start.header.stamp).toSec() / time, 1.0); // normalized time value that increases from 0->1
	
	// Seperate speed into x,y,z components by normalizing displacement vector (unit vector) and calculating speed components from it
	// Example: Vx = (|rx|/|rtotal|)*speed
	float vx = ((setpoint_position_transformed.pose.position.x - nav_start.pose.position.x)/distance)*speed;
	float vy = ((setpoint_position_transformed.pose.position.y - nav_start.pose.position.y)/distance)*speed;
	float vz = ((setpoint_position_transformed.pose.position.z - nav_start.pose.position.z)/distance)*speed;
	
	
	
	nav_setpoint.x = nav_start.pose.position.x + (setpoint_position_transformed.pose.position.x - nav_start.pose.position.x) * passed;
	nav_setpoint.y = nav_start.pose.position.y + (setpoint_position_transformed.pose.position.y - nav_start.pose.position.y) * passed;
	nav_setpoint.z = nav_start.pose.position.z + (setpoint_position_transformed.pose.position.z - nav_start.pose.position.z) * passed;
	
	// From output analysis, as soon as passed equals 1, all position setpoints hit their constant value in x,y,z at the same time. 
	// i.e this means the velocity at this point should be zero (feedforward) where it reached the desired destination.
	// All the time until that point, the feedforward velocity is the slope of the lines calculated above with vx, vy, vz
	
	// When the Clover reaches its destination, that second term i zero. So need to publish feedforward velocity until that term is zero. Then feedforward velocity is zero once setpoint destination is reached/calculated
	// feedforward acceleration will always be zero with this linear point to point tracking function
	if (fabs(passed-1.0f)<1e-6){
	
		velx = 0.0f;
		vely = 0.0f;
		velz = 0.0f;
		
	
	}else{
	
		velx = vx;
		vely = vy;
		velz = vz;
	
	}
}
```
{% endcode %}

The above function retrieves the setpoints for the navigate function. There are two main changes in the getNavigateSetpoint function:

1. Calculation of vx, vy, and vz in lines 14-16 with the following equation:

$$
V_x = \frac{|\hat{r}_x|}{|\hat{r}_{total}|}*speed
$$

Once each component is calculated, deciding what velocity value to publish was the next step.

2. Once passed=1, meaning the Clover has reached its destination then we switch over to publishing a velocity of 0. But until then, the constant velocity calculated with the equation from step 1 is used. The lines making these decisions are 30-43.

The second function to look at is publishing function, the one used for publishing the setpoints. The modified version can be seen:

{% code lineNumbers="true" %}
```cpp
void publish(const ros::Time stamp)
{
	if (setpoint_type == NONE) return;

	position_raw_msg.header.stamp = stamp;
	thrust_msg.header.stamp = stamp;
	rates_msg.header.stamp = stamp;

	try {
		// transform position and/or yaw
		if (setpoint_type == NAVIGATE || setpoint_type == NAVIGATE_GLOBAL || setpoint_type == POSITION || setpoint_type == VELOCITY || setpoint_type == ATTITUDE) {
			setpoint_position.header.stamp = stamp;
			tf_buffer.transform(setpoint_position, setpoint_position_transformed, local_frame, ros::Duration(0.05));
		}

		// transform velocity
		if (setpoint_type == VELOCITY) {
			setpoint_velocity.header.stamp = stamp;
			tf_buffer.transform(setpoint_velocity, setpoint_velocity_transformed, local_frame, ros::Duration(0.05));
		}

	} catch (const tf2::TransformException& e) {
		ROS_WARN_THROTTLE(10, "can't transform");
	}

	if (setpoint_type == NAVIGATE || setpoint_type == NAVIGATE_GLOBAL) {
		position_msg.pose.orientation = setpoint_position_transformed.pose.orientation; // copy yaw
		position_raw_msg.yaw = tf::getYaw(setpoint_position_transformed.pose.orientation); // copy yaw to raw message we are using now (for YAW condition)
		getNavigateSetpoint(stamp, nav_speed, position_msg.pose.position);

		if (setpoint_yaw_type == TOWARDS) {
			double yaw_towards = atan2(position_msg.pose.position.y - nav_start.pose.position.y,
			                           position_msg.pose.position.x - nav_start.pose.position.x);
			position_raw_msg.yaw = yaw_towards; // copy yaw to raw message we are using now (for the TOWARDS condition)
			position_msg.pose.orientation = tf::createQuaternionMsgFromRollPitchYaw(0, 0, yaw_towards);
		}
	}

	if (setpoint_type == POSITION) {
		position_msg = setpoint_position_transformed;
	}

	if (setpoint_type == POSITION || setpoint_type == NAVIGATE || setpoint_type == NAVIGATE_GLOBAL) {
		position_msg.header.stamp = stamp;

		if (setpoint_yaw_type == YAW || setpoint_yaw_type == TOWARDS) {
		
		// From clover website (yaw_rate will be used if yaw is set to NaN) i.e the next else statement is used when yaw_rate is set or yaw is set to NaN
		// position_msg container was already gotten from the getNavigateSetpoint function call above. We want to feed forward velocity and acceleration here
			//position_pub.publish(position_msg); --> not publishing to position setpoint anymore, we are using position raw for greater flexibility
			
		// my implementation using feedforward velocity, acceleation 
	 		position_raw_msg.type_mask = PositionTarget::IGNORE_YAW_RATE;
			
			//double yaw_towards = atan2(position_msg.pose.position.y - nav_start.pose.position.y,
			//                           position_msg.pose.position.x - nav_start.pose.position.x);
			                             
			// Set yaw value from above if statements
			position_raw_msg.position = position_msg.pose.position;
			position_raw_msg.velocity.x = velx;
			position_raw_msg.velocity.y = vely;
			position_raw_msg.velocity.z = velz;
			position_raw_msg.acceleration_or_force.x = 0.0f;
			position_raw_msg.acceleration_or_force.y = 0.0f;
			position_raw_msg.acceleration_or_force.z = 0.0f;
			position_raw_pub.publish(position_raw_msg);

		} else {
			position_raw_msg.type_mask = PositionTarget::IGNORE_VX +
			                             PositionTarget::IGNORE_VY +
			                             PositionTarget::IGNORE_VZ +
			                             PositionTarget::IGNORE_AFX +
			                             PositionTarget::IGNORE_AFY +
			                             PositionTarget::IGNORE_AFZ +
			                             PositionTarget::IGNORE_YAW;
			position_raw_msg.yaw_rate = setpoint_yaw_rate;
			position_raw_msg.position = position_msg.pose.position;
			position_raw_pub.publish(position_raw_msg);
		}

		// publish setpoint frame
		if (!setpoint.child_frame_id.empty()) {
			if (setpoint.header.stamp == position_msg.header.stamp) {
				return; // avoid TF_REPEATED_DATA warnings
			}

			setpoint.transform.translation.x = position_msg.pose.position.x;
			setpoint.transform.translation.y = position_msg.pose.position.y;
			setpoint.transform.translation.z = position_msg.pose.position.z;
			setpoint.transform.rotation = position_msg.pose.orientation;
			setpoint.header.frame_id = position_msg.header.frame_id;
			setpoint.header.stamp = position_msg.header.stamp;
			transform_broadcaster->sendTransform(setpoint);
		}
	}

	if (setpoint_type == VELOCITY) {
		position_raw_msg.type_mask = PositionTarget::IGNORE_PX +
		                             PositionTarget::IGNORE_PY +
		                             PositionTarget::IGNORE_PZ +
		                             PositionTarget::IGNORE_AFX +
		                             PositionTarget::IGNORE_AFY +
		                             PositionTarget::IGNORE_AFZ;
		position_raw_msg.type_mask += setpoint_yaw_type == YAW ? PositionTarget::IGNORE_YAW_RATE : PositionTarget::IGNORE_YAW;
		position_raw_msg.velocity = setpoint_velocity_transformed.vector;
		position_raw_msg.yaw = tf2::getYaw(setpoint_position_transformed.pose.orientation);
		position_raw_msg.yaw_rate = setpoint_yaw_rate;
		position_raw_pub.publish(position_raw_msg);
	}

	if (setpoint_type == ATTITUDE) {
		attitude_pub.publish(setpoint_position_transformed);
		thrust_pub.publish(thrust_msg);
	}

	if (setpoint_type == RATES) {
		// rates_pub.publish(rates_msg);
		// thrust_pub.publish(thrust_msg);
		// mavros rates topics waits for rates in local frame
		// use rates in body frame for simplicity
		att_raw_msg.header.stamp = stamp;
		att_raw_msg.header.frame_id = fcu_frame;
		att_raw_msg.type_mask = AttitudeTarget::IGNORE_ATTITUDE;
		att_raw_msg.body_rate = rates_msg.twist.angular;
		att_raw_msg.thrust = thrust_msg.thrust;
		attitude_raw_pub.publish(att_raw_msg);
	}
}

```
{% endcode %}

The changes made will be outlined assuming the navigate function is being used (corresponding to line 43). The important sequence of changes are listed:

1. Previously the position [setpoint\_position](http://docs.ros.org/en/api/geometry\_msgs/html/msg/PoseStamped.html) was used for publishing setpoints from navigate function. Now we would like to use the [setpoint\_raw](http://docs.ros.org/en/api/mavros\_msgs/html/msg/PositionTarget.html) which offers more flexibility by publishing what the user desires. To do this, line 50 was commented out which previously published the position\_msg. Then on line 53 the new publishing message position\_raw\_msg was set to publish everything except yaw rate. This means you can set the yaw, or not and it will remain as is.

{% hint style="info" %}
Keep in mind, if one desires to set the yaw rate, then feedforward velocity and acceleration (zero) are not provided. This can be changed with some minor changes (starting the with type\_mask on line 69).
{% endhint %}

2. Each state published is defined in lines (59-65) except for yaw which is defined on 28 or 34 depending on whether yaw is provided by the user with the navigate function (28), or set to face towards the target location (34). Position setpoint is updated on line 29 with the getNavigateSetpoint function (along with the velocity setpoints) and then set on lines (59-62). The acceleration setpoints are set to zero in lines (63-65) and are zero for linear trajectory tracking.

### Cons:

The issue with publishing feedforward velocity using this publishing method, is having discontinuities in the velocity (the function is not smooth). It will go from setting a constant value directly to zero. At low speeds this is not an issue but when the velocity increases it will overshoot the target location some as the Clover tries to quickly adjust to the changes velocity setpoint. Setting lower speeds can help adjust for this as the Clover tracks more closely (assuming well tuned controllers).

## Using Simple Offboard Feedforward with SITL

The modified Simple Offboard feedforward file ([simple\_offboard.cpp](https://github.com/ssmith-81/MoCap\_Clover/blob/master/simple\_offboard.cpp)) can be downloaded from the following repository:

{% embed url="https://github.com/ssmith-81/MoCap_Clover/blob/master/figure8_com.py" %}

with the code details explained [above](feedforward-simple-offboard.md). Once downloaded, the user can navigate to the current simple offboard file location within the simulation workspace:&#x20;

```bash
cd ~/catkin_ws/src/clover/clover/src
```

you can then delete the current [simple\_offboard.cpp](https://github.com/CopterExpress/clover/blob/master/clover/src/simple\_offboard.cpp) with the rm command:

```bash
rm simple_offboard.cpp
```

Replace it with the downloaded feedforward version in the same location (`catkin_ws/src/clover/clover/src`) then rebuild the workspace with the following:

```bash
cd ~/catkin_ws
catkin_make
```

## Using the Simple Offboard feedforward for experiment

The process for real Clover testing is the same as simulation, we just need to get the file from your computer to the Raspberry Pi onboard the Clover. The fastest way to copy files to your Raspberry Pi is with SCP, which stands for “secure copy”. The following steps assume the user has successfully configured the Clover Raspberry Pi for autonomous flight with the motion capture system, i.e. connected to an external network with internet.

1. Download the [simple\_offboard.cpp](https://github.com/ssmith-81/MoCap\_Clover/blob/master/simple\_offboard.cpp) script from the following repository:

{% embed url="https://github.com/ssmith-81/MoCap_Clover" %}

2. The Clover is connected to an external network with internet access as described in Section [Network Topology and Raspberry Pi Configuration](../../../data-transfer/feeding-pose-data-into-ros-on-raspberry-pi/network-topology-and-raspberry-pi-configuration.md). Therefore the following scp command can be used on the users computer (connected on the same network) to transfer the simple offboard C++ script `simple_offboard.cpp` from the current folder on your pc to the `src` folder within the Clover workspace on the Raspberry Pi:

```bash
scp simple_offboard.cpp pi@192.168.0.187:catkin_ws/src/clover/clover/src
```

This is assuming `192.168.0.187` is the raspberry Pi IP address, replace it with your IP address once you [identify it](../../../data-transfer/feeding-pose-data-into-ros-on-raspberry-pi/network-topology-and-raspberry-pi-configuration.md). More details on the scp command can be found [here](https://howchoo.com/pi/how-to-transfer-files-to-the-raspberry-pi).
