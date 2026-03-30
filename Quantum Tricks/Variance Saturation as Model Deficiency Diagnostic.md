# Variance Saturation as Model Deficiency Diagnostic

> **Source:** Wang, Paesani, Santagati et al., arXiv:1703.05402 (2017); theoretical basis in Wiebe, Granade, Ferrie, Cory, arXiv:1311.5269 (2014)
> **Tags:** #trick #Bayesian-inference #model-selection #hamiltonian-learning #diagnostics

## What it does
Detects when a parametric model is insufficient to describe the true dynamics of a quantum system, by monitoring the posterior variance during Bayesian learning.

## The trick

In [[Sequential Monte Carlo for Quantum Parameter Estimation|SMC]]-based Bayesian parameter estimation, the posterior variance $\sigma^2$ (or covariance norm $\|\Sigma\|_2$ for multiparameter models) decreases exponentially with experiment number when the model is correct:

$$\sigma^2 \approx A\, e^{-\gamma N_{\mathrm{steps}}}.$$

When the model is **wrong** — i.e., the true system has dynamics not capturable by the parametric family — the variance stops decreasing and saturates at a floor determined by $\|H_{\mathrm{true}} - H(\mathbf{x}^*)\|$ where $\mathbf{x}^*$ is the closest model parameter.

The diagnostic:
1. Track $\sigma^2$ (or $\|\Sigma\|_2$) as experiments accumulate.
2. If the decay plateaus while the model should still have room to improve, the model is deficient.
3. Introduce additional parameters (expand the Hamiltonian family) and compare models via the [[Bayesian Model Selection via SMC Marginal Likelihoods|Bayes factor]] — the ratio of marginal likelihoods, available directly from the [[Sequential Monte Carlo for Quantum Parameter Estimation|SMC]] normalisation constants.

In the [[Experimental Quantum Hamiltonian Learning (Wang, Paesani, Santagati et al 2017) — Paper Notes|Wang et al. (2017) experiment]]: a single-parameter Rabi model saturated at $\sigma^2 \approx 4.2 \times 10^{-5}$ after 35 steps. Adding a chirping parameter let the covariance continue decreasing to $7.5 \times 10^{-6}$, with Bayes factor $K = 560$ favouring the expanded model.

## When to reach for it

- Any Bayesian system identification / Hamiltonian learning scenario where you want to know if your model is good enough.
- Characterising quantum devices where the Hamiltonian isn't precisely known a priori (drift, crosstalk, calibration errors).
- Deciding when to move from a simple model to a more complex one — the saturation tells you the simple model has been maxed out, and the Bayes factor tells you whether the complexity is justified.

## Complexity

Zero additional cost beyond what you're already computing in [[Sequential Monte Carlo for Quantum Parameter Estimation|SMC]]: the variance and marginal likelihood are standard outputs of the particle filter.

## Caveat

- The saturation level depends on the distance between the true Hamiltonian and the nearest model in your family. If the model is only slightly wrong, the saturation floor may be hard to distinguish from statistical noise or slow convergence.
- Requires enough experiments to establish the exponential decay phase before you can detect saturation. For high-dimensional parameter spaces, this warm-up period grows.
- The Bayes factor favours the simpler model when data is scarce (Occam's razor effect). You need sufficient data for the expanded model's improved fit to outweigh its complexity penalty.

## Related notes
- [[Experimental Quantum Hamiltonian Learning (Wang, Paesani, Santagati et al 2017) — Paper Notes]] — First experimental demonstration of this diagnostic
- [[Quantum Hamiltonian Learning Using Imperfect Quantum Resources (Wiebe-Granade-Ferrie-Cory 2014) — Paper Notes]] — Theoretical analysis of model mismatch and saturation behaviour
- [[Bayesian Model Selection via SMC Marginal Likelihoods]] — The Bayes factor computation that complements variance monitoring
- [[Sequential Monte Carlo for Quantum Parameter Estimation]] — The inference framework providing the variance estimates
