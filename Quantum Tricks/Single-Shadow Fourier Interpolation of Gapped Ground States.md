# Single-Shadow Fourier Interpolation of Gapped Ground States

> **Source:** Huang, Kueng, Torlai, Albert, Preskill, arXiv:2106.12627
> **Tags:** #trick #classical-shadows #many-body #interpolation #fourier

## What it does

Predicts local ground-state properties across a smooth gapped Hamiltonian family using only one classical-shadow snapshot per training point, plus a low-frequency interpolation kernel over parameter space.

## The trick

Suppose a Hamiltonian family $H(x)$ has gapped local ground states $\rho(x)$ and you care about a local observable sum $O$. Define

$$
f_O(x)=\operatorname{tr}(O\rho(x)).
$$

For gapped local systems, quasi-adiabatic continuation and Lieb--Robinson bounds imply that $f_O(x)$ is smooth in $x$ on average. Expand it in Fourier modes and truncate to frequencies $\lVert k\rVert_2\le \Lambda$.

Then learn the truncated expansion from training data with predictor

$$
\hat\sigma_N(x)=\frac1N\sum_{\ell=1}^{N}\kappa(x,x_\ell)\,\sigma_1(\rho(x_\ell)),
$$

using the $\ell_2$-Dirichlet kernel

$$
\kappa(x,x_\ell)=\sum_{\lVert k\rVert_2\le \Lambda}\cos\big(\pi k\cdot(x-x_\ell)\big).
$$

The remarkable part is that the label at each training point can be just one shadow snapshot rather than a high-precision tomography record.

## When to reach for it

- You have a parameterized Hamiltonian family staying inside one gapped phase.
- You want average-case prediction across parameter space rather than worst-case certification at every point.
- The observables of interest are local or sums of local terms.
- Training data is expensive, so one-shot labels matter.

## Complexity

The formal guarantee needs training size polynomial in the parameter dimension $m$ for fixed error, with runtime linear in system size $n$ and polynomial in the learned Fourier cutoff.

## Caveat

The precision dependence is bad: without more structure, the sample complexity grows very quickly as $\varepsilon\to 0$. The method is an interpolation theorem inside a smooth phase, not a universal solver for hard many-body ground states.

## Related notes

- [[Provably efficient machine learning for quantum many-body problems (Huang-Kueng-Torlai-Albert-Preskill 2022) — Paper Notes]]
- [[Predicting Many Properties from Very Few Measurements (Huang-Kueng-Preskill 2020) — Paper Notes]]
- [[Power of Data in Quantum Machine Learning (Huang, Babbush, McClean et al 2021) — Paper Notes]]
