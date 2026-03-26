# Projected Quantum Kernel via Local Observables

> **Source:** Huang, Broughton, Mohseni, Babbush, Boixo, Neven, McClean, arXiv:2011.01938
> **Tags:** #trick #quantum-ml #kernel-methods #classical-shadows #projected-quantum-kernel

## What it does
Fixes the fundamental failure mode of standard quantum kernels (exponential dimension → all points equidistant → no generalization) by projecting quantum states to approximate classical representations before computing similarity.

## The trick
Instead of using the full fidelity $k_Q(x_i, x_j) = \text{Tr}(\rho(x_i)\rho(x_j))$ — which approaches $\delta_{ij}$ exponentially fast — project the quantum state to local observables and define the kernel in that space.

**1-RDM projected kernel:**

$$k_{PQ}(x_i, x_j) = \exp\left(-\gamma \sum_k \|\rho_k(x_i) - \rho_k(x_j)\|_F^2\right)$$

where $\rho_k(x) = \text{Tr}_{m \neq k}[\rho(x)]$ is the single-qubit reduced density matrix. Requires measuring $\langle X_k \rangle$, $\langle Y_k \rangle$, $\langle Z_k \rangle$ for each qubit — $3n$ expectation values per state.

**All-order RDM kernel via [[Shadow Tomography for QMC Overlap Estimation|classical shadows]]:**

$$Q_\gamma^\infty(\rho(x_i), \rho(x_j)) = \mathbb{E}\exp\left(\frac{\gamma}{n}\sum_{p=1}^n (9\delta_{s_p^i s_p^j}\delta_{b_p^i b_p^j} - 4)\right)$$

Computed from local randomized Pauli measurements. Contains information about all orders of RDMs and can learn any quantum model with sufficient data.

**Why it works:** The projection reduces the effective dimension from exponential to polynomial, improving generalization. But the quantum circuit is still needed to *evaluate* the projected kernel (you need the quantum state to measure local observables), so quantum hardness is preserved. Empirically, projection *increases* the [[Geometric Difference Test for Quantum ML Advantage|geometric difference]] $g_{CQ}$ rather than decreasing it.

## When to reach for it
- Any quantum kernel method where the standard fidelity kernel is failing (kernel matrix close to identity)
- When you need a quantum ML method with provable advantage properties
- When the encoding circuit has meaningful local structure (entangling gates that create non-trivial reduced states)
- Designing NISQ-era QML experiments that might show advantage

## Complexity
- 1-RDM version: $O(n/\epsilon^2)$ measurements per state for $\epsilon$-precision on each local expectation value
- All-RDM version: $O(N_s)$ randomized measurement repetitions per state; kernel computation from stored classical data in $O(N_s^2 \cdot n)$ per pair
- Classical kernel matrix assembly: $O(N^2)$ pairs

## Caveat
- The 1-RDM version can only learn functions expressible in terms of 1-RDMs. For product-state encodings (E1-type), this may not capture inter-qubit correlations at all.
- The all-RDM version is universal but requires more measurements and has higher variance
- Projecting to classical space means you lose some information — for some quantum models, the lost information might be exactly what distinguishes quantum from classical
- The improvement depends heavily on the encoding: separable encodings (E1) show small $g_{CQ}$ even after projection; entangling encodings (E3) show large $g_{CQ}$
- DFT motivation (1-body density determines ground state) is suggestive but doesn't directly apply to arbitrary quantum ML models

## Related notes
- [[Power of Data in Quantum Machine Learning (Huang, Babbush, McClean et al 2021) — Paper Notes]]
- [[Geometric Difference Test for Quantum ML Advantage]]
- [[Adversarial Dataset Construction via Generalized Eigenvalue Problem]]
- [[Shadow Tomography for QMC Overlap Estimation]]
