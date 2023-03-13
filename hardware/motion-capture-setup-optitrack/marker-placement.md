---
description: This page describes OptiTrack markers and marker placement.
---

# Marker Placement

The first step is attaching the OptiTrack reflective markers to the Clover drone. A good spot is putting them on the top of the drone guard as seen in the following figure:

<figure><img src="../../.gitbook/assets/D8A8E9E8-115E-4044-B518-8269EC2EEC06.jpeg" alt=""><figcaption><p>COEX Clover with OptiTrack markers</p></figcaption></figure>

A list of keys points are made when attaching the markers:

1. Make sure the markers are clean.
2. The markers should be placed such that the shape it creates is asymmetric and they should also be placed as far apart as possible (to improve attitude readings).
3. The recommended number of markers for a rigid body is 4\~12, suggested [here](https://v30.wiki.optitrack.com/index.php?title=Rigid\_Body\_Tracking). 5 seems to be a good number, 4 also works.

{% hint style="info" %}
The importance of an asymmetrical marker setup is so the program does not get confused when tracking the rigid body orientation. It also allows one to easily determine the heading within the motive software. Also, if one were to use two Clovers in the same motion capture setup, it would be advised to use two different asymmetrical marker shapes to avoid concurrency when tracking multiple marker sets. A more extensive guide to the marker setup can be found [here](https://v30.wiki.optitrack.com/index.php?title=Markers).
{% endhint %}

