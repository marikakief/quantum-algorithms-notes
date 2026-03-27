> **Source:** Vera von Burg, Guang Hao Low, Thomas Häner, Damian S. Steiger, Markus Reiher, Martin Roetteler, Matthias Troyer, *Quantum computing enhanced computational catalysis*, arXiv:2007.14460, Phys. Rev. Research **3**, 033055 (2021)
> **Links:** [arXiv](https://arxiv.org/abs/2007.14460) · [Journal](https://doi.org/10.1103/PhysRevResearch.3.033055)
> **Tags:** #quantum-chemistry #resource-estimation #fault-tolerant #double-factorization #qubitization #FeMoCo #catalysis #Toffoli #surface-code

---

## The computational problem

Given a molecular Hamiltonian for a transition-metal catalyst in a chosen active space:

$$H = \sum_{pq} h_{pq} a^\dagger_p a_q + \frac{1}{2}\sum_{pqrs} h_{pqrs} a^\dagger_p a^\dagger_q a_r a_s$$

compute the ground-state energy via quantum phase estimation (QPE) using a qubitized block encoding, compiled to Toffoli gates under the surface code. Target: energy differences sufficient to distinguish reaction intermediates and transition states along a catalytic cycle.

The primary system is a ruthenium pincer complex that activates CO₂ and reduces it to methanol — a catalytic cycle with 12 key intermediates and transition states. Secondary benchmark: FeMoCo (54 electrons in 54 orbitals, the same active space as [[Elucidating Reaction Mechanisms on Quantum Computers (Reiher, Wiebe, Svore, Wecker, Troyer 2017) — Paper Notes|Reiher et al. 2017]]).

---

## What the paper does

This is the paper that introduced **double factorization** (DF) as the algorithmic basis for qubitized quantum chemistry resource estimates — replacing the sparse Coulomb or single-factorization approaches of [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes|Berry et al. 2019]] with a two-step low-rank decomposition that compresses the two-electron integral tensor before block-encoding.

The key claim: for FeMoCo, the DF approach reduces the Toffoli gate count from $8.8 \times 10^{10}$ (sparse qubitization) to $1.0 \times 10^{10}$, an order-of-magnitude improvement. For the ruthenium catalyst intermediates, the estimates range from $2.4 \times 10^9$ to $1.2 \times 10^{10}$ Toffolis. The paper also provides a careful analysis of active-space size escalation: while the 54-orbital FeMoCo active space is tractable, fully converged dynamical correlation would require hundreds of orbitals and remains far out of reach.

---

## The algorithm / construction

### Double factorization

The two-electron integrals $h_{pqrs}$ are first written in chemist notation and decomposed by an eigendecomposition of the two-electron tensor. The Hamiltonian decomposes as:

$$H = H_{\rm one} + \frac{1}{2}\sum_\ell \left(\sum_j \lambda_{j}^{(\ell)} n_j^{(\ell)}\right)^2$$

where $n_j^{(\ell)} = \sum_\sigma (f_{j\sigma}^{(\ell)})^\dagger f_{j\sigma}^{(\ell)}$ are occupation operators in rotated orbital frames $f^{(\ell)} = U^{(\ell)} a$, and $\lambda_j^{(\ell)}$ are eigenvalues of the $\ell$-th leaf. Each leaf is a squared one-body operator, making the Hamiltonian a sum of squared diagonal operators after orbital rotation.

Truncation: only leaves with $\|\lambda^{(\ell)}\|_1 > \varepsilon_\ell$ are kept. The truncation threshold controls accuracy vs. cost.

### Block-encoding and qubitization

Each leaf contributes a term $T_\ell = \left(\sum_j \lambda_j^{(\ell)} n_j^{(\ell)}\right)^2$. Since it's diagonal in the rotated frame, SELECT only needs to implement phase kicks proportional to $\lambda_j^{(\ell)}$. PREPARE uses QROAM to load $\lambda^{(\ell)}$ values. The qubitization iterate $W = (2G^\dagger G - I)$ (where $G$ encodes the block structure) has eigenvalues $e^{\pm i \arcsin(E_k/\lambda)}$ for energy eigenvalues $E_k$, enabling QPE with Heisenberg-limited scaling.

The 1-norm $\lambda = \sum_\ell \|\lambda^{(\ell)}\|_1^2 / 2 + \lambda_{\rm one}$ determines the iteration count of QPE:

$$N_{\rm QPE} \sim \frac{\pi \lambda}{2 \varepsilon}$$

The central algorithmic contribution is reducing $\lambda$ by the DF compression compared to alternative decompositions.

### Resource accounting pipeline

1. Compute molecular integrals classically (CASSCF with DFT pre-optimization)
2. Apply DF decomposition, optimize truncation threshold $\varepsilon_\ell$
3. Count Toffolis for QPE: $T_{\rm total} = N_{\rm QPE} \cdot T_{\rm oracle}$ where $T_{\rm oracle}$ counts QROAM + rotation synthesis costs per qubitization step
4. Map to physical qubits via surface code, accounting for magic state distillation factories

---

## Key results

**FeMoCo (54e, 54o):**

| Method | Logical qubits | Toffoli count |
|---|---|---|
| [[Elucidating Reaction Mechanisms on Quantum Computers (Reiher, Wiebe, Svore, Wecker, Troyer 2017) — Paper Notes|Reiher et al. 2017]] (Trotter) | 111 | $5.0 \times 10^{13}$ |
| [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes|Berry et al. 2019]] (sparse) | 2,190 | $8.8 \times 10^{10}$ |
| **von Burg et al. 2020 (DF)** | **3,725** | $\mathbf{1.0 \times 10^{10}}$ |
| [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes|Lee et al. 2021]] (THC) | 2,142 | $5.3 \times 10^9$ |

**Ruthenium catalyst (representative intermediate, 33–37 orbitals):**
- Toffoli count: $2.4 \times 10^9$–$1.2 \times 10^{10}$ depending on intermediate
- Physical qubits: ~$10^6$–$4 \times 10^6$ at error rate $10^{-3}$
- Runtime: ~4–22 days at 1 µs cycle time

**Scaling with active space size:**
- The paper explicitly confronts the dynamical correlation problem: FeMoCo at the converged CBS limit likely needs $\sim$1000+ orbitals, which is intractable even with DF
- This is identified as the central open problem

---

## Comparison with prior work

| Paper | Algorithm | FeMoCo Toffolis |
|---|---|---|
| [[Elucidating Reaction Mechanisms on Quantum Computers (Reiher, Wiebe, Svore, Wecker, Troyer 2017) — Paper Notes|Reiher et al. (2017)]] | Trotter-Suzuki | $5 \times 10^{13}$ |
| [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes|Berry et al. (2019)]] | Sparse qubitization | $8.8 \times 10^{10}$ |
| **von Burg et al. (2020)** | **Double factorization** | $\mathbf{1.0 \times 10^{10}}$ |
| [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes|Lee et al. (2021)]] | THC | $5.3 \times 10^9$ |

The DF approach improves over sparse qubitization for FeMoCo by $\sim 9\times$ and over Trotter by $\sim 5000\times$. THC (2021) subsequently improved over DF by another $\sim 2\times$.

---

## Limits / caveats

- **Active space vs. dynamical correlation:** The entire paper operates in a fixed active space. For FeMoCo, the active space is 54e/54o from Reiher et al. — adequate to study qualitative chemistry, but not quantitatively converged. The paper explicitly flags that including dynamical correlation requires dramatically larger active spaces (potentially 1000+ orbitals), where even DF would require $10^{16}+$ Toffolis.
- **Classical preprocessing bottleneck:** The DF decomposition and orbital optimization are classical, but for large systems (100+ orbitals), the classical cost becomes significant.
- **1-norm optimality:** The DF decomposition is not guaranteed to give the minimal 1-norm. THC (Lee et al. 2021) showed that a factored THC representation achieves better $\lambda$ reduction for the same accuracy target.
- **Ruthenium catalyst:** This is a near-term prototype, but the "computational catalysis" vision requires both the quantum resource reductions shown here and classical-quantum interfacing that remains engineering-incomplete.
- **Physical qubit estimates:** Assumes idealized surface code with fixed cycle time and error rate — no account for crosstalk, connectivity constraints, or realistic compilation overhead.

---

## Reusable ideas

1. [[Double Factorization of Two-Electron Integrals]] — Decompose the two-electron integral tensor by first applying a Cholesky/eigendecomposition to get rank-$L$ leaves, then diagonalizing each leaf. The two-body operator becomes a sum of squared one-body diagonal operators in rotated frames. Enables efficient SELECT: each leaf's operation is just phase kicks proportional to $\lambda^{(\ell)}$.

2. [[Double Factorisation for Block-Encoding]] — The double-factorized Hamiltonian admits a qubitization walk operator with 1-norm $\lambda \approx \sum_\ell \|\lambda^{(\ell)}\|_1^2/2$ instead of the full 4-index tensor norm. Reduces QPE iteration count proportionally.

3. [[QROAM (Space-Time Tradeoff for QROM)]] — Used to load $\lambda^{(\ell)}$ values coherently during PREPARE. Balances ancilla count vs. gate depth.

4. [[Qubitization Iterate]] — The qubitization walk operator $W$ applied to block-encoded Hamiltonians, enabling Heisenberg-limited energy estimation via QPE.

5. [[Error Budget Optimization for Fault-Tolerant Algorithms]] — Joint optimization over QPE precision, truncation threshold, and gate synthesis error to minimize total Toffoli count subject to a total error budget.

6. [[Hybrid Classical-Quantum Chemistry Workflow]] — The paper articulates the full pipeline: DFT structure → CASSCF natural orbitals → four-index integrals → DF compression → QPE on quantum hardware → energy differences → kinetic modeling. This workflow is now the standard template.

---

## Cross-links

### Paper notes
- [[Elucidating Reaction Mechanisms on Quantum Computers (Reiher, Wiebe, Svore, Wecker, Troyer 2017) — Paper Notes]] — Predecessor; establishes the FeMoCo benchmark and the "quantum chemistry on fault-tolerant hardware" problem statement. This paper is its algorithmic successor.
- [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes]] — Sparse qubitization approach. von Burg et al. directly compare against this, claiming $9\times$ improvement for FeMoCo via DF.
- [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes]] — Direct successor: THC achieves better $\lambda$ reduction than DF for FeMoCo ($5.3 \times 10^9$ vs $1.0 \times 10^{10}$ Toffolis).
- [[Low Rank Representations for Quantum Simulation of Electronic Structure (Motta, Babbush, Chan et al 2018) — Paper Notes]] — Introduced double factorization as a classical compression scheme; this paper provides the quantum resource accounting on top.
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]] — Provides the qubitization framework that DF plugs into.
- [[FeMoCo Resource Estimation Timeline]] — Timeline tracking all FeMoCo estimates; this paper is a major waypoint.

### Trick cards
- [[Double Factorization of Two-Electron Integrals]]
- [[Double Factorisation for Block-Encoding]]
- [[QROAM (Space-Time Tradeoff for QROM)]]
- [[Qubitization Iterate]]
- [[Error Budget Optimization for Fault-Tolerant Algorithms]]
- [[Hybrid Classical-Quantum Chemistry Workflow]]
- [[DFTHC Factorization]]
