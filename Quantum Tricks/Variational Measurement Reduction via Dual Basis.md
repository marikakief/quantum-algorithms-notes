# Variational Measurement Reduction via Dual Basis

> **Source:** Babbush, Wiebe, McClean, McClain, Neven, Chan, arXiv:1706.00023 (2018)
> **Tags:** #trick #variational #measurement #VQE #shot-reduction #dual-basis #plane-wave #commuting-terms

## What it does

Reduces the number of distinct measurement circuits needed for variational energy estimation from $O(N^4)$ to 2 (for the [[Plane-Wave Dual Basis]] Hamiltonian), cutting the total shot count from $O(N^8/\varepsilon^2)$ to $O(N^4/\varepsilon^2)$ for fixed absolute error $\varepsilon$.

## The trick

Standard VQE with a Gaussian-orbital Hamiltonian requires sampling $O(N^4)$ distinct Pauli terms. Even grouping commuting terms, the number of measurement bases is $O(N^4)$ in general.

In the [[Plane-Wave Dual Basis]], the Hamiltonian splits as $H = T + (U + V)$ where:

- $T = \sum_{p,\sigma} \frac{k_p^2}{2} a^\dagger_{p\sigma} a_{p\sigma}$: all $N$ terms are simultaneously diagonal in the plane-wave basis. Under Jordan–Wigner, these are all $Z$-type single-body operators that commute with each other.
- $U + V = \sum_{j,\sigma} U_j n_{j\sigma} + \sum_{j \neq j',\sigma,\sigma'} V_{jj'} n_{j\sigma} n_{j'\sigma'}$: all $\Theta(N^2)$ terms are simultaneously diagonal in the dual basis. Under Jordan–Wigner, these are all $Z$ and $ZZ$ operators that commute with each other.

So the entire Hamiltonian can be measured in **two** circuits:

1. **Dual basis circuit:** Prepare the variational state $|\psi(\theta)\rangle$, measure all qubits in the computational basis (dual basis). Extract $\langle U + V \rangle$ from the $N^2$ correlators.

2. **Plane-wave basis circuit:** Prepare $|\psi(\theta)\rangle$, apply the [[Fermionic Fast Fourier Transform (FFFT)]], measure all qubits. Extract $\langle T \rangle$ from the $N$ diagonal terms.

The variance of each group is bounded. For $U + V$: the variance per shot of the total estimator is bounded by the square of the energy fluctuation in that sector. The key quantity is the sum of squared Hamiltonian coefficients in each group:

$$\text{Var}[\widehat{E}] \leq \left(\sum_\ell h_\ell^2\right) / M$$

Using the dual-basis structure, this sum is bounded:
- For fixed absolute error $\varepsilon$: total shots $M \in O(N^4/\varepsilon^2)$
- For fixed relative error $\mu$ in the thermodynamic limit (fixed density): $M \in O(N^2/\mu^2)$

The $N^2$ scaling in the relative-error case comes from the fact that the energy itself scales as $N$ at fixed density, while $\sum h_\ell^2 \in O(N^3)$, giving $M \propto \sum h_\ell^2 / (\mu E)^2 \in O(N^3/(N^2 \mu^2)) = O(N/\mu^2)$... the exact scaling depends on the density regime. The paper derives the thermodynamic-limit scaling explicitly.

**No grouping heuristics needed.** The grouping is exact and comes from the Hamiltonian structure, not from a classical optimisation over Pauli term partitions. This is exact and avoids the NP-hard clique-cover problem that arises for general Pauli Hamiltonians.

## When to reach for it

- VQE on periodic systems (jellium, crystal lattices) using the plane-wave dual basis
- Any variational algorithm where the Hamiltonian splits into two exactly-commuting groups (kinetic and potential simultaneously diagonal in Fourier-dual bases)
- As an alternative to LP-based variance reduction (see [[Variance Reduction via N-Representability Constraints (LP Method)]]) when the system has dual-basis structure — this approach requires no classical LP solve
- When designing near-term hardware experiments where total shot count is the limiting resource

## Complexity

- **Measurement circuits:** 2 (not $O(N^4)$)
- **Total shots (fixed $\varepsilon$):** $O(N^4/\varepsilon^2)$
- **Total shots (thermodynamic limit, fixed $\mu$):** $O(N^2/\mu^2)$
- **Overhead per circuit:** One FFFT application ($O(N)$ depth, $O(N \log N)$ gates) for the plane-wave measurement circuit

Comparison with Gaussian-orbital VQE: the standard approach needs $O(N^4)$ measurement bases and $O(N^8/\varepsilon^2)$ total shots (rough bound). This approach needs 2 measurement bases and $O(N^4/\varepsilon^2)$ shots — an $O(N^4)$ reduction in shot count.

Comparison with [[Variance Reduction via N-Representability Constraints (LP Method)]]: the LP method works for any Hamiltonian and achieves $>10\times$ reduction in practice for molecular systems, but requires a classical LP solve over $n$-representability constraints. This approach is structurally exact and requires no classical optimisation, but only applies when the Hamiltonian has the dual-basis commuting structure.

## Caveat

- **Periodic systems only.** The commuting structure is specific to the plane-wave dual basis. It doesn't apply to Gaussian-orbital Hamiltonians or real-space grid approaches directly.
- **FFFT cost for kinetic measurement.** Measuring $\langle T \rangle$ requires applying the FFFT circuit before measurement, adding $O(N)$ depth to that circuit. For near-term devices with limited coherence, this may be costly.
- **State preparation must work in both bases.** The variational ansatz $|\psi(\theta)\rangle$ must be preparable efficiently before either measurement. If the ansatz natively lives in the dual basis (e.g., the jellium proposal), the dual measurement is free but the plane-wave measurement requires an extra FFFT.
- **The $O(N^4/\varepsilon^2)$ bound may not be tight.** The bound is conservative; in practice, the variance may be lower, especially for low-density systems where correlations are weaker.

## Related notes
- [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes]]
- [[Plane-Wave Dual Basis]]
- [[Fermionic Fast Fourier Transform (FFFT)]]
- [[Pauli Expectation Value Estimation]]
- [[Variance Reduction via N-Representability Constraints (LP Method)]]
- [[Variational Quantum Eigensolver (VQE)]]
- [[Application of Fermionic Marginal Constraints to Hybrid Quantum Algorithms (Rubin, Babbush, McClean 2018) — Paper Notes]]
- [[Efficient and Noise Resilient Measurements for Quantum Chemistry on Near-Term Quantum Computers (Huggins, McClean, Rubin, Babbush et al 2021) — Paper Notes]] — Generalizes this $L=1$ trick to arbitrary molecular orbital bases with $L = O(N)$ factors
- [[Basis Rotation Grouping for VQE Measurement]]
