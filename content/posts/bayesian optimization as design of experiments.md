---
title: "ENBIS-26 Talk: Bayesian Optimization as Design of Experiments"
date: 2026-10-01
draft: false
---

At ENBIS-26 in Florence, I gave a talk on how Bayesian optimization fits into the design of experiments.

<!--more-->

The talk grew out of a [lecture on Bayesian optimization](https://github.com/arno-papers/bayesian-optimization-doe/blob/main/lecture/slides.pdf) that I added to the [Experimental Design course](https://lstat.kuleuven.be/training/coursedescriptions/experimental_designnew) at KU Leuven.
Classical design of experiments and Bayesian optimization are usually taught as two fixed recipes:
a low-order polynomial with D- or I-optimal designs,
versus a Gaussian process with an acquisition function, adding one run at a time.
Both recipes are built from the same three choices: a model, a target, and a rule for spending the experimental budget.
These choices can be recombined freely.
For example, a quadratic model can be used to locate an optimum, by designing for the precision of its stationary point.

Bayesian experimental design chooses runs to reduce the entropy of any function of the model parameters.
When that function is the location of the optimum, we get Bayesian optimization, in the form of entropy search.
Bayesian optimization is then a special case of the Bayesian experimental design from my [mODa 14 tutorial]({{< ref "moda14 tutorial" >}}),
so the adaptive design policies from that tutorial apply to it as well.
From this point of view, the Gaussian process is a prior, and it should be defended like any other prior.
A stationary Gaussian process is a reasonable prior for ore concentrations, which is where kriging comes from,
but less obviously for the yield of a manufacturing process.
Universal kriging combines the two:
the second-order model goes into the mean function,
and the Gaussian process only has to explain what the polynomial misses.

The [slides](https://github.com/arno-papers/bayesian-optimization-doe/blob/main/enbis26/slides.pdf) and the [source to generate them](https://github.com/arno-papers/bayesian-optimization-doe/tree/main/enbis26) are on GitHub.
