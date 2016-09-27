---
layout: page
title: Paper Notes - Deep Learning I
mathjax: true
---
Paper Notes - Deep Learning I

A Neural Approach to Blind Motion Deblurring
Ayan Chakrabarti
[Paper](http://arxiv.org/abs/1603.04771)

- The output of the network is the Fourier coefficient tensor of a deconvolution filter that can be applied to the input patch to create a sharp patch.
- The image is divided into overlapping patches and each patch is processed independently by the network.

Even though the network output is the complex deconvolution filter Fourier cofficient tensor, the backpropagation starts from the MSE of the image intensities.\\(e=mc^2\\)
$$ y = Ax$$