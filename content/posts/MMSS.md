---
title: "MixedModelsSmallSample.jl: Small Sample Inference for Mixed Models in Julia"
date: 2025-07-18
draft: false
---

Announcing the registration of [MixedModelsSmallSample.jl](https://github.com/ArnoStrouwen/MixedModelsSmallSample.jl) in the Julia General Registry!

<!--more-->

Mixed effect regression models are statistical models that not only contain fixed effects (non-random quantities) but also random effects (random variables). Both of these effects must be estimated from data, and a popular method for this is restricted maximum likelihood (REML).

The Julia programming language already has a state-of-the-art package, [MixedModels.jl](https://juliastats.org/MixedModels.jl/stable/), for estimating these effects using REML. Inference for the effects is, however, based on large sample approximations or on bootstrapping. When working with a small sample, the asymptotic approximation might not hold, and the resampling in the bootstrapping procedure might make the model inestimable.

Alternative small sample inference approaches have been suggested by Kenward and Roger [1] and by Fai and Cornelius [2]. In these methods, the asymptotic results for confidence intervals and hypothesis tests are adjusted to account for finite sample sizes.

MixedModelsSmallSample.jl implements these small sample adjustments for models estimated by MixedModels.jl.

These adjustment methods have already been incorporated in many statistical software programs, such as SAS [3], JMP [4], and lmerTest [5]. However, these packages differ in some statistical details. For example, SAS and JMP use the observed Fisher information matrix for variance components, while lmerTest uses the expected Fisher information matrix.

MixedModelsSmallSample.jl provides user options to configure these technical details, such that results from SAS, JMP, and lmerTest can be exactly reproduced and easily compared.

### References

[1]: Kenward, Michael G., and James H. Roger. “Small sample inference for fixed effects from restricted maximum likelihood.” Biometrics (1997): 983-997.

[2]: Hrong-Tai Fai, Alex, and Paul L. Cornelius. “Approximate F-tests of multiple degree of freedom hypotheses in generalized least squares analyses of unbalanced split-plot experiments.” Journal of statistical computation and simulation 54.4 (1996): 363-378.

[3]: SAS Institute Inc. 2015. SAS/STAT® 14.1 User’s Guide: The MIXED Procedure. Cary, NC: SAS Institute Inc

[4]: JMP Statistical Discovery LLC 2024. JMP ® 18 Fitting Linear Models. Cary, NC: JMP Statistical Discovery LLC

[5]: Kuznetsova, Alexandra, Per B. Brockhoff, and Rune HB Christensen. “lmerTest package: tests in linear mixed effects models.” Journal of statistical software 82 (2017): 1-26.
