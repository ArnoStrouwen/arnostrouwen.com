---
draft: false
title: Notes on variational Bayesian experimental design
date: 2026-04-01
---
{{< katex >}}
At the end of [Bayesian experimental design]({{< ref "bayesian experimental design" >}}),
we were left with a nested integral.
Variational methods replace the inner integral by an optimization problem.
<!--more-->

The expected information gain (EIG) of a design \\(D\\) is the expected KL divergence from the prior to the posterior:
$$
\text{EIG}(D) = \mathbb E_{p(y,\theta \mid D)}\left [\ln \frac{p(\theta \mid y, D)}{p(\theta)}\right ].
$$
We do not have access to the posterior \\(p(\theta \mid y, D)\\).
Rewriting the EIG to avoid the posterior,
$$
\text{EIG}(D) = \mathbb E_{p(y,\theta \mid D)}\left [\ln p(y \mid \theta, D)\right ] - \mathbb E_{p(y \mid D)}\left [\ln p(y \mid D)\right ],
$$
leaves the marginal likelihood in the second term,
which is an integral over \\(\theta\\) inside the expectation over \\(y\\):
$$
p(y \mid D) = \int p(y \mid \theta, D) p(\theta) d\theta.
$$

### Nested Monte Carlo

The direct approach is nested Monte Carlo.
For each of \\(N\\) outer samples \\(y^{(i)}\\), the marginal likelihood is estimated with an inner loop over \\(M\\) prior samples:
$$
\hat p(y^{(i)} \mid D) = \frac{1}{M}\sum_{j=1}^{M} p(y^{(i)} \mid \theta^{(j)}, D), \qquad \theta^{(j)} \sim p(\theta).
$$
Every outer sample needs its own inner loop,
so a single evaluation of the EIG costs \\(N \times M\\) likelihood evaluations.
This cost is paid again for every design that is tried during the optimization over \\(D\\).

### A lower bound

Instead of computing the posterior, we approximate it with a function \\(q(\theta \mid y)\\) of our own choosing.
We require \\(q\\) to be a density over \\(\theta\\) for every possible measurement \\(y\\):
it must be non-negative and integrate to 1,
$$
q(\theta \mid y) \ge 0, \qquad \int q(\theta \mid y) d\theta = 1.
$$
Multiplying and dividing by \\(q(\theta \mid y)\\) inside the logarithm splits the EIG into two terms:
$$
\text{EIG}(D) = \mathbb E_{p(y,\theta \mid D)}\left [\ln \frac{q(\theta \mid y)}{p(\theta)}\right ] + \mathbb E_{p(y,\theta \mid D)}\left [\ln \frac{p(\theta \mid y, D)}{q(\theta \mid y)}\right ].
$$
The second term is the expected KL divergence from the posterior to \\(q\\), which is never negative.
To see this, factor the joint density as \\(p(y,\theta \mid D) = p(y \mid D) p(\theta \mid y, D)\\),
so that the expectation over the joint becomes an expectation over the measurements of an expectation over the posterior:
$$
\mathbb E_{p(y,\theta \mid D)}\left [\ln \frac{p(\theta \mid y, D)}{q(\theta \mid y)}\right ]
= \mathbb E_{p(y \mid D)}\left [\mathbb E_{p(\theta \mid y, D)}\left [\ln \frac{p(\theta \mid y, D)}{q(\theta \mid y)}\right ]\right ].
$$
It is therefore enough to show that the inner expectation is non-negative for every \\(y\\).
Fix \\(y\\). Jensen's inequality for the concave logarithm shows that the inner expectation with the fraction flipped is at most zero:
$$
\mathbb E_{p(\theta \mid y, D)}\left [\ln \frac{q(\theta \mid y)}{p(\theta \mid y, D)}\right ]
\le \ln \mathbb E_{p(\theta \mid y, D)}\left [\frac{q(\theta \mid y)}{p(\theta \mid y, D)}\right ]
\le \ln \int q(\theta \mid y) d\theta = \ln 1 = 0.
$$
The second inequality is where both requirements on \\(q\\) are used.
The expectation over the posterior only sees the values of \\(\theta\\) where the posterior is positive.
There, the posterior cancels against the denominator:
$$
\mathbb E_{p(\theta \mid y, D)}\left [\frac{q(\theta \mid y)}{p(\theta \mid y, D)}\right ] = \int_{p(\theta \mid y, D) > 0} q(\theta \mid y) d\theta.
$$
Since \\(q\\) is non-negative, any mass it puts where the posterior is zero is lost,
so this integral is at most the full integral of \\(q\\), which is 1.
Both inequalities become equalities when \\(q\\) is the posterior.

Dropping the second term therefore gives a lower bound on the EIG:
$$
\text{EIG}(D) \ge \mathbb E_{p(y,\theta \mid D)}\left [\ln \frac{q(\theta \mid y)}{p(\theta)}\right ].
$$
This is the Barber-Agakov bound.
Without the requirement that \\(q\\) integrates to 1, we could increase the bound without limit by making \\(q\\) larger everywhere.

### Optimizing over q

Since the bound is tight when \\(q\\) is the posterior, the EIG is the maximum of the bound over all possible \\(q\\):
$$
\text{EIG}(D) = \max_{q} \mathbb E_{p(y,\theta \mid D)}\left [\ln \frac{q(\theta \mid y)}{p(\theta)}\right ].
$$
This maximum runs over all non-negative functions with two inputs, \\(\theta\\) and \\(y\\), that integrate to 1 over \\(\theta\\) for every value of \\(y\\).
The maximizer is the posterior \\(p(\theta \mid y, D)\\).

Unlike the EIG itself, the bound is easy to estimate. We only need to:
1. Sample \\(\theta \sim p(\theta)\\) from the prior.
2. Sample \\(y \sim p(y \mid \theta, D)\\) from the likelihood.
3. Evaluate \\(\ln q(\theta \mid y)\\) and \\(\ln p(\theta)\\).

No inner integral is needed, and the likelihood is only sampled from, never evaluated.
So the method can also be used to design experiments when the model is only available as a simulator,
which generates measurements for given parameters and design, but whose likelihood cannot be evaluated.

### Optimizing the design

To find the best design, we also maximize over \\(D\\):
$$
\max_{D} \text{EIG}(D) = \max_{D} \max_{q} \mathbb E_{p(y,\theta \mid D)}\left [\ln \frac{q(\theta \mid y)}{p(\theta)}\right ] = \max_{D, q} \mathbb E_{p(y,\theta \mid D)}\left [\ln \frac{q(\theta \mid y)}{p(\theta)}\right ].
$$
The second equality looks trivial, but it is what makes the variational approach fast.
We do not need to solve the inner maximization over \\(q\\) before taking a gradient ascent step in \\(D\\).
Instead, every stochastic gradient step updates \\(D\\) and \\(q\\) together.
During the optimization, \\(q\\) follows the posterior for the current design,
and at the optimum, it is the posterior for the best design.
In nested Monte Carlo, every new design needs a full inner loop before we know its EIG.

### In practice

We cannot search over all possible densities \\(q\\).
Instead, \\(q\\) is parameterized by a neural network with weights \\(\phi\\), \\(q_\phi(\theta \mid y)\\), which takes the measurement \\(y\\) as an input.
A single network then approximates the posterior for every possible measurement, which is called amortized inference.
The joint gradient steps are taken in \\(D\\) and \\(\phi\\).

With this restriction, the bound is only as tight as \\(q_\phi\\) is accurate.
Variational methods are a solution for speed, not for accuracy.
Nested Monte Carlo can be made more accurate by taking more samples,
while making \\(q_\phi\\) a better approximation of the posterior is more involved.

### Densities from neural networks

We said to replace \\(q\\) by a neural network,
but it is not obvious how to make a neural network satisfy the two requirements on \\(q\\).
Non-negativity is easy, for example by taking the exponential of the output.
Integrating to 1 is harder.
A neural network \\(g_\phi(\theta, y)\\) with a positive output is a function of two inputs,
but there is no reason why it would integrate to 1 over \\(\theta\\) for every \\(y\\).
The obvious fix is to divide by the integral,
$$
q_\phi(\theta \mid y) = \frac{g_\phi(\theta, y)}{\int g_\phi(\tilde \theta, y) d\tilde \theta}.
$$
But this puts an intractable integral over \\(\theta\\) back inside the logarithm, which is the problem we started from.
Normalizing flows are neural networks whose output integrates to 1 by construction.
They are the topic of the next post.

### Addendum: making this work for policies

In [adaptive Bayesian experimental design]({{< ref "adaptive bayesian experimental design" >}})
and [policy equivalence]({{< ref "policy equivalence in adaptive bayesian experimental design" >}}),
we saw that adaptive design comes down to optimizing a policy,
which picks each design based on the measurements gathered so far.
For 3 experiments:
$$
D_1 = \pi_1, \qquad D_2 = \pi_2(D_1, y_1), \qquad D_3 = \pi_3(D_1, y_1, D_2, y_2).
$$
We write \\(\pi\\) and \\(y\\) for all three policies and measurements together, respectively.
For a given policy, the measurements therefore determine the designs.
Conditioning on \\(\pi\\) means using the designs that the policy picks.
The criterion uses the posterior after all three measurements, averaged over experiments run with the policy:
$$
\text{EIG}(\pi) = \mathbb E_{p(y,\theta \mid \pi)}\left [\ln \frac{p(\theta \mid y, \pi)}{p(\theta)}\right ].
$$

Everything from the static case carries over, with the policy in place of the design.
Since the designs are determined by the measurements, \\(q\\) is again a function of two inputs, \\(\theta\\) and \\(y\\),
which must be non-negative and integrate to 1 over \\(\theta\\) for every possible set of measurements.
The bound becomes
$$
\text{EIG}(\pi) \ge \mathbb E_{p(y,\theta \mid \pi)}\left [\ln \frac{q(\theta \mid y)}{p(\theta)}\right ],
$$
and it is tight when \\(q\\) is the posterior with the designs implied by the policy:
$$
q(\theta \mid y) = p(\theta \mid y, \pi).
$$

The maximizations over the policy and over \\(q\\) combine as before,
$$
\max_{\pi} \text{EIG}(\pi) = \max_{\pi, q} \mathbb E_{p(y,\theta \mid \pi)}\left [\ln \frac{q(\theta \mid y)}{p(\theta)}\right ],
$$
so every gradient step updates the policy and \\(q\\) together,
and during the optimization, \\(q\\) follows the posterior with the designs implied by the current policy.
In practice, both are neural networks, trained on experiments simulated from the prior, the likelihood, and the current policy.
