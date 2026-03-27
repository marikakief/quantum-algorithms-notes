> **Source:** Rubin, Babbush, McClean, arXiv:1801.03524, New J. Phys. 20, 053020 (2018)
> **Tags:** #trick #measurement #VQE #n-representability #variance-reduction #LP #quantum-chemistry

## What it does

Reduces the total number of quantum measurements needed to estimate a fermionic Hamiltonian's expectation value by exploiting the algebraic constraints that any physical fermionic state must satisfy — specifically, the $n$-representability conditions on reduced density matrices.

## The trick

The total sample count for estimating $\langle H \rangle = \sum_\ell w_\ell \langle H_\ell \rangle$ to precision $\varepsilon$ is bounded by:

$$M \leq \left(\frac{\Lambda}{\varepsilon}\right)^2, \quad \Lambda = \sum_\ell |w_\ell|$$

For any physical $n$-electron fermionic state, certain linear combinations of Hamiltonian terms evaluate to exactly zero:

$$C_k = \sum_\ell c_{k\ell} H_\ell = 0 \quad \text{for all physical states}$$

These are the **$n$-representability equality constraints** — they follow from fermionic antisymmetry, trace conditions on the 1- and 2-RDMs, and the linear maps relating 2D, 2Q, and 2G matrices:
- ${}^2D \to {}^1D$ by tracing over one index
- ${}^2D \to {}^2Q$ via particle-hole exchange
- ${}^2D \to {}^2G$ via mixed exchange
- Fixed traces: $\text{Tr}[{}^2D] = n(n-1)$, $\text{Tr}[{}^2Q] = (m-n)(m-n-1)$, $\text{Tr}[{}^2G] = n(m-n+1)$

Adding multiples of these zero-valued constraints to $H$ leaves $\langle H \rangle$ unchanged but can reduce $\Lambda$:

$$\tilde{H} = H + \sum_k \beta_k C_k, \quad \langle \tilde{H} \rangle = \langle H \rangle$$

**Optimal $\beta$ minimizes** $\tilde{\Lambda} = \sum_\ell |\tilde{w}_\ell|$, which is an $\ell_1$ minimization:

$$\beta^* = \arg\min_\beta \|v_H - C^T \beta\|_1$$

where $v_H$ is the vector of original coefficients $w_\ell$ and $C$ is the matrix of constraint vectors. This is a **linear program** (introduce auxiliary variable $q \geq |v_H - C^T\beta|$, minimize $\mathbf{1}^T q$) solvable in polynomial time with standard LP solvers.

Modified coefficients: $\tilde{w}_\ell = w_\ell + \sum_k \beta^*_k c_{k\ell}$. Use these in the [[Pauli Expectation Value Estimation|optimal shot allocation]] $M_\ell \propto |\tilde{w}_\ell|$.

**Practical gain:** Rubin et al. show $> 10\times$ reduction in $\tilde{\Lambda}^2$ (hence $> 10\times$ fewer shots) for medium-sized chemical systems (10–30 spin-orbitals) using the DQG constraint set.

## When to reach for it

- Any [[Variational Quantum Eigensolver (VQE)|VQE]] or hybrid algorithm where measurement cost is the bottleneck.
- When the Hamiltonian is fermionic (second-quantized chemistry), not a generic spin model.
- When $\Lambda = \sum|w_\ell|$ is large relative to $\|H\|$, indicating significant cancellations among terms that the constraint structure can exploit.
- Most useful for medium to large molecules where the $O(N^4)$ Pauli terms are expensive to measure.

## Complexity

- **LP construction:** $O(K \cdot L)$ where $K$ is the number of constraints and $L$ is the number of Hamiltonian terms. For DQG constraints and a molecular Hamiltonian: $K = O(m^4)$ constraints, $L = O(m^4)$ terms.
- **LP solve:** Polynomial time via simplex or interior-point; practically fast for $m \leq 50$ orbitals.
- **Measurement reduction:** Empirically $> 10\times$ for medium systems; no worst-case guarantee.

## Caveat

- The constraints are **equality** constraints for physical states only. If the quantum state is far from physical (e.g., highly noisy), the modified Hamiltonian is no longer equal to the original.
- DQG conditions are necessary but not sufficient for $n$-representability. Adding higher-order T1, T2 constraints could improve the reduction further but at higher LP construction cost.
- No known worst-case bound on how much $\Lambda$ decreases — systems with little fermionic structure (weakly correlated, nearly classical) may see smaller gains.
- Does not directly extend to QAOA (spin Hamiltonians) because spin marginals lack the DQG antisymmetry structure.

## Related notes
- [[Application of Fermionic Marginal Constraints to Hybrid Quantum Algorithms (Rubin, Babbush, McClean 2018) — Paper Notes]]
- [[Pauli Expectation Value Estimation]]
- [[Variational Quantum Eigensolver (VQE)]]
- [[Symmetry Reduction in Qubit Hamiltonians]]
- [[2-RDM Physicality Restoration by PSD Projection]]
- [[Efficient and Noise Resilient Measurements for Quantum Chemistry on Near-Term Quantum Computers (Huggins, McClean, Rubin, Babbush et al 2021) — Paper Notes]] — Benchmarks this LP method against [[Basis Rotation Grouping for VQE Measurement]]; shows that RDM constraints don't substantially reduce actual variance (despite reducing bounds)
- [[Basis Rotation Grouping for VQE Measurement]]
- [[Qubit vs Fermionic Variance Bounds]]
