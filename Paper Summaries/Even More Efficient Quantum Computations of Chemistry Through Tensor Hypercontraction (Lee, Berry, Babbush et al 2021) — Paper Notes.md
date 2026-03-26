> **Source:** Joonho Lee, Dominic W. Berry, Craig Gidney, William J. Huggins, Jarrod R. McClean, Nathan Wiebe, Ryan Babbush, *Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction*, arXiv:2011.03494, PRX Quantum **2**, 030305 (2021)
> **Links:** [arXiv](https://arxiv.org/abs/2011.03494) · [Journal](https://doi.org/10.1103/PRXQuantum.2.030305)
> **Tags:** #quantum-simulation #quantum-chemistry #qubitization #tensor-hypercontraction #fault-tolerant #T-complexity #FeMoCo #block-encoding #surface-code

---

## The computational problem

Given the electronic Hamiltonian in an arbitrary second-quantized basis of $N$ spin-orbitals:

$$H = \sum_\sigma \sum_{p,q} T_{pq}\, a^\dagger_{p,\sigma} a_{q,\sigma} + \frac{1}{2} \sum_{\alpha,\beta} \sum_{p,q,r,s} V_{pqrs}\, a^\dagger_{p,\alpha} a_{q,\alpha}\, a^\dagger_{r,\beta} a_{s,\beta}$$

construct a [[Qubitization (Quantum Walk for Spectral Encoding)|qubitized]] quantum walk operator whose eigenphases encode the eigenvalues of $H$. Use quantum phase estimation to sample eigenvalues to precision $\varepsilon$. The goal is to minimize the total Toffoli (or T) gate count and logical qubit count required under fault-tolerant compilation, specifically targeting molecular orbital bases where the Coulomb tensor $V_{pqrs}$ is dense (no special diagonal structure).

---

## What the paper does

Achieves the lowest known Toffoli complexity for quantum simulation of chemistry in an arbitrary orbital basis: $\widetilde{O}(N \lambda_\zeta / \varepsilon)$ with $\widetilde{O}(N)$ space. The trick is to use tensor hypercontraction (THC) to factorize the Coulomb operator, then rewrite the Hamiltonian in a larger non-orthogonal basis where the Coulomb interaction becomes diagonal. [[Qubitization (Quantum Walk for Spectral Encoding)|Qubitization]] of the resulting diagonal-Coulomb operator — with [[Givens Rotation Slater Determinant Preparation|Givens rotation]] basis changes loaded from [[QROM (Quantum Read-Only Memory)|QROM]] — gives an $\widetilde{O}(N)$-Toffoli block encoding per walk step, matching the scaling previously available only for plane-wave dual bases. The $\lambda_\zeta$ norm is comparable to (and sometimes smaller than) the double low rank factorization's $\lambda_{\rm DF}$, so this isn't just an asymptotic win — it's a concrete win at finite size too.

For FeMoCo (Reiher Hamiltonian): 2,142 logical qubits and $5.3 \times 10^9$ Toffolis. That's $\sim 2 \times$ fewer Toffolis than the next best method (double factorization, von Burg et al.) and $\sim 10{,}000 \times$ fewer than Reiher et al.'s original Trotter estimate. Surface code compilation: $\sim 4$ million physical qubits and $< 4$ days runtime at 1 µs cycle time and $0.1\%$ physical error rate.

---

## The algorithm / construction

### Step 1: THC factorization of the Coulomb operator

The standard THC factorization approximates the two-electron integrals as:

$$V_{pqrs} \approx G_{pqrs} = \sum_{\mu,\nu=1}^{M} \chi_p^{(\mu)} \chi_q^{(\mu)}\, \zeta_{\mu\nu}\, \chi_r^{(\nu)} \chi_s^{(\nu)}$$

where $M = O(N)$ is the THC rank (empirically $M \approx 5$–$10 \times N/2$), the $\chi_p^{(\mu)}$ are "leaf tensors" defining non-orthogonal orbitals, and $\zeta_{\mu\nu}$ is an $M \times M$ symmetric "central tensor." The information content is $\Gamma = O(MN + M^2) = O(N^2)$, already smaller than single factorization's $O(N^3)$.

The THC tensors are obtained numerically via least-squares fitting (or interpolative decomposition). Key point: the representation is approximate, so accuracy depends on $M$ and the optimization procedure. The paper uses gradient descent on a least-squares objective.

### Step 2: Non-orthogonal diagonal Coulomb representation

Rather than directly qubitizing the THC form (which gives a large $\lambda$, see Appendix E of the paper), the authors make a more elegant move. Define $M$ new "THC orbitals" via:

$$\tilde{\phi}_\mu(\mathbf{r}) = \sum_{p=1}^{N/2} \chi_p^{(\mu)} \phi_p(\mathbf{r})$$

These form a non-orthogonal basis. In this basis, the Coulomb operator becomes diagonal:

$$V \approx \frac{1}{2} \sum_{\alpha,\beta} \sum_{\mu \neq \nu} \zeta_{\mu\nu}\, \tilde{n}_{\mu,\alpha}\, \tilde{n}_{\nu,\beta}$$

where $\tilde{n}_{\mu,\alpha}$ is the number operator in the THC orbital basis. The two-body interaction is now a sum of $n_\mu n_\nu$ terms — the same structure as the [[Plane-Wave Dual Basis]] Hamiltonian from [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes|Babbush et al. 2018]]. The catch: the THC orbitals are non-orthogonal, so the standard second-quantized formalism doesn't directly apply.

### Step 3: Handling non-orthogonality via per-term basis rotations

The key insight that makes the non-orthogonal picture work for qubitization: write the Hamiltonian as an LCU where each term involves rotating into the THC basis, applying a diagonal operator, and rotating back. Concretely, the block encoding implements:

1. Use [[Coherent Alias Sampling for PREPARE|coherent alias sampling]] to prepare a superposition over the LCU index registers $(\mu, \nu)$ with amplitudes proportional to $\sqrt{|\zeta_{\mu\nu}|/\lambda_\zeta}$.

2. For each pair $(\mu, \nu)$, the SELECT oracle:
   - Loads [[Givens Rotation Slater Determinant Preparation|Givens rotation]] angles from [[QROM (Quantum Read-Only Memory)|QROM]] (the angles define the basis rotation from computational basis to THC orbital $\mu$).
   - Applies the Givens rotation network to rotate the system register into the THC basis.
   - Applies a diagonal $n_\mu n_\nu$ interaction (just a controlled-Z).
   - Reverses the Givens rotation to go back.

3. The one-body part $T_{pq}$ is absorbed into the LCU in a standard way.

The non-orthogonality is sidestepped because each LCU term independently rotates into and out of its own THC basis. The overlap matrix of the non-orthogonal basis never needs to be inverted or diagonalized on the quantum computer.

### Step 4: Angle coarse-graining

The Givens rotation angles from QROM contribute significantly to the cost: there are $O(N^2)$ angles (one per Givens rotation per THC index), each requiring $b_\theta$ bits of precision. The paper introduces a coarse-graining strategy: round rotation angles to fewer bits, absorbing the induced error into the Hamiltonian approximation error budget. This creates a smooth tradeoff between rotation precision $b_\theta$ and the accuracy of the block-encoded Hamiltonian. For FeMoCo, $b_\theta = 16$–$20$ bits suffice for chemical accuracy.

### Step 5: Qubitized phase estimation

Repeat the walk operator $O(\lambda_\zeta / \varepsilon)$ times for phase estimation. The total Toffoli cost per walk step is:

$$\text{Cost per step} = 2M\left\lceil\frac{N/2}{2}\right\rceil + O(N + M\log M + b_\theta N)$$

where the first term is the Givens rotation cost (two passes per step: one for $\mu$, one for $\nu$), and the remaining terms cover QROM lookups, alias sampling, and arithmetic. The dominant scaling is $O(MN) = O(N^2)$ per step, but since there are $O(\lambda_\zeta/\varepsilon)$ steps, the total is $\widetilde{O}(N\lambda_\zeta/\varepsilon)$ after accounting for the $\sqrt{\Gamma}$ QROM cost reduction.

---

## Key results

**Theorem (informal, from Section III.4):** There exists a qubitized quantum walk for the THC Hamiltonian with:

- **Toffoli complexity per step:** $\widetilde{O}(N)$
- **Total Toffoli complexity:** $\widetilde{O}(N \lambda_\zeta / \varepsilon)$
- **Space complexity:** $\widetilde{O}(N)$ logical qubits
- **$\lambda_\zeta$ norm:** $\lambda_\zeta \leq \lambda_{\rm DF} \leq \lambda_V \leq \lambda_{\rm SF}$ (ordering holds in general; sometimes $\lambda_\zeta$ beats $\lambda_{\rm DF}$ significantly)

**FeMoCo resource estimates (Table 3 of paper):**

| Algorithm | Logical qubits | Toffoli count (Reiher) | Toffoli count (Li) |
|---|---|---|---|
| Reiher et al. (Trotter) | 111 | $5.0 \times 10^{13}$ | — |
| Berry et al. (sparse) | 2,190 | $8.8 \times 10^{10}$ | $4.4 \times 10^{10}$ |
| Berry et al. (single factorization) | 3,320 | $9.5 \times 10^{10}$ | $1.2 \times 10^{11}$ |
| von Burg et al. (double factorization) | 3,725 | $1.0 \times 10^{10}$ | $6.4 \times 10^{10}$ |
| **This work (THC)** | **2,142** | $\mathbf{5.3 \times 10^9}$ | $\mathbf{3.2 \times 10^{10}}$ |

**Surface code compilation (Reiher FeMoCo):**
- $\sim 4$ million physical qubits, $< 4$ days runtime (0.1% gate error, 1 µs cycle)
- $\sim 1$ million physical qubits, $< 2$ days runtime (0.01% gate error)

**Empirical scaling on hydrogen benchmarks (Table 2 of paper):**

| Algorithm | H₄ continuum limit | H-chain thermodynamic limit |
|---|---|---|
| Double factorization (von Burg) | $\widetilde{O}(N^{3.8}/\varepsilon)$ | $\widetilde{O}(N^{3.4}/\varepsilon)$ |
| **THC (this work)** | $\widetilde{O}(N^{3.1}/\varepsilon)$ | $\widetilde{O}(N^{2.1}/\varepsilon)$ |

The thermodynamic limit scaling of $N^{2.1}$ is striking — it's barely above $N^2$, which is the scaling for plane-wave dual basis qubitization where the Coulomb operator is already diagonal. The THC factorization achieves nearly the same efficiency in an arbitrary molecular orbital basis.

---

## Comparison with prior work

| Feature | Sparse (Berry 2019) | Single factorization (Berry 2019) | Double factorization (von Burg 2020) | **THC (this work)** |
|---|---|---|---|---|
| Toffoli per step | $\widetilde{O}(N + \sqrt{S})$ | $\widetilde{O}(N^{3/2})$ | $\widetilde{O}(N\sqrt{\Xi})$ | $\widetilde{O}(N)$ |
| Space | $\widetilde{O}(N + \sqrt{S})$ | $\widetilde{O}(N^{3/2})$ | $\widetilde{O}(N\sqrt{\Xi})$ | $\widetilde{O}(N)$ |
| $\lambda$ ordering | $\lambda_V$ | $\lambda_{\rm SF}$ (largest) | $\lambda_{\rm DF}$ (smallest prior) | $\lambda_\zeta$ (smallest) |
| Works with arbitrary basis? | Yes | Yes | Yes | Yes |
| Requires orthogonal basis? | No | No | No | No |

The paper also corrects errors in von Burg et al.'s algorithm (Appendix C) and significantly improves the resource estimates for Berry et al.'s sparse and SF algorithms (Appendices A & B): the sparse method's Toffoli count for FeMoCo drops by $\sim 3\times$ compared to what Berry et al. originally reported.

---

## Limits / caveats

1. **THC rank scaling.** The asymptotic analysis assumes $M = O(N)$ THC rank, which holds empirically but isn't rigorously proven for all molecular systems. The constant factor matters: $M \approx 5$–$10 \times N/2$ in practice. If a system requires larger $M$, the advantage shrinks.

2. **Classical preprocessing cost.** Computing the THC factorization requires a least-squares fit that scales as $O(N^4)$ or worse classically. For very large $N$, this classical preprocessing could become a bottleneck (though it's polynomial and one-time).

3. **Angle coarse-graining error.** The rotation angle precision $b_\theta$ must be chosen carefully: too few bits and the Hamiltonian approximation error dominates; too many bits and the QROM cost grows. The paper handles this via an explicit error bound (Section III.5), but the tradeoff is system-dependent.

4. **Non-orthogonal basis adds cost.** Each walk step requires two full Givens rotation circuits (one for $\mu$, one for $\nu$), each costing $O(N)$ gates. This constant factor overhead is the price of using a non-orthogonal basis. For the plane-wave dual basis (which is already diagonal), no rotations are needed — so the THC approach is always slower than dual-basis qubitization per step, though it achieves similar *asymptotic* scaling.

5. **FeMoCo is a small-molecule benchmark.** The $\sim 2\times$ improvement over double factorization is real but modest at FeMoCo scale. The asymptotic advantage ($\widetilde{O}(\sqrt{N})$ better spacetime volume) matters more for larger systems.

6. **Initial state preparation not addressed.** Like all QPE-based approaches, the algorithm assumes access to a good initial state with significant overlap with the ground state. See [[Postponing the Orthogonality Catastrophe (Tubman, Mejuto-Zaera, Babbush et al 2018) — Paper Notes|Tubman et al. (2018)]] for discussion of this bottleneck.

---

## Reusable ideas

1. [[THC Non-Orthogonal Diagonal Coulomb Representation]] — Transform the Coulomb operator into diagonal form using THC leaf tensors to define a non-orthogonal basis; sidestep non-orthogonality by rotating into/out of the THC basis independently for each LCU term.

2. [[QROM-Loaded Givens Rotation Networks]] — Store basis rotation angles in QROM and load them coherently for each LCU index; this generalizes the double factorization approach of von Burg et al. to arbitrary non-orthogonal bases.

3. [[Rotation Angle Coarse-Graining for Block Encodings]] — Round Givens rotation angles to $b_\theta$ bits of precision, trading rotation fidelity for reduced QROM cost; the induced error is analytically bounded and absorbed into the Hamiltonian approximation error budget.

---

## References within this paper

**Papers with notes in the vault:**
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes|Babbush, Gidney et al. (2018)]] — Introduces [[Qubitization (Quantum Walk for Spectral Encoding)|qubitization]], [[Unary Iteration]], [[QROM (Quantum Read-Only Memory)|QROM]], [[Coherent Alias Sampling for PREPARE|coherent alias sampling]] for chemistry; this paper uses all four.
- [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes|Babbush, Wiebe, McClean et al. (2018)]] — [[Plane-Wave Dual Basis]]; the diagonal Coulomb structure in the dual basis is the template that THC replicates for arbitrary bases.
- [[Quantum Simulation of Electronic Structure with Linear Depth and Connectivity (Kivlichan, McClean et al 2018) — Paper Notes|Kivlichan, McClean et al. (2018)]] — [[Givens Rotation Slater Determinant Preparation]] and [[Fermionic Swap Network]]; basis rotation technique reused here for non-orthogonal THC orbitals.
- [[Compressing Many-Body Fermion Operators under Unitary Constraints (Rubin, Lee, Babbush 2021) — Paper Notes|Rubin, Lee, Babbush (2021)]] — Related tensor factorization work; Joonho Lee is co-author on both.
- [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes|Su, Berry, Wiebe, Rubin, Babbush (2021)]] — Competing approach using first-quantized plane waves; THC is the best second-quantized approach, first-quantized qubitization may still win for very large $N$.
- [[Quantum Simulation of Chemistry with Sublinear Scaling in Basis Size (Babbush, Berry, McClean, Neven 2019) — Paper Notes|Babbush, Berry, McClean, Neven (2019)]] — First-quantized plane-wave interaction picture; sublinear in $N$ but requires plane waves.
- [[Postponing the Orthogonality Catastrophe (Tubman, Mejuto-Zaera, Babbush et al 2018) — Paper Notes|Tubman et al. (2018)]] — Initial state preparation for QPE, a bottleneck this paper doesn't address.

**Paper now in vault:**
- [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes]] — immediate predecessor; introduces sparse and single-factorization qubitization for molecular bases; THC paper corrects errors and improves resource estimates for both methods.
- von Burg, Low, Häner, Steiger, Reiher, Roetteler, Troyer (2020) — "Quantum Computing Enhanced Computational Catalysis" (arXiv:2007.14460). Double factorization qubitization; the primary comparison target. The THC paper also corrects errors in this algorithm.
- Hohenstein, Parrish, Martínez (2012) — Original THC factorization papers from classical quantum chemistry.
- Motta, Ye, McClean, Li, Chan (2021) — Low-rank factorization techniques for quantum computing; source of both single and double factorization ideas.
- [[Elucidating Reaction Mechanisms on Quantum Computers (Reiher, Wiebe, Svore, Wecker, Troyer 2017) — Paper Notes|Reiher, Wiebe, Svore, Wecker, Troyer (2017)]] — FeMoCo Trotter simulation; the standard benchmark. ~$10^{14}$ T gates for 108 spin-orbitals; this paper achieves ~$20{,}000\times$ improvement.
- Li, Li, Dattani, Umrigar (2019) — Improved FeMoCo Hamiltonian with proper open-shell character.
- Low, Chuang (2019) — [[Qubitization (Quantum Walk for Spectral Encoding)|Qubitization]] framework.
- Litinski (2019) — Surface code magic state distillation; used for physical resource estimates.

---

## Cross-links

### Related paper notes
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]] — foundational qubitization framework this paper builds on; the standard-form encoding abstraction here is exactly the Low-Chuang setup, with THC providing the PREPARE oracle
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]] — QSVT framework; the qubitized chemistry walk here is an instance of the block-encoding + polynomial-transform paradigm; the [[Block-Encoding Composition Algebra]] rules govern how the Givens rotations compose with the diagonal SELECT
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes]] — Foundation: qubitization + circuit primitives
- [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes]] — Diagonal Coulomb template (dual basis)
- [[Quantum Simulation of Electronic Structure with Linear Depth and Connectivity (Kivlichan, McClean et al 2018) — Paper Notes]] — Givens rotation and swap network primitives
- [[Low Rank Representations for Quantum Simulation of Electronic Structure (Motta, Babbush, Chan et al 2018) — Paper Notes]] — Introduced [[Double Factorization of Two-Electron Integrals|double factorization]] of two-electron integrals; THC can be seen as an alternative factorization that achieves diagonal Coulomb structure more directly, with comparable or smaller $\lambda$-norm
- [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes]] — Direct predecessor for arbitrary-basis qubitization; introduces [[QROAM (Space-Time Tradeoff for QROM)|QROAM]] and [[Measurement-Based QROM Uncomputation|measurement-based uncomputation]] reused here; single factorization approach gives $\widetilde{O}(N^{3/2}\lambda)$ which THC improves to $\widetilde{O}(N\lambda_\zeta)$
- [[Compressing Many-Body Fermion Operators under Unitary Constraints (Rubin, Lee, Babbush 2021) — Paper Notes]] — Related tensor factorization / compression work
- [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes]] — Competing first-quantized approach
- [[Quantum Simulation of Chemistry with Sublinear Scaling in Basis Size (Babbush, Berry, McClean, Neven 2019) — Paper Notes]] — First-quantized plane-wave approach
- [[Postponing the Orthogonality Catastrophe (Tubman, Mejuto-Zaera, Babbush et al 2018) — Paper Notes]] — QPE initial state preparation
- [[Compilation of Fault-Tolerant Quantum Heuristics for Combinatorial Optimization (Sanders, Berry, Babbush et al 2020) — Paper Notes|Sanders, Berry, Babbush et al 2020]]
- [[Discontinuous Galerkin Discretization for Quantum Simulation of Chemistry (McClean, Babbush, Lin et al 2019) — Paper Notes|McClean, Babbush, Lin et al 2019]]
- [[Efficient and Noise Resilient Measurements for Quantum Chemistry on Near-Term Quantum Computers (Huggins, McClean, Rubin, Babbush et al 2021) — Paper Notes|Huggins, McClean, Rubin, Babbush et al 2021]]
- [[Reliably Assessing the Electronic Structure of Cytochrome P450 (Goings, Babbush, Rubin et al 2022) — Paper Notes]] — Applies THC qubitization to CYP P450 active spaces; confirms $O(N^{2.5})$ Toffoli scaling on a pharmaceutically relevant system; introduces [[L1-Regularized THC Optimization from CP3|improved THC optimization]] with CP3 initial guess
- [[Efficient Quantum Computation of Molecular Forces and Other Energy Gradients (O'Brien, Babbush et al 2022) — Paper Notes]] — Differentiates the THC factorisation to block-encode force operators; shows circuit cost grows by $\sim\sqrt{2}\times$ and $\lambda_F$ by $\leq 2\times$ vs. $\lambda_H$; force estimation is a natural next step after energy estimation
- [[Analyzing Prospects for Quantum Advantage in Topological Data Analysis (Berry, Su, Babbush et al 2024) — Paper Notes|Berry, Su, Babbush et al 2024]]
- [[Nearly Optimal Quantum Algorithm for Estimating Multiple Expectation Values (Huggins, Wan, McClean, Babbush et al 2022) — Paper Notes|Huggins, Wan, McClean, Babbush et al 2022]]
- [[Quantum Simulation of Realistic Materials in First Quantization Using Non-Local Pseudopotentials (Berry, Rubin, Babbush et al 2024) — Paper Notes|Berry, Rubin, Babbush et al 2024]]
- [[Rapid Initial State Preparation for the Quantum Simulation of Strongly Correlated Molecules (Berry, Tong, Babbush, Rubin et al 2024) — Paper Notes]] — Closes the loop on FeMoCo resource estimation by addressing MPS state preparation and QPE with imperfect overlap; total cost $7.3 \times 10^{10}$ Toffolis (THC, 95% CI) is only $2.3\times$ above the perfect-overlap estimates from this paper; introduces [[Number Operator Symmetry Shifting for LCU 1-Norm Reduction|symmetry shifting]] that reduces $\lambda_{\text{THC}}$ from 1201.5 to 781.8

### Related trick cards
- [[Qubitization (Quantum Walk for Spectral Encoding)]] — The simulation framework
- [[Coherent Alias Sampling for PREPARE]] — PREPARE oracle construction
- [[QROM (Quantum Read-Only Memory)]] — Data loading primitive
- [[QROAM (Space-Time Tradeoff for QROM)]] — Improved QROM with space-time tradeoff from [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes|Berry et al. 2019]]
- [[Measurement-Based QROM Uncomputation]] — $M$-independent uncomputation from [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes|Berry et al. 2019]]
- [[Unary Iteration]] — SELECT oracle primitive
- [[Givens Rotation Slater Determinant Preparation]] — Basis rotation method
- [[Fermionic Swap Network]] — Alternative basis change
- [[Plane-Wave Dual Basis]] — The diagonal Coulomb structure that THC replicates
- [[THC Non-Orthogonal Diagonal Coulomb Representation]] — New trick from this paper
- [[QROM-Loaded Givens Rotation Networks]] — New trick from this paper
- [[Rotation Angle Coarse-Graining for Block Encodings]] — New trick from this paper

---

## Assessment

This paper is a genuine advance, not just an incremental improvement. The idea of using THC to diagonalize the Coulomb operator in a non-orthogonal basis, then sidestepping the non-orthogonality via per-term basis rotations, is elegant. It's the kind of construction where the final result looks inevitable, but getting there required insight.

The practical impact is real: for FeMoCo, the gap between "conceivable on a future quantum computer" and "actually worth building" narrows measurably. Going from $10^{10}$ to $5 \times 10^9$ Toffolis and from 3,725 to 2,142 logical qubits (vs. double factorization) is the kind of constant-factor improvement that matters when you're trying to fit inside a surface code budget. The surface code compilation analysis (4M physical qubits, 4 days) puts this firmly in the "ambitious but not absurd" range for a mid-term fault-tolerant machine.

The asymptotic advantage is even more important for the long game: $\widetilde{O}(N)$ per walk step in an arbitrary basis is optimal up to logarithmic factors. Prior methods that achieve this scaling require bases that diagonalize the Coulomb operator (plane-wave dual), which demand many more qubits to reach chemical accuracy for molecules. THC bridges this gap.

The paper also does the community a service by carefully reanalyzing and correcting prior algorithms (Berry et al., von Burg et al.), making the comparison table in Table 3 the most reliable resource estimate compilation available as of publication. The qDRIFT analysis in Appendix D, showing that mean-square-error cost is $10^{18}\times$ more expensive than qubitization for FeMoCo, is a useful sanity check on randomized methods.

Remaining open questions: Can THC rank be rigorously bounded as $O(N)$ for generic molecular systems? Can the classical THC fitting be made cheaper than $O(N^4)$? And — the big one — how does this approach compare to first-quantized qubitization (Su, Berry et al. 2021) for large molecules where $N \gg \eta$?

**Update:** [[Fault-Tolerant Quantum Simulation of Materials Using Bloch Orbitals (Rubin, Berry, Babbush et al 2023) — Paper Notes|Rubin, Berry, Babbush et al. (2023)]] extends the THC qubitization framework to periodic systems via Bloch orbitals ([[k-Point THC Factorization (Bloch Orbital THC)|k-THC]]). The periodic THC shows no asymptotic speedup from translational symmetry (unary iteration is the cost floor), and the classical cost of reoptimizing k-THC factors limits it to small k-meshes. DF turns out to be more practical for periodic systems.

**Update:** [[Is There Evidence for Exponential Quantum Advantage in Quantum Chemistry (Lee, Babbush, Chan et al 2022) — Paper Notes|Lee, Babbush, Chan et al. (2022)]] — several co-authors overlap — directly questions whether FeMoCo-class problems actually *need* a quantum computer. The resource estimates here are impressive, but the EQA paper argues that classical heuristics (DMRG, coupled cluster) may achieve comparable accuracy at polynomial cost. This doesn't invalidate the resource estimation work, but it shifts the goalposts: the value of these circuits may lie in polynomial rather than exponential advantage.

**Update:** [[Expressing and Analyzing Quantum Algorithms with Qualtran (Harrigan, Khattar, Babbush, Rubin 2024) — Paper Notes|Harrigan, Khattar, Babbush, Rubin (2024)]] — the THC block encoding from this paper is implemented as a Qualtran bloq; FeMoCo resource estimates reproduced (17,150 Toffoli block encoding cost vs 16,891 in OpenFermion, slight discrepancy from primitive implementation differences).

**Update:** [[Fast Quantum Simulation of Electronic Structure by Spectrum Amplification (Low, King, Berry, Babbush, Somma, Rubin 2025) — Paper Notes|Low, King, Berry et al. (2025)]] — introduces [[DFTHC Factorization|DFTHC]], which generalizes both DF and THC via a three-parameter $(R, B, C)$ ansatz. Combined with [[SOS Spectral Amplification|spectrum amplification]], this achieves $9.99 \times 10^8$ Toffolis for FeMoCo-76 — a $4.3\times$ improvement over THC+BLISS. The THC factorization can be seen as the special case $(R, B, C) = (1, N, N)$ of DFTHC.

See [[FeMoCo Resource Estimation Timeline]] for how this estimate fits in the broader progression.
