# Eigenvalue Error as the Correct Product Formula Metric

> **Source:** Morales, Costa, Pantaleoni, Burgarth, Sanders, Berry, arXiv:2210.15817 (2025)
> **Tags:** #trick #product-formulas #error-analysis #hamiltonian-simulation

## What it does

Shows that for long-time simulation with [[Order-Condition Cancellation in Product Formulas|product formulas]], the eigenvalue error per step — not the spectral-norm error — determines the total simulation error.

## The trick

Decompose the approximate evolution operator $\tilde{U} = VDV^\dagger$ in the Hamiltonian eigenbasis. Here $D$ captures eigenvalue error (diagonal) and $V$ captures basis error (rotation of eigenvectors). Over $r$ repeated steps:

$$
\|\tilde{U}^r - U^r\| \leq 2\|V - I\| + r\|D - U\|
$$

The eigenvalue error $\|D - U\|$ accumulates linearly with $r$. The basis error $\|V - I\|$ does not — it enters only at the boundary and is bounded independent of $r$ (up to a factor $\sim 1/t$ from commutation with $D-I$).

For a $k$-th order formula with eigenvalue error $\zeta t^{k+1}$ and basis error $\mu t^{k+\nu}$ ($\nu \in [0,1]$), the ratio of basis-to-eigenvalue contribution to total error scales as:

$$
\frac{2\mu}{\zeta T} \left(\frac{\varepsilon}{\zeta T}\right)^{\nu/k}
$$

This vanishes for large $T$. So eigenvalue error dominates precisely when it matters most — large-scale simulation.

## When to reach for it

- Selecting product formulas for long-time Hamiltonian simulation or [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes|phase estimation]]: optimize for eigenvalue error $\zeta$, not spectral-norm error $\chi$.
- Evaluating whether [[qDRIFT Randomized Hamiltonian Simulation (Campbell 2018) — Paper Notes|randomised product formulas]] actually help: if successive steps aren't identical, basis error cancellation breaks. Randomisation may improve spectral-norm error per step but not eigenvalue error, limiting the total improvement.
- Understanding why some product formulas with larger spectral-norm error outperform those with smaller spectral-norm error in practice.

## Complexity

No additional computational cost — this is a change of metric, not algorithm. But it changes which product formula you should choose by up to two orders of magnitude in error.

## Caveat

If the application genuinely requires accurate state reconstruction (not just eigenvalue estimation), then spectral-norm error matters and the argument weakens. Also, the eigenvalue error of a single step must be computed numerically for candidate formulas — there's no closed-form expression for $\zeta$ in terms of the formula coefficients.

## Related notes
- [[Selection and Improvement of Product Formulae for Best Performance of Quantum Simulation (Morales-Costa-Pantaleoni-Burgarth-Sanders-Berry 2025) — Paper Notes]]
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]]
- [[Trotter Commutator-Scaling Bound]]
- [[Processed Product Formula Kernel-Processor Decomposition]]
- [[Cross-Order Product Formula Threshold]]
