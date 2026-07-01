---
title: "mODa 14 Tutorial: From Optimal Designs to Optimal Policies"
date: 2026-07-01
draft: false
---

At mODa 14, I gave a tutorial on modern Bayesian experimental design.

<!--more-->

At the previous mODa, several speakers argued that adaptive experimental design should be framed as learning a policy.
Since then, the machine learning community has built exactly that.
The tutorial starts from entropy and the expected information gain,
and works towards amortized design policies:
a neural network that maps the measurements gathered so far to the next experiment,
trained once before the experiment starts.
Along the way, it covers variational bounds that remove the inner Monte Carlo loop,
normalizing flows that are trained together with the policy to approximate the posterior,
and diffusion, a newer way to approximate the posterior, as a direction for future work.

Some of this material is covered in more detail in my notes on
[Bayesian experimental design]({{< ref "bayesian experimental design" >}}),
[adaptive Bayesian experimental design]({{< ref "adaptive bayesian experimental design" >}}),
[policy equivalence]({{< ref "policy equivalence in adaptive bayesian experimental design" >}}),
and [variational Bayesian experimental design]({{< ref "variational bayesian experimental design" >}}).

The [slides](https://github.com/arno-papers/moda14/blob/main/slides.pdf) and the [source to generate them](https://github.com/arno-papers/moda14) are on GitHub.
