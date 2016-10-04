---
layout: page
title: Rolling Shutter Change Detection
---
## Abstract
The coalesced presence of motion blur and rolling shutter effect is
unavoidable due to the sequential exposure of sensor rows in CMOS
cameras. We address the problem of detecting changes in an image
affected by motion blur and rolling shutter artifacts with respect to
a reference image. Our framework bundles modelling of motion blur in
global shutter and rolling shutter cameras into a single entity. We
leverage the sparsity of the camera trajectory in the pose space and
the sparsity of occlusion in spatial domain to propose an optimization
problem that not only registers the reference image to the observed
distorted image but detects occlusions as well, both within a single
framework.

## Publications
Change Detection in the Presence of Motion Blur and Rolling Shutter Effect<br>
Vijay Rengarajan, A.N. Rajagopalan, and R. Aravind<br>
European Conference on Computer Vision (ECCV)<br>
Zurich, September 2014<br>
[Paper](../pdf/2014_eccv.pdf) [Poster](../pdf/2014_eccv_poster.pdf)
<br>

Illumination Robust Change Detection with CMOS Imaging Sensors<br>
Vijay Rengarajan, Sheetal B. Gupta, A.N. Rajagopalan, and Guna Seetharaman<br>
SPIE Defense + Security Symposium, International Society for Optics and Photonics<br>
Baltimore, Maryland, April 2015<br>
[Paper](../pdf/2015_spie.pdf) [Slides](../pdf/2015_spie_present.pdf)

## Results
Given a reference image and an RS/RSMB image, we estimate the row-wise camera motion between the two images to register them, and simultaneously detect the regions of changes between them.
![alt text](../img/rs_cd_rsmb_eg1.png "Curved road")
![alt text](../img/rs_cd_rsmb_eg2.png "Sheared road")