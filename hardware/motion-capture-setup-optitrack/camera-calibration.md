---
description: Brief resource on optitrack camera calibration.
---

# Camera Calibration

## Quick Start Resources

A visual overview on this calibration process can be found in the following video:

{% embed url="https://www.youtube.com/watch?v=cNZaFEghTBU&embeds_euri=https%3A%2F%2Fcdn.iframe.ly%2F&feature=emb_imp_woyt" %}

A detailed guide to beginning calibration can be found:

{% embed url="https://docs.optitrack.com/motive/calibration#starting-a-new-calibration" %}

### Main Steps

There are two main steps that need to be considered:

1. Collect samples using the wand stick and markers
2. Use the L-shape tool to set the ground plane. Ground plane refinement.

1\) With a new calibration activated, the next step is wanding and capturing samples from the cameras:

{% embed url="https://docs.optitrack.com/motive/calibration#wanding" %}

{% hint style="info" %}
**Wanding Tips**

* Avoid waving the wand too fast. This may introduce bad samples.
* Avoid wearing reflective clothing or accessories while wanding. This can introduce extraneous samples which can negatively affect the calibration result.
* Try not to collect samples beyond 10,000. Extra samples could negatively affect the calibration.

From my experience with my 4 camera setup, about 7000-9000 samples for each camera seems work well for calibration.

* Try to collect wanding samples covering different areas of each camera view. The status indicator on Prime cameras can be used to monitor the sample coverage on individual cameras.
* Although it is beneficial to collect samples all over the volume, it is sometimes useful to collect more samples in the vicinity of the target regions where more tracking is needed. By doing so, calibration results will have a better accuracy in the specific region.

I had a relatively small motion capture volume, however I would give extra samples in the landing/takeoff zone as well as its hovering position about 1m off the ground.
{% endhint %}

2\) After wanding and calibrating the cameras, the final step of the calibration process is setting the ground plane and the origin. This is accomplished by placing the calibration square in your volume and telling Motive where the calibration square is. Setting the ground plane with the L-frame should be done intently because it will set the global coordinate axes as well as the ground plane for the capture volume

Details outlining this process can be found:

{% embed url="https://docs.optitrack.com/motive/calibration#ground-plane-and-origin" %}

This also depends on which version of the motive software is being used and the type of calibration square, with versions presented in the following:

{% embed url="https://docs.optitrack.com/motive/calibration/calibration-squares" %}

Once the x-axis direction is determined, this will be needed later when creating the rigid body because the Clover must be properly alligned with it.
