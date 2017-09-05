---
layout: page
title: Learning-based Rolling Shutter Rectification
---

## Abstract
Row-wise exposure delay present in CMOS cameras is responsible for skew and curvature distortions known as the rolling shutter (RS) effect while imaging under camera motion. Existing RS correction methods resort to using multiple images or tailor scene-specific correction schemes. We propose a convolutional neural network (CNN) architecture that automatically learns essential scene features from a single RS image to estimate the row-wise camera motion and undo RS distortions back to the time of first-row exposure. We employ long rectangular kernels to specifically learn the effects produced by the row-wise exposure. Experiments reveal that our proposed architecture performs better than the conventional CNN employing square kernels. Our single-image correction method fares well even operating in a frame-by-frame manner against video-based methods and performs better than scene-specific correction schemes even under challenging situations.

## Publication
Unrolling the Shutter: CNN to Correct Motion Distortions<br>
Vijay Rengarajan, Yogesh Balaji, A.N. Rajagopalan<br>
Computer Vision and Pattern Recognition (CVPR)<br>
Honolulu, July 2017<br>
[Paper](../pdf/2017_cvpr.pdf) [Supplementary](../pdf/2017_cvpr_supp.pdf) [Poster](../pdf/2017_cvpr_poster.pdf)

## Code
[GitHub](https://github.com/yogeshbalaji/CVPR17_Unrolling_the_shutter)

## Data
[Building Training Dataset](http://www.ee.iitm.ac.in/~ee11d035/building.tar.gz)

[Face Training Dataset](http://www.ee.iitm.ac.in/~ee11d035/face.tar.gz)

[Test Dataset (Building, Face, Synthetic Motion)](http://www.ee.iitm.ac.in/~ee11d035/cvpr17_test_dataset.zip)

## Results
Given a single rolling shutter distorted image, our RowColCNN maps it to camera motion which is used to rectify the distortions.
![alt text](../img/rs_rect_cnn_eg.png "Examples")
