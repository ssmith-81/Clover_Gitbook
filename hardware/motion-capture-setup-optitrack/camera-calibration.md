---
description: Brief resource on optitrack camera calibration.
---

# Camera Calibration

There are three main steps that need to be considered:

1. Collect samples using the wand stick and markers
2. Use the L-shape tool to set the ground plane
3. Ground refinement

A detailed document on performing the camera calibration can be found [here](https://v30.wiki.optitrack.com/index.php?title=Calibration). A video tutorial for camera calibration is provided:

{% embed url="https://www.youtube.com/watch?ab_channel=OptiTrack&v=cNZaFEghTBU" %}
Optitrack camera calibration
{% endembed %}

{% hint style="info" %}
For the calibration, try to ensure the majority of the view of each camera has been sampled and with this 4 camera setup, about 8000-9000 samples for each camera seems work well for calibration.
{% endhint %}

{% hint style="info" %}
After wanding and calibrating the cameras; setting the ground plane with the L-frame should be done intently because it will set the orientation of the motion capture volume reference frame. This also depends on which version of the motive software is being used and these conventions can be found [here](https://v20.wiki.optitrack.com/index.php?title=Calibration\_pane##Ground\_Plane). Once the x-axis direction is determined, this will be needed later when creating the rigid body.
{% endhint %}
