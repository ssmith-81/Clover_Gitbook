---
description: This page describes the rigid body tracking in motive.
---

# Rigid Body Tracking and Data Streaming

In Motive, Rigid Bodies are used for tracking rigid, unmalleable, objects. A set of markers securely attached to the Clover, with respective placement information gets used to identify and report 6 Degree of Freedom (6DoF) data. A detailed document on rigid body selection, creation and tracing can be found:

{% embed url="https://docs.optitrack.com/motive/rigid-body-tracking" %}

### Creating a Rigid Body

The steps needed to create rigid body are:

* Align the Clovers forward direction with the [system +x-axis](https://v20.wiki.optitrack.com/index.php?title=Template:Coordinate\_System).
* Define a rigid body in the Motive software:&#x20;

{% embed url="https://www.youtube.com/watch?ab_channel=AcuityInc&v=1e6Qqxqe-k0" %}
OptiTrack Motive: Intro to Rigid Bodies
{% endembed %}

* This is simply done by using the following steps:

**Step 1.**

Select all associated Rigid Body markers in the [3D viewport](https://docs.optitrack.com/motive-ui-panes/viewport).

**Step 2.**

On the Builder pane, confirm that the selected markers match the markers that you wish to define the Rigid Body from.

**Step 3.**

Click _Create_ to define a Rigid Body asset from the selected markers.

**Step 4.**

Once the Rigid Body asset is created, the markers will be colored (labeled) and interconnected to each other. The newly created Rigid Body will be listed under the [Assets pane](https://docs.optitrack.com/motive-ui-panes/assets-pane).

More details can be found in the [Creating Rigid Body](https://docs.optitrack.com/motive/rigid-body-tracking#creating-rigid-body).

* Give the Clover a name that does not contain spaces, e.g. clover1 instead of clover 1. Rigidbody1 works fine too.

### Streaming pose data from Motive

An overview on data streaming from motive can be found:

{% embed url="https://docs.optitrack.com/motive/data-streaming#streaming-in-motive" %}

Selecting the [streaming IP address](https://docs.optitrack.com/motive/data-streaming#streaming-ip-address) must be configured properly as stated:

> It is important to select the network adapter (interface, IP Address) for streaming data. Most Motive Host PCs will have multiple network adapters - one for the camera network and one (or more) for the local area network (LAN). Motive will only stream over the selected adapter (interface). Select the desired interface using the **Streaming** tab in Motive's Settings. The interface can be either over a local area network (LAN) or on the same machine (localhost, local loopback). If both server (Motive) and client application are running on the same machine, set the network interface to the local loopback address (127.0.0.1). When streaming over a LAN, select the IP address of the network adapter connected to the LAN. This will be the same address the Client application will use to connect to Motive.

We want the Clover and Motive machine connected to the same router, where in this case we can select the IP address assigned from this network. Keep a note of this IP address as it will be used when setting the data stream in the following steps.

{% hint style="danger" %}
Firewall or anti-virus software can block network traffic, so make sure these applications are disabled or configured to allow data transfer from Motive to the Clover.
{% endhint %}

The data streaming method used in this tutorial is [VRPN streaming](https://docs.optitrack.com/motive/data-streaming#vrpn-sample). However, I implemented a UDP streaming method in conjunction with [NatNek SDK](https://docs.optitrack.com/motive/data-streaming#natnet-sdk) that is more reliable and I prefer. Providing a brief tutorial in the future on this method is on my to do list. For VRPN streaming, follow these steps:

* [Enable Frame Broadcast and VRPN streaming](https://v22.wiki.optitrack.com/index.php?title=Data\_Streaming\_Pane): inser pic
* Note down the IP address given to the OptiTrack PC as it will be used when feeding pose data to the Clover: insert pic
* In the Advanced Network Options in the Data Streaming pane set the Up axis to be the Z-axis (the default is Y). Insert pic:

{% hint style="warning" %}
It is very important that you align the forward direction of the Clover with the x-axis of your motion capture volume before creating the rigid body in motive. This x-axis was previously defined when setting the ground plane during calibration (it can simply be viewed in the motive software). If this is not done, the external vision data may not fuse with the onboard pose estimate i.e. fusion errors. If it does fuse, the yaw offset between the drones body frame and motion capture system reference frame will result in the Clover being unable to follow set-points i.e. uncontrollable.
{% endhint %}
