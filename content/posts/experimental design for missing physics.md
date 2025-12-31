---
draft: false
title: "New Conference Paper: Experimental Design for Missing Physics"
date: 2025-12-30
---

I'm excited to announce that Sebastian Micluța-Câmpeanu and I have published [**"Experimental Design for Missing Physics"**](https://www.sciencedirect.com/science/article/pii/S240589632500552X) at DYCOPS 2025.

<!--more-->

When building models of real-world systems like bioreactors, we often don't know the complete physics governing the process. This "missing physics" needs to be discovered from experimental data. Modern machine learning offers promising tools for this: neural networks can flexibly represent unknown dynamics, while symbolic regression can translate these black-box models into interpretable equations.

However, these techniques are data-hungry—they need high-quality measurements to reliably uncover the true underlying physics. Random experiments won't cut it.

In this paper, we tackle this challenge by developing a **sequential experimental design technique**. The idea is to intelligently choose which experiments to run next based on the candidate model structures that symbolic regression suggests. By designing experiments that best discriminate between competing hypotheses, we can efficiently guide the discovery process toward the correct physics. We demonstrate our approach on a bioreactor system, showing how smart experimentation accelerates scientific discovery.
