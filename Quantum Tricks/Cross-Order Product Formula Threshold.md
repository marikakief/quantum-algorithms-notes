# Cross-Order Product Formula Threshold

> **Source:** Morales, Costa, Pantaleoni, Burgarth, Sanders, Berry, arXiv:2210.15817 (2025)
> **Tags:** #trick #product-formulas #complexity-comparison #hamiltonian-simulation

## What it does

Gives a principled method to determine at what simulation parameters ($T/\varepsilon$) a higher-order product formula becomes preferable to a lower-order one.

## The trick

For two product formulas of orders $k_1 < k_2$ with eigenvalue error constants $\zeta_1, \zeta_2$ and $M_1, M_2$ stages, equate their total exponential count:

$$
\frac{T}{\varepsilon} = \left(\frac{M_2 \zeta_2^{1/k_2}}{M_1 \zeta_1^{1/k_1}}\right)^{\frac{1}{1/k_1 - 1/k_2}}
$$

Above this threshold, order $k_2$ is cheaper. Below it, order $k_1$ wins.

**Non-asymptotic correction:** The asymptotic formula assumes the time step is small enough for $\delta \propto t^{k+1}$ to hold. When the threshold corresponds to a large time step, use the actual error function $f(t)$ numerically:
1. Solve $f_1(t_1) = f_2(t_1 M_2/M_1) \cdot M_1/M_2$ for $t_1$
2. Compute $T/\varepsilon = t_1/f_1(t_1)$

This can shift the threshold by orders of magnitude. The asymptotic 6th→8th threshold is ~68,000; the non-asymptotic threshold is ~$3.2 \times 10^7$.

**For fermionic Hamiltonians**, the threshold is in terms of:
$$
\frac{\|\tau\|_1 \|\nu\|_{1,[\eta]} \eta}{\|\tau\|_1 + \|\nu\|_{1,[\eta]}} \cdot \frac{T}{\varepsilon}
$$
where the prefactor captures the structure of the electron Hamiltonian. For eigenvalue estimation, replace $T/\varepsilon$ with $1/\varepsilon_H$.

## When to reach for it

- Deciding which order of [[Order-Condition Cancellation in Product Formulas|product formula]] to implement for a specific quantum chemistry or condensed matter simulation.
- Resource estimation: the order choice affects total gate count by large constant factors.
- Sanity-checking whether 10th-order formulas are worth implementing (they almost never are: the 8th→10th threshold is $\sim 10^{16}$).

## Complexity

The threshold computation itself is trivial (one equation to solve). The expensive part is accurately determining $\zeta$ for each candidate formula, which requires numerical testing on representative Hamiltonians.

## Caveat

The threshold depends on the class of Hamiltonians. Random matrices, fermionic chemistry, lattice models, and spin chains can give different thresholds for the same pair of formulas. Always test on representative instances. Also, the non-asymptotic correction requires computing $f(t)$ beyond the scaling regime — at large time steps the error function may have complicated structure.

## Related notes
- [[Selection and Improvement of Product Formulae for Best Performance of Quantum Simulation (Morales-Costa-Pantaleoni-Burgarth-Sanders-Berry 2025) — Paper Notes]]
- [[Eigenvalue Error as the Correct Product Formula Metric]]
- [[Suzuki Order as a Tunable Knob for Time Scaling]]
- [[Processed Product Formula Kernel-Processor Decomposition]]
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]]
