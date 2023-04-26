---
description: >-
  Introduces the motion capture system setup with the COEX Clover and its
  purpose.
cover: .gitbook/assets/semi_logo_gitbook.jpg
coverY: 7.5025906735751295
---

# Motion Capture System with Clover

## Project description

This project is an educational reference and detailed tutorial on how to setup the OptiTrack Motion Capture system with the COEX Clover platform for the [CopterHack 2023](https://clover.coex.tech/en/copterhack2023.html#copterhack-2023) competition.&#x20;

The main focus of the project is the integration of the motion capture system with the Clover platform. Ideally the user has a basic understanding of the motion capture technology. Although, many resources are provided in the [HARDWARE section](broken-reference) to give the reader the needed understanding for this document. This includes brief descriptions on the camera and motive software setup with many resourceful links.

While the document is targeted for the OptiTrack system, the details provided will help the user better understand how to incorporate data fusion from many sensors with PX4. This is important because while the localization methods may change:

* External vision sensors for indoor flight (OptiTrack)
* Onboard computer vision sensors for indoor flight (ArUco marker system with camera)
* Global Positioning System (GPS) for outdoor flight

The idea remains the same, which is the drone needs to know where it is at (a position) so it can localize itself. Many cases involve data integration from multiple sensors to provide position and attitude. Thankfully the PX4 EKF is able to fuse data from a combination of many sensors.

The content of this document is based on a paper:

## Citation

Paper coming soon...

```
@misc{Smith2023Sean,
	   author = {Smith, Sean and Pan, Ya-Jun},
	   title = {A Linkage-Based Gripper Design with Optimized Data Transmission for Aerial Pick-and-Place Tasks},
	   year = {2023},
	   eprint = {}
}
```

### Grasping Experiment&#x20;

insert video

### Github

{% embed url="https://github.com/ssmith-81/MoCap_Clover" %}

### Final Video

{% embed url="https://www.youtube.com/watch?v=jOovjo0aBpQ&ab_channel=SeanSmith" %}
