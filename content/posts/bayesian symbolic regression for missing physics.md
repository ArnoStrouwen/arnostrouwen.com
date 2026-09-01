---
title: "New Conference Paper: Bayesian Symbolic Regression for Missing Physics"
date: 2026-09-01
draft: false
---

My paper "Bayesian Symbolic Regression for Missing Physics" was accepted at the 2026 IFAC World Congress.

<!--more-->

When part of a dynamic model is unknown, a common approach is to replace the missing terms by a neural network,
fit this universal differential equation to data,
and then translate the neural network into an interpretable equation with symbolic regression.
Symbolic regression is usually done with genetic algorithms.
These return good candidate equations,
but no indication of how much confidence we should place in any of them.

In this paper, the genetic algorithm is replaced by Bayesian symbolic regression,
which samples expression trees from their posterior distribution using reversible jump Markov chain Monte Carlo.
On a Lotka-Volterra system, most chains find the interaction between predator and prey,
written in many different forms.

The paper also follows up on our earlier work on [experimental design for missing physics]({{< ref "experimental design for missing physics" >}}).
For a fed-batch bioreactor, Bayesian symbolic regression recovers the Monod equation from the optimally designed experiment,
but not from an experiment with random inputs.

[Read the paper on arXiv.](https://arxiv.org/abs/2603.14918)
The code is available on [GitHub](https://github.com/arno-papers/IFACWC2026).
