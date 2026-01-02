---
title: "New paper about Experimental Design using the Kalman Filter"
date: 2023-03-31
draft: false
---

I am happy to announce the publication of our paper "Adaptive and robust experimental design for linear dynamical models using Kalman filter" in *Statistical Papers*.


Current experimental design techniques for dynamical systems often only incorporate measurement noise, while dynamical systems also involve process noise.
<!--more-->
To construct experimental designs we need to quantify their information content. The Fisher information matrix is a popular tool to do so. Calculating the Fisher information matrix for linear dynamical systems with both process and measurement noise involves estimating the uncertain dynamical states using a Kalman filter. The Fisher information matrix, however, depends on the true but unknown model parameters. In this paper we combine two methods to solve this issue and develop a robust experimental design methodology. First, Bayesian experimental design averages the Fisher information matrix over a prior distribution of possible model parameter values. Second, adaptive experimental design allows for this information to be updated as measurements are being gathered. This updated information is then used to adapt the remainder of the design.

[Read the full article here.](https://link.springer.com/article/10.1007/s00362-023-01438-9)
