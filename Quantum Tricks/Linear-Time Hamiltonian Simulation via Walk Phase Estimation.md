# Linear-Time Hamiltonian Simulation via Walk Phase Estimation

> **Source:** Childs, arXiv:0810.0312 (2010), §5, Theorem 5
> **Tags:** #trick #Hamiltonian-simulation #quantum-walk #phase-estimation #non-sparse

## What it does

Simulates $e^{-iHt}$ using $O(\|\mathrm{abs}(H)\| t / \sqrt{\delta})$ steps of a Szegedy-quantized walk — **linear in $t$**, optimal by the no-fast-forwarding theorem. Works for **non-sparse** Hamiltonians.

## The procedure

Given input $|\psi\rangle = \sum_\lambda \psi_\lambda |\lambda\rangle$:

1. Apply isometry $T$: $|\psi\rangle \to T|\psi\rangle \in \mathbb{C}^N \otimes \mathbb{C}^N$
2. Phase estimate on walk $U$ to extract eigenphases $\phi \approx \arcsin\lambda$
3. Apply phase $e^{-it\sin\phi}$ to the eigenvalue register (since $\sin(\arcsin\lambda) = \lambda$, this implements $e^{-i\lambda t}$)
4. Uncompute phase estimation
5. Apply $T^\dagger$

Result: $|\psi\rangle \mapsto \sum_\lambda \psi_\lambda e^{-i\lambda \|\mathrm{abs}(H)\| t} |\lambda\rangle \approx e^{-iHt}|\psi\rangle$.

## Why it's linear in $t$

Phase estimation with $M$ walk steps estimates eigenphases to accuracy $O(1/M)$. The error in $e^{-it\sin\phi}$ vs $e^{-it\lambda}$ is $O(t/M)$. Setting $M = O(\|\mathrm{abs}(H)\| t / \sqrt{\delta})$ gives fidelity $\geq 1 - \delta$.

Contrast with the direct Trotter limit (Theorem 2): $O((\|H\|t)^{3/2}/\sqrt{\delta})$ — worse by a factor of $\sqrt{t}$.

## Optimal phase estimation

Using a **sine window** initial state (not uniform superposition):

$$
\sqrt{\frac{2}{M+1}} \sum_{x=0}^{M-1} \sin\frac{\pi(x+1)}{M+1} |x\rangle
$$

minimises the variance of the phase estimate, achieving:

$$
\sum_\phi (\theta - \phi)^2 |a_{\phi|\theta}|^2 \leq \frac{186}{M^2}
$$

This is optimal among all phase estimation strategies with $M$ queries.

## When to reach for it

- Simulating non-sparse Hamiltonians (adjacency matrices of dense graphs)
- When Trotter-based methods are too expensive (high-order product formulas have worse constant factors)
- As a conceptual stepping stone to [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes|qubitization]] (which replaces phase estimation with QSP phase factors)

## Complexity

$O(\|\mathrm{abs}(H)\| t / \sqrt{\delta})$ walk steps. Each walk step costs $O(\text{poly}(d, \log N))$ gates where $d$ = max degree.

**Optimal in $t$**: linear scaling matches the no-fast-forwarding lower bound.

## Caveat

The cost depends on $\|\mathrm{abs}(H)\|$, not $\|H\|$. For Hamiltonians with many cancelling signs, this can be polynomially or even exponentially worse than the spectral norm. This is the **sign problem** of walk-based simulation.

The phase estimation approach introduces a $1/\sqrt{\delta}$ dependence on error, which is suboptimal. [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes|Qubitization]] and [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|QSVT]] improve the error dependence to $\log(1/\delta)$ by replacing phase estimation with quantum signal processing.

## Related notes

- [[On the Relationship Between Continuous- and Discrete-Time Quantum Walk (Childs 2010) — Paper Notes]]
- [[Szegedy Walk Quantization of Hamiltonians]]
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]]
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]]
- [[Gapped Phase Estimation]]
