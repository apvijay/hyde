---
layout: page
title: Global Shutter Motion Blur Registration
---

## Abstract
We address the challenging problem of registration and change
detection in very large motion blurred images. The unreasonable demand
that this task puts on computational and memory resources precludes
the possibility of any direct attempt at solving this problem. We
address this issue by observing the fact that the camera motion
experienced by a sufficiently large sub-image is approximately the
same as that of the entire image itself. We devise an algorithm for
judicious sub-image selection so that the camera motion can be
deciphered correctly, irrespective of the presence or absence of
occluder. We follow a reblur-difference framework to detect changes as
this is an artifact-free pipeline unlike the traditional
deblur-difference approach. We demonstrate the results of our
algorithm on both synthetic and real data.

## Publication
Efficient Change Detection for Very Large Motion Blurred Images<br>
Vijay Rengarajan, Abhijith Punnappurath, A.N. Rajagopalan, and Guna Seetharaman<br>
CVPR Workshop on Registration of Very Large Images<br>
Columbus, Ohio, USA, June 2014<br>
[Paper](../pdf/2014_cvprw.pdf) [Slides](../pdf/2014_cvprw_present.pdf)

## Results
Given a reference image and a motion blur image
having very large spatial dimensions, we estimate the camera poses
that contribute to motion blur between the two images only from
sub-images of smaller size, and simultaneously detect the regions of
changes between them.
![alt text](../img/gs_mb_vlarge_eg1.png "Motion blur registration")
