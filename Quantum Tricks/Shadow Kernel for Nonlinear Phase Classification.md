# Shadow Kernel for Nonlinear Phase Classification

> **Source:** Huang, Kueng, Torlai, Albert, Preskill, arXiv:2106.12627
> **Tags:** #trick #classical-shadows #kernel-methods #phases-of-matter #topology

## What it does

Builds an efficiently computable kernel on classical shadows that can learn nonlinear functions of few-body reduced density matrices, which is exactly what linear order parameters miss for topological and SPT phase classification.

## The trick

A single observable $\operatorname{tr}(O\rho)$ cannot generally separate topological phases. So instead of searching for a linear order parameter in state space, lift each classical shadow into an implicit feature space containing:

- reduced density matrices on bounded-size subsystems,
- polynomial features of all degrees.

The paper implements this through the kernel

$$
k_{\mathrm{shadow}}(S_T(\rho),S_T(\tilde\rho))
=
\exp\left(
\frac{\tau}{T^2}
\sum_{t,t'=1}^{T}
\exp\left(\frac{\gamma}{n}\sum_{i=1}^{n}\operatorname{tr}(\sigma_i^{(t)}\tilde\sigma_i^{(t')})\right)
\right).
$$

This is expressive enough to represent analytic functions of constant-size marginals, while still being computable from the stored shadow data.

## When to reach for it

- Linear observables fail to separate the phases you care about.
- You suspect the classifier lives in local entanglement structure rather than a simple order parameter.
- You have classical-shadow data from experiment or simulation and want a kernel method with interpretable geometry.

## Complexity

Kernel evaluation costs $O(nT^2)$ per pair of states, where $n$ is system size and $T$ is the number of shadow shots per state.

## Caveat

The guarantee is conditional on the phase being recognisable by a nonlinear function of constant-size marginals. Near a phase boundary, the required subsystem size can grow with the correlation length, so the method becomes less friendly.

## Related notes

- [[Provably efficient machine learning for quantum many-body problems (Huang-Kueng-Torlai-Albert-Preskill 2022) — Paper Notes]]
- [[Projected Quantum Kernel via Local Observables]]
- [[Predicting Many Properties from Very Few Measurements (Huang-Kueng-Preskill 2020) — Paper Notes]]
