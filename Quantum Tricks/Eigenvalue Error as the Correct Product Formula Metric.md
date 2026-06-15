
> **Source:** Morales, Costa, Pantaleoni, Burgarth, Sanders, Berry, arXiv:2210.15817 (2025)
> **Tags:** #trick #product-formulas #error-analysis #hamiltonian-simulation

## What it does

Shows that for repeated-step long-time simulation with [[Order-Condition Cancellation in Product Formulas|product formulas]], the eigenvalue error per step can be the sharper metric than the one-step spectral-norm error.

## The trick

For a fixed product-formula step that is reused $r$ times, decompose the approximate evolution operator $\tilde{U} = VDV^\dagger$ in the Hamiltonian eigenbasis. Here $D$ captures eigenvalue error (diagonal) and $V$ captures basis error (rotation of eigenvectors). Over $r$ repeated steps:

$$
\|\tilde{U}^r - U^r\| \leq 2\|V - I\| + r\|D - U\|
$$

The eigenvalue error $\|D - U\|$ accumulates linearly with $r$. The basis error $\|V - I\|$ does not — under the fixed-step diagonalization assumptions, it enters only at the boundary and is bounded independent of $r$ (up to a factor $\sim 1/t$ from commutation with $D-I$).

For a $k$-th order formula with eigenvalue error $\zeta t^{k+1}$ and basis error $\mu t^{k+\nu}$ ($\nu \in [0,1]$), the ratio of basis-to-eigenvalue contribution to total error scales as:

$$
\frac{2\mu}{\zeta T} \left(\frac{\varepsilon}{\zeta T}\right)^{\nu/k}
$$

This vanishes for large $T$. So eigenvalue error dominates precisely when it matters most — large-scale simulation.

## When to reach for it

- Selecting [[Product Formulas]]s for long-time [[Hamiltonian simulation]] or [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes|phase estimation]]: optimize for eigenvalue error $\zeta$, not spectral-norm error $\chi$.
- Evaluating whether [[qDRIFT Randomized Hamiltonian Simulation (Campbell 2018) — Paper Notes|randomised product formulas]] actually help in repeated-step use: if successive steps are not identical, the same basis-error cancellation argument no longer directly applies. This is a limitation of the metric argument, not a complete no-go theorem for randomization.
- Understanding why some product formulas with larger spectral-norm error outperform those with smaller spectral-norm error in practice.

## Complexity

No additional computational cost — this is a change of metric, not algorithm. But it changes which [[Product Formulas]] you should choose by up to two orders of magnitude in error.

## Caveat

If the application genuinely requires accurate state reconstruction (not just eigenvalue or phase estimation), then spectral-norm error remains the safe worst-case metric. Also, the eigenvalue-error argument assumes a fixed approximate step with a stable nearby eigenbasis; if degeneracies or changing randomized steps invalidate that picture, the basis-error cancellation bound should not be used blindly. The eigenvalue error of a single step must usually be computed numerically for candidate formulas — there is no simple closed-form expression for $\zeta$ in terms of the formula coefficients.

## Related notes
- [[Selection and Improvement of Product Formulae for Best Performance of Quantum Simulation (Morales-Costa-Pantaleoni-Burgarth-Sanders-Berry 2025) — Paper Notes]]
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]]
- [[Trotter Commutator-Scaling Bound]]
- [[Processed Product Formula Kernel-Processor Decomposition]]
- [[Cross-Order Product Formula Threshold]]
