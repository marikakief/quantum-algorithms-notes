# Independent Batch Splitting for Hamiltonian-Guiding State Decoupling

> **Source:** Schmidhuber, O'Donnell, Kothari, Babbush, arXiv:2406.19378
> **Tags:** #trick #decoupling #second-moment-method #independence #planted-inference

## What it does

Decouples a Hamiltonian from its guiding state when both are constructed from the same random input, enabling a clean second-moment proof that the guiding state overlaps the target eigenspace.

## The trick

When both the Kikuchi Hamiltonian $\mathcal{K}_\ell$ and the guiding state $|\Psi\rangle$ are built from the same planted instance $\hat{\mathcal{I}}$, their correlation prevents straightforward composition of two facts: (a) the planted solution has large overlap with the cutoff eigenspace, and (b) the guiding state has large overlap with the planted solution.

**Fix:** Randomly partition the input constraints into two independent batches:
- $\mathcal{I}$: fraction $1 - \zeta$ of constraints → build the Kikuchi Hamiltonian
- $\mathcal{I}_{\text{guide}}$: fraction $\zeta$ of constraints → build the guiding state

By Poisson splitting, these are independent draws from the planted distribution. The cost:

- **Hamiltonian quality:** Losing a $\zeta$-fraction of constraints barely affects the Alice Theorem (the eigenvalue gap changes by $O(\zeta)$, which is negligible for $\zeta = 1/\ln n$).
- **Guiding state overlap:** Reduced by a factor $\zeta^{\ell/k}$. Since $\zeta = 1/\ln n$ and this enters multiplicatively (not in the exponent of $n$), it's absorbed into the $\text{poly}(n)$ factor.

Once independence is established, fix any "good" outcome of $\mathcal{I}$ (where the Alice Theorem holds), then the guiding state is still random conditioned on the planted solution. A standard Chebyshev/second-moment argument (Theorem 38 in the paper) then proves the overlap bound.

## When to reach for it

- Any setting where you need to prove that a data-derived quantum state overlaps the eigenspace of a data-derived Hamiltonian — and both objects share the same randomness.
- Classical analogue: sample splitting in statistics (train/test splits, cross-validation). The quantum version has the same flavor but the stakes are different — you need to prove a spectral overlap bound, not just estimate a prediction error.

## Complexity

No additional computational cost — the splitting is purely classical preprocessing ($O(m)$ coin flips). The only price is a mild reduction in signal quality for both the Hamiltonian and the guiding state.

## Caveat

The choice of $\zeta$ involves a tradeoff: too large, and the Hamiltonian loses signal (forces larger $\ell$, exponentially more expensive); too small, and the guiding state loses overlap. The sweet spot $\zeta = 1/\ln n$ attributes most signal to the Hamiltonian and an $o(1)$-fraction to the guiding state.

This trick is simpler than Hastings's approach for Tensor PCA, which instead added Gaussian noise to create multiple partially independent guiding states and used Carbery-Wright anticoncentration to guarantee that at least one had good overlap.

## Related notes
- [[Quartic Quantum Speedups for Planted Inference (Schmidhuber, O'Donnell, Kothari, Babbush 2024) — Paper Notes]]
- [[Input-Derived Guiding State for Planted Problems]]
- [[Kikuchi Method for Degree Reduction]]
- [[Guided Sparse Hamiltonian Framework]]
