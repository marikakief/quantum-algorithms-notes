
> **Source:** Babbush, Berry, Sanders, Kivlichan, Scherer, Wei, Love, Aspuru-Guzik, arXiv:1506.01029 (2018)
> **Tags:** #trick #quantum-chemistry #oracle #integrals #on-the-fly #sparse-oracle

## What it does

Builds a quantum oracle that evaluates individual molecular integral values (one- and two-electron integrals $h_{ij}$, $h_{ijkl}$) directly inside the quantum circuit, using $\tilde{O}(N)$ gates per evaluation. Avoids the classical preprocessing step of computing and storing all $O(N^4)$ integrals, which would be exponentially expensive for large basis sets or require transmitting exponentially many values to the quantum computer.

## The trick

Classical quantum chemistry computes the molecular integral matrix elements:

$$h_{ij} = \int \phi_i^*(r)\left(-\frac{\nabla^2}{2} - \sum_q \frac{Z_q}{|R_q - r|}\right)\phi_j(r)\, dr$$

$$h_{ijkl} = \int \int \frac{\phi_i^*(r_1)\phi_j^*(r_2)\phi_k(r_1)\phi_l(r_2)}{|r_1 - r_2|}\, dr_1 dr_2$$

for all $O(N^2)$ one-electron and $O(N^4)$ two-electron pairs — then stores them. For large $N$ this is impractical classically (exponential in basis set size for some scenarios).

**The quantum approach:** Instead, build an oracle circuit $Q_\text{val}$ that, given orbital indices $(i,j)$ or $(i,j,k,l)$ as input registers, **computes the integral value into an output register** by evaluating a discrete Riemann sum:

$$h_{ij} \approx \sum_{\rho=1}^{\mu} w_\rho \cdot f_{ij}(\xi_\rho)$$

where $\{\xi_\rho\}$ are grid points, $w_\rho$ are weights, and $f_{ij}(\xi_\rho)$ is the integrand at grid point $\rho$.

Each grid-point evaluation requires: (1) compute $\phi_i(\xi_\rho)$ and $\phi_j(\xi_\rho)$ from the orbital indices and grid point; (2) compute the Coulomb/kinetic kernel; (3) multiply. Under the orbital regularity assumptions (values and derivatives bounded by polylog in $N$, exponential spatial decay), the orbital evaluation cost is $\tilde{O}(1)$ per point, and $\mu = \tilde{O}(N)$ grid points suffice to achieve error $\delta$ via the bounds in Lemmas 1–3 of the paper.

**Combined oracle:** The CI matrix simulation needs a "select" oracle — given a 1-sparse color index $\gamma$ and a row $\alpha$, compute the column $\beta$ and the (normalized, rounded) matrix entry. The architecture is:

```
[color γ] ---→ Q_col: compute column β from row α
[row α]   ---→          ↓
              [β in ancilla]
              Q_val: evaluate h_{ij} or h_{ijkl} for the (α,β) pair
                     ↓
              [±1 sign in ancilla, used by select(H)]
```

Gate cost: $O(\mu) = \tilde{O}(N)$ per oracle call (dominated by the Riemann sum evaluation). The $O(N)$ factor comes from the $\tilde{O}(N)$ grid points needed to resolve the integrand.

**Why this beats the classical approach:** Classically, evaluating all $N^4$ integrals takes $O(N^4 \cdot \mu)$ time and requires $O(N^4)$ storage. The quantum oracle evaluates one integral at a time, on demand, at cost $\tilde{O}(N)$. When combined with [[Truncated Taylor Series Simulation]], only $O(\text{circuit depth})$ oracle calls are needed — never all $N^4$ at once.

## When to reach for it

- Implementing simulation oracles for molecular Hamiltonians where classical precomputation of all integrals is too expensive.
- When building sparse-access oracles ($H(i,j)$ queries) for quantum linear algebra algorithms (QSVT, quantum walks) applied to chemistry.
- Any scenario where $H$ is defined implicitly (by a formula) rather than explicitly (as a stored matrix), and you need access to individual matrix entries.

## Complexity

- **Gate cost per integral evaluation:** $\tilde{O}(N)$ (Riemann sum with $\tilde{O}(N)$ grid points)
- **Compared to classical:** Classical precomputation: $O(N^4 \cdot \tilde{O}(N)) = \tilde{O}(N^5)$ time, $O(N^4)$ storage. Quantum on-the-fly: $\tilde{O}(N)$ per query, $\tilde{O}(\eta)$ qubits total (no classical storage needed)
- **Total in CI simulation:** combined with $\tilde{O}(\eta^2 N^2)$ 1-sparse terms and $O(\log 1/\varepsilon)$ Taylor truncation, the per-query cost $\tilde{O}(N)$ yields total gate count $\tilde{O}(\eta^2 N^3 t)$

## Caveat

- The $\tilde{O}(N)$ grid point count relies on the orbital regularity assumptions in Babbush et al. Theorem 1. For orbitals that are slowly decaying or highly oscillatory, more points may be needed.
- Requires quantum arithmetic circuits for the Riemann sum. These are feasible in principle but add substantial constant-factor overhead compared to simple gate operations.
- The $\tilde{O}(\eta)$ working-qubit count for the oracle needs temporary ancilla registers for the arithmetic — the actual circuit may need more qubits transiently.
- This approach is specific to the sparse CI representation. For second-quantized or other representations, the oracle structure differs.

## Related notes
- [[Exponentially More Precise Quantum Simulation of Fermions in the CI Representation (Babbush et al 2018) — Paper Notes]]
- [[CI Matrix Simulation (First-Quantized Encoding)]]
- [[Truncated Taylor Series Simulation]]
- [[1-Sparse Hamiltonian Decomposition via Graph Coloring]]
