---
layout: page
title: Rolling Shutter Change Detection
---
## Abstract
We address the problem of registering a distorted image and a reference image of the same scene by estimating the camera motion that had caused the distortion. We simultaneously detect the regions of changes between the two images. We attend to the coalesced effect of rolling shutter and motion blur that occurs frequently in moving CMOS cameras. We first model a general image formation framework for a 3D scene following a layered approach in the presence of rolling shutter and motion blur. We then develop an algorithm which performs layered registration to detect changes. This algorithm includes an optimisation problem that leverages the sparsity of the camera trajectory in the pose space and the sparsity of changes in the spatial domain. We create a synthetic dataset for change detection in the presence of motion blur and rolling shutter effect covering different types of camera motion for both planar and 3D scenes. We compare our method with existing registration methods and also show several real examples captured with CMOS cameras.

## Publications
<b>Image Registration and Change Detection under Rolling Shutter Motion Blur</b><br>
Vijay Rengarajan, A.N.Rajagopalan, R.Aravind, and Guna Seetharaman<br>
IEEE Transactions on Pattern Analysis and Machine Intelligence (PAMI)<br>
November 2016<br>
[\[Paper\]](../pdf/2016_tpami.pdf)
<br>

<b>Change Detection in the Presence of Motion Blur and Rolling Shutter Effect</b><br>
Vijay Rengarajan, A.N. Rajagopalan, and R. Aravind<br>
European Conference on Computer Vision (ECCV)<br>
Zurich, September 2014<br>
[Paper](../pdf/2014_eccv.pdf) [Poster](../pdf/2014_eccv_poster.pdf)
<br>

<b>Illumination Robust Change Detection with CMOS Imaging Sensors</b><br>
Vijay Rengarajan, Sheetal B. Gupta, A.N. Rajagopalan, and Guna Seetharaman<br>
SPIE Defense + Security Symposium, International Society for Optics and Photonics<br>
Baltimore, Maryland, April 2015<br>
[Paper](../pdf/2015_spie.pdf) [Slides](../pdf/2015_spie_present.pdf)

## Results
Given a reference image and an RS/RSMB image, we estimate the row-wise camera motion between the two images to register them, and simultaneously detect the regions of changes between them.
![alt text](../img/rs_cd_rsmb_eg1.png "Curved road")
![alt text](../img/rs_cd_rsmb_eg2.png "Sheared road")