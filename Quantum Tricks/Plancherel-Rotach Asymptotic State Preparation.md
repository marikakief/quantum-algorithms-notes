
> **Tags:** #trick #state-preparation #asymptotics #hermite
> **Source:** arXiv:2510.04929 (Quantum Hermite Transform)

## What it does
Prepares high-quality approximate Hermite eigenstates efficiently using asymptotic formulas, then refines them with filtering/amplification.

## The trick
Use the Plancherel-Rotach asymptotic form of Hermite functions in the oscillatory region ($|x| < \sqrt{2n}$, the classically allowed region):
$$\psi_n(x) \approx A_n(x) \cos(\Phi_n(x) - \pi/4)$$
where amplitude $A_n(x) = (n!)^{-1/2}2^{-n/2}(\pi)^{-1/4}(1 - x^2/(2n))^{-1/4}$ and phase $\Phi_n(x)$ are smooth functions computable in polylog gates.

This gives a cheaply preparable proxy state $|\phi_n\rangle$ with **constant** (i.e., $n$-independent) overlap with the true Hermite state $|\psi_n\rangle$. The constant overlap is the key property needed for amplitude amplification to succeed uniformly over all $n$. Then:
- eigenstate filtering (via fast QPE on $e^{-i\bar{H}t}$, using [[QHO Fast-Forwarding via Lie Algebra|QHO fast-forwarding]]) removes off-target components
- fixed-point amplitude amplification boosts fidelity to $1-\varepsilon$

## When to reach for it
- Orthogonal polynomial eigenbases (Hermite, possibly Laguerre/Jacobi analogues)
- Need scalable state prep for large quantum numbers
- Exact recursion-based prep is too costly

## Complexity
Proxy state prep uses coherent arithmetic in polylog dimension; in QHT overall contributes $O((\log N + \log(1/\varepsilon))^3)$.

## Caveat
Asymptotics are valid only in the right regime (oscillatory region, sufficiently large $n$). Need boundary handling/cutoffs.

## Related Paper Notes

- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes]]
