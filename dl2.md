---
layout: page
title: Paper Notes - Deep Learning II
mathjax: true
---
Paper Notes - Deep Learning II

# GoogLeNet
## Inception v1
https://www.cs.unc.edu/~wliu/papers/GoogLeNet.pdf
## Incepton V2, V3
https://www.robots.ox.ac.uk/~vgg/rg/papers/reinception.pdf [Batch Normalization]
## Inception V4
https://arxiv.org/pdf/1602.07261.pdf [Inceptionv3 + ResNet]

# Residual Network
https://arxiv.org/pdf/1512.03385v1.pdf

# Arch improvements
## Network in Network 
1*1 convolution + Average Pooling - introduced first
## Highway networks
gated flow of information
## The all convolutional net (No pooling)
Striving for simplicity

    
# Activations based on local competetion 

## Maxout Networks
http://www.jmlr.org/proceedings/papers/v28/goodfellow13.pdf

## Additional: Compete to compute

## Skip connections
Deeply-supervised nets - http://www.jmlr.org/proceedings/papers/v38/lee15a.pdf

## FitNets
https://arxiv.org/abs/1412.6550
Using shallow networks to teach deep networks 

# Optimization approaches that help go deeper

## Training deep and recurrent networks with hessian-free optimisation
http://www.cs.toronto.edu/~jmartens/docs/HF_book_chapter.pdf

## On the importance of intialization and momentum in deep learning
http://www.cs.toronto.edu/~hinton/absps/momentum.pdf

## Identifying and tackling the saddle point problem in high-dimensional non-convex optimisation
https://papers.nips.cc/paper/5486-identifying-and-attacking-the-saddle-point-problem-in-high-dimensional-non-convex-optimization.pdf

# Weight initialisation strategies
## Xavier Initialization
http://jmlr.org/proceedings/papers/v9/glorot10a/glorot10a.pdf
## Prelu Intialization
https://arxiv.org/pdf/1502.01852v1.pdf

# Other interesting papers:-
## Random walk initialisation for training very deep feedforward networks
## Exact solutions to the nonlinear dynamics of learning in deep linear neural networks