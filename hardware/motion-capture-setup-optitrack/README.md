---
description: This section provides helpful information on motion capture system setup.
cover: ../../.gitbook/assets/Clover_flight.jpeg
coverY: 14
---

# Motion Capture Setup: OptiTrack

## Quick Start Resources

This section references the [OptiTrack Gitbook](https://docs.optitrack.com/) and sections out quick start material to allow the reader a guide to efficiently install and use OptiTrack motion capture system. A video format is provided in the following:

{% embed url="https://www.youtube.com/watch?v=aK1cpr6ShPE&t=2s&ab_channel=OptiTrack" %}

Preparing the motion capture volume requires a variety of checkpoints that must be followed to ensure a successful configuration, these can be found:

#### Preparing Capture Area

{% embed url="https://docs.optitrack.com/quick-start-guides/quick-start-guide-getting-started#preparing-the-capture-area" %}

{% embed url="https://docs.optitrack.com/hardware/prepare-setup-area" %}

With an ideal motion capture area, a quick guide on the placement and aiming of camera can be found:

#### Camera Mounting, Aiming and Placement

{% embed url="https://docs.optitrack.com/quick-start-guides/quick-start-guide-getting-started#placing-and-aiming-cameras" %}

{% embed url="https://docs.optitrack.com/hardware/camera-mount-structures" %}

{% embed url="https://docs.optitrack.com/hardware/camera-placement" %}

## By Example with 4 Camera Setup

A simplified process to my hardware configuration in the lab is provided with brief descriptions on my 4 camera Optitrack motion capture system configuration. The steps are as follows: &#x20;

* Mount the Flex-13 cameras onto the provided tripods, elevate the cameras 7 slots up and make sure the center view of each camera corresponds to the center ground position within the volume. I used the L-shape tool and motive software to ensure the camera were orientated correctly.
* Use two different OptiTrack hubs and plug two cameras into one hub (usb ports) and two cameras into the other hub.
* Connect the power ports of the hubs into a power bar with appropriate wires.
* Plug the usb type B into the port next to the power port (uplink port) and then connect to the usb port into the computer. Do this for each hub.
* Connect both hubs by connecting the hub sync-in port from one hub to the hub sync-out port of the other hub.

<div>

<figure><img src="../../.gitbook/assets/Mocap.jpg" alt=""><figcaption><p>Four camera Motion Capture System setup in the ACM Lab at Dalhousie University.</p></figcaption></figure>

 

<figure><img src="../../.gitbook/assets/Mocap_hub.jpg" alt=""><figcaption><p>Two cameras connected to MoCap hub.</p></figcaption></figure>

</div>

Once all the hardware is connected, the next step is camera setup and calibration within the motive software.
