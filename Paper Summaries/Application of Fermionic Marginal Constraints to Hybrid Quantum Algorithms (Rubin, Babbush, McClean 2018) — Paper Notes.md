> **Source:** Nicholas C. Rubin, Ryan Babbush, Jarrod McClean, *Application of fermionic marginal constraints to hybrid quantum algorithms*, arXiv:1801.03524, New J. Phys. **20**, 053020 (2018)
> **Links:** [arXiv](https://arxiv.org/abs/1801.03524) · [Journal](https://doi.org/10.1088/1367-2630/aab919)
> **Tags:** #VQE #measurement #RDM #n-representability #NISQ #quantum-chemistry #variance-reduction #error-mitigation

---

## The computational problem

The problem is measurement efficiency in hybrid classical–quantum algorithms. When running [[Variational Quantum Eigensolver (VQE)|VQE]] or similar hybrid algorithms on a quantum device, you estimate the ground-state energy by measuring many Pauli terms:

$$E = \langle H \rangle = \sum_\ell w_\ell \langle H_\ell \rangle$$

For a molecular Hamiltonian with $N$ spin-orbitals this has $O(N^4)$ terms. Each requires repeated state preparations and measurements, with shot noise costing $O(1/\varepsilon^2)$ samples per term. For medium molecules, this measurement overhead is the practical bottleneck.

The paper asks: can the algebraic structure of fermionic quantum states — specifically the **$n$-representability conditions** on reduced density matrices (RDMs) — reduce this cost? And can the same structure help restore physicality when state preparation is corrupted by noise?

---

## What the paper does

Rubin, Babbush, and McClean translate the classical quantum chemistry literature on **fermionic marginals** and **$n$-representability** into the language of quantum computation, then derive two practical tools for near-term hardware:

1. **Variance reduction via linear constraints:** Re-express $H$ using known zero-valued linear combinations of RDM elements (constraints that hold for any physical fermionic state) to reduce the effective spectral norm $\Lambda = \sum_\ell |w_\ell|$, which directly bounds the total sample count. The optimal constraint coefficients are found by solving a linear program. They show $> 10 \times$ reduction in $\Lambda^2$ (hence sample count) for medium-sized chemical systems.

2. **Physicality restoration via 2-RDM projection:** When hardware noise corrupts a measured 2-RDM, project it back onto the set of approximately $n$-representable matrices using PSD projection, iterative alternating projection across the 2D/2Q/2G hierarchy, or SDP-based reconstruction. Demonstrated on a 4-qubit H₂ system under three distinct 1-qubit error channels.

The paper also proves a concentration-of-measure result showing that Haar-random states have trivially diagonal RDMs — a formal argument that structured ansätze are necessary for meaningful variational quantum chemistry.

---

## The algorithm / construction

### Fermionic marginals: definitions

For a fermionic system with $m$ spin-orbitals and $n$ electrons, the **$p$-body reduced density matrix** ($p$-RDM) is:

$${}^p D^{i_1 \ldots i_p}_{j_1 \ldots j_p} = \langle \Psi | a^\dagger_{i_1} \cdots a^\dagger_{i_p} a_{j_p} \cdots a_{j_1} | \Psi \rangle$$

The molecular energy is a linear functional of just the 1- and 2-RDM:

$$E = \sum_{ij} h_{ij} {}^1 D^i_j + \frac{1}{2}\sum_{ijkl} g_{ijkl} {}^2 D^{ij}_{kl}$$

**The 2-RDM has dimension $\binom{m}{2}^2 \approx m^4/4$** — for $m=10$ orbitals that's $\sim 2000$ elements; for $m=50$ it's $\sim 1.5 \times 10^6$.

Key objects in the 2-RDM hierarchy:
- ${}^2D$: two-particle RDM (electrons)
- ${}^2Q$: two-hole RDM (obtained from ${}^2D$ by particle-hole transformation)
- ${}^2G$: particle-hole RDM (one electron, one hole)
- ${}^1D$: 1-RDM (obtained by contraction of ${}^2D$)

### $n$-representability conditions

A matrix is $n$-representable if it arises from an actual $n$-electron quantum state. Necessary (but not sufficient for $p \geq 2$) conditions:
- **Positivity:** ${}^2D \succeq 0$, ${}^2Q \succeq 0$, ${}^2G \succeq 0$ (DQG conditions)
- **Trace:** $\text{Tr}[{}^2D] = n(n-1)$, $\text{Tr}[{}^2Q] = (m-n)(m-n-1)$, $\text{Tr}[{}^2G] = n(m-n+1)$
- **Linear maps:** ${}^2D \to {}^1D$ by contraction; ${}^2D \to {}^2Q$ via particle-hole exchange; ${}^2D \to {}^2G$ via mixed exchange

These give a large set of **linear equality constraints** on the Hamiltonian coefficients when expressed in the RDM basis. Since the constraints evaluate to zero on any physical state, adding multiples of them to $H$ doesn't change $\langle H \rangle$ but can reduce $\sum_\ell |w_\ell|$.

### Algorithm A: optimal measurement allocation

For $H = \sum_\ell w_\ell H_\ell$ with $H_\ell^2 = 1$, measuring each term $M_\ell$ times gives estimator variance:

$$\varepsilon^2 = \sum_\ell \frac{w_\ell^2 \sigma_\ell^2}{M_\ell}, \quad \sigma_\ell = \sqrt{1 - \langle H_\ell \rangle^2}$$

Minimizing total samples $M = \sum_\ell M_\ell$ subject to fixed $\varepsilon^2$ via Lagrange multipliers gives:

$$M_\ell \propto |w_\ell| \sigma_\ell$$

Total samples bounded by:

$$M \leq \left(\frac{\Lambda}{\varepsilon}\right)^2, \quad \Lambda = \sum_\ell |w_\ell|$$

This formalizes the intuitive $M_\ell \propto |w_\ell|$ allocation from earlier VQE work.

### Algorithm B: variance reduction via LP (main result)

The $n$-representability constraints give linear equalities $C_k = \sum_\ell c_{k\ell} H_\ell = 0$ for any physical state. Adding these with coefficients $\beta_k$ to $H$ leaves the expectation unchanged:

$$\tilde{H} = H + \sum_k \beta_k C_k, \quad \langle \tilde{H} \rangle = \langle H \rangle \text{ for any physical state}$$

The modified $\tilde{\Lambda} = \sum_\ell |\tilde{w}_\ell|$ can be much smaller. Optimally choosing $\beta$ minimizes $\tilde{\Lambda}$:

$$\beta^* = \arg\min_\beta \|v_H - C^T \beta\|_1$$

where $v_H$ is the vector of original coefficients. This is an **$\ell_1$ minimization** — a linear program:

$$\min_{\beta, q} \mathbf{1}^T q \quad \text{s.t.} \quad q \geq v_H - C^T\beta, \quad q \geq -(v_H - C^T\beta)$$

Solvable in polynomial time. Plug $\tilde{w}_\ell = w_\ell + \sum_k \beta^*_k c_{k\ell}$ into the optimal allocation.

**Constraint types used:** hermiticity, trace relations, contractions ${}^2D \to {}^1D$, ${}^2D \to {}^2Q$, ${}^2D \to {}^2G$, and first-order relations within each matrix. The paper works with the Hamiltonian written in the RDM basis (not the Pauli basis directly) and uses the RDM linear structure.

### Algorithm C: PSD projection for physicality restoration

Given a noisy measured 2-RDM:
1. Hermitize: ${}^2D_s = ({}^2D_\text{meas} + {}^2D_\text{meas}^\dagger)/2$
2. Eigendecompose: ${}^2D_s = U \text{diag}(\lambda_i) U^\dagger$
3. Clip: $\lambda_i' = \max(\lambda_i, 0)$, adjusted to enforce $\sum \lambda_i' = n(n-1)$
4. Reconstruct: ${}^2D_\text{proj} = U \text{diag}(\lambda_i') U^\dagger$

Cost: $O(m^6)$ naively (eigendecomposition of an $m^2 \times m^2$ matrix). Cheap for small systems.

### Algorithm D: iterative projection across DQG hierarchy

More sophisticated version — alternately projects across the 2D, 2Q, 2G PSD cones:
1. Hermitize and PSD-project ${}^2D$ to fixed trace $n(n-1)$
2. Map ${}^2D \to {}^2Q$ via particle-hole; PSD-project to fixed trace $(m-n)(m-n-1)$
3. Map ${}^2Q \to {}^2G$; PSD-project to fixed trace $n(m-n+1)$
4. Map back to ${}^2D$; repeat until convergence

Convergence criterion: largest negative eigenvalue across ${}^2D$, ${}^2Q$, ${}^2G$ below $10^{-7}$. No correctness guarantee, but fast and works well when noise is moderate.

### Algorithm E: SDP reconstruction

Full SDP formulation: minimize $\|{}^2D_\text{recon} - {}^2D_\text{meas}\|_F^2$ subject to all DQG positivity constraints, trace constraints, and linear maps. Frobenius minimization is cast as a semidefinite constraint:

$$\begin{pmatrix} I & E \\ E^\dagger & F \end{pmatrix} \succeq 0 \implies F \succeq E^\dagger E \implies \text{Tr}[F] \geq \|E\|_F^2$$

Minimize $\text{Tr}[F]$. Polynomial time but high practical cost; best suited for small systems or when exact representability is required.

### Concentration of measure result

Using Lévy's lemma: for a Lipschitz function $f$ on the unit sphere of $\mathbb{C}^{d}$ with Lipschitz constant $\eta$,

$$\Pr\left[|f - \mathbb{E}[f]| \geq \varepsilon\right] \leq 2 \exp\left(-\frac{(d-1)\varepsilon^2}{2\eta^2}\right)$$

Applied to $f = {}^1D^i_j$: for Haar-random states on $m$ spin-orbitals, diagonal 1-RDM elements concentrate to $1/2$ (for all-occupation-number-random states) or $n/m$ (for fixed particle number), with exponential tails. Off-diagonal elements concentrate to zero. The entire 1-RDM concentrates to a scalar multiple of identity as $m \to \infty$.

**Implication:** random circuits or Haar-random states are useless for chemistry. The 1-RDM and 2-RDM encode no correlation information for large systems unless the state is structured. This is a formal argument for why the ansatz matters in VQE.

---

## Key results

**Theorem (optimal sample bound):** For $H = \sum_\ell w_\ell H_\ell$ with $H_\ell^2 = I$, the minimum total samples $M$ to achieve energy precision $\varepsilon$ satisfies:

$$M \leq \left(\frac{\Lambda}{\varepsilon}\right)^2, \quad \Lambda = \sum_\ell |w_\ell|$$

achieved by the allocation $M_\ell \propto |w_\ell|\sigma_\ell$.

**Main empirical result:** Using DQG $n$-representability constraints in the LP variance reduction (Algorithm B), the paper demonstrates $> 10 \times$ reduction in $\Lambda^2$ (hence in required sample count) for representative medium-sized chemical systems. For example, for systems with 10–30 spin-orbitals, $\Lambda^2$ drops from $O(10^3)$–$O(10^5)$ to $O(10^2)$–$O(10^4)$ Hartree$^2$, implying an order-of-magnitude fewer shots for fixed precision.

**Physicality restoration:** Demonstrated on 4-qubit H₂ with three error models (dephasing, amplitude damping, depolarizing). The iterative projection method successfully restores the correct energy curve in all three cases at error rates where the bare measured curve has visible deviations. The simple PSD projection also works, but the iterative DQG method handles larger deviations.

**Concentration result:** 1-RDM elements concentrate to their Haar-random mean with Gaussian tails, with width $\sim 1/\sqrt{m}$. For $m = 50$ spin-orbitals, fluctuations in 1-RDM elements are $< 0.01$ — the 1-RDM is essentially diagonal for random states.

---

## Comparison with prior work

| Approach | What it does | Measurement cost | Constraint type |
|---|---|---|---|
| Naive Pauli estimation | Measure each $P_\gamma$ independently | $O(N^4/\varepsilon^2)$ | None |
| Commuting group partitioning | Measure mutually commuting Paulis together | $O(N^3/\varepsilon^2)$ (empirical) | Commutativity |
| **This paper (LP reduction)** | **Re-weight Hamiltonian via $n$-rep constraints** | **$> 10\times$ reduction in $\Lambda^2$** | **Fermionic physics** |
| Classical SDP (variational 2-RDM) | Compute ground-state energy from 2-RDM alone | Classical; $O(m^9)$ SDP | Full N-rep hierarchy |

The connection to classical variational 2-RDM theory (Garrod, Percus, Erdahl, Mazziotti) is made explicit: the $n$-representability constraints used here are the same DQG conditions exploited classically, now applied to reduce quantum measurement cost rather than to compute energies directly.

Prior work on hybrid algorithms (McClean et al. 2016, O'Malley et al. 2016) used ad hoc $M_\ell \propto |w_\ell|$ allocation without proving it's optimal. This paper proves the optimality and shows how physics-aware constraints push $\Lambda$ much lower.

---

## Limits / caveats

- The variance reduction is empirical — no worst-case guarantee on how much $\Lambda$ decreases. Systems with very little symmetry may see smaller gains.
- The LP approach assumes the Hamiltonian is expressed in the 2-RDM/Pauli basis and that the relevant $n$-representability constraints can be enumerated. For larger systems (hundreds of orbitals), constructing and solving the LP itself becomes non-trivial, though it's still polynomial time.
- Physicality restoration (Algorithms C–E) assumes measurement errors are small enough that the corrupted 2-RDM is still "close" to physical. For high error rates, the projection may converge to a physically valid 2-RDM that doesn't correspond to the actual quantum state.
- The DQG conditions are necessary but not sufficient for $n$-representability. There exist matrices satisfying DQG that are not $n$-representable. The full T2 conditions and higher constraints are computationally harder to enforce.
- The concentration-of-measure result applies to Haar-random states on the full Hilbert space. Fixed-particle-number subspaces and hardware-efficient ansätze give stronger concentration on a restricted manifold — the argument still holds qualitatively.
- Open question (stated in paper): can the variance reduction technique extend to QAOA or quantum spin Hamiltonians? Spin marginals lack the fermionic antisymmetry structure, so the DQG constraints don't directly apply.

---

## Reusable ideas

1. **[[Variance Reduction via N-Representability Constraints (LP Method)]]** — reduce VQE measurement cost by solving an LP over fermionic $n$-rep constraints to minimize $\sum|w_\ell|$.
2. **[[2-RDM Physicality Restoration by PSD Projection]]** — iterative projection across the DQG cone hierarchy to restore physical validity of noisy measured RDMs.
3. **Optimal Pauli shot allocation** — already covered in [[Pauli Expectation Value Estimation]]; this paper proves the $M_\ell \propto |w_\ell|\sigma_\ell$ formula is optimal via Lagrange multipliers. No separate trick card needed; the [[Pauli Expectation Value Estimation]] card should be updated to note this.
4. **Concentration of measure for fermionic RDMs** — Lévy's lemma applied to RDM elements shows random ansätze are trivially diagonal; this is reusable as a general argument for why structured ansätze are necessary.

---

## References within this paper

Key citations:

- **[[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes|Peruzzo et al. (2014)]]** — original VQE proposal; referenced as motivation. No vault note yet.
- **McClean, Romero, Babbush, Aspuru-Guzik (2016)** — derives the VQE framework including Pauli decomposition and suggests $M_\ell \propto |w_\ell|$ allocation without proving optimality. No vault note yet.
- [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes]] — first hardware VQE demonstration; uses the $M_\ell \propto |w_\ell|$ heuristic; this paper proves it optimal.
- [[Strategies for Quantum Computing Molecular Energies Using the UCC Ansatz (Romero, Babbush et al 2018) — Paper Notes]] — concurrent work from largely the same group; focuses on ansatz design rather than measurement.
- **Garrod and Percus (1964), Erdahl (1978), Mazziotti (2002–2007)** — classical variational 2-RDM and $n$-representability theory; the DQG conditions originate here.
- **Higham (1988), Bauer (1960s)** — PSD projection algorithms used in Algorithm C.
- **Lévy (1951), Milman (1986)** — Lévy's lemma for concentration of measure.
- **Farhi, Goldstone, Gutmann (2014)** — QAOA; mentioned as potential future application for spin-marginal constraints.

---

## Cross-links

### Related paper notes
- [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes]] — Original VQE proposal; this paper proves that the $M_\ell \propto |w_\ell|$ shot allocation Peruzzo suggested is optimal and provides physics-aware improvement
- [[The Theory of Variational Hybrid Quantum-Classical Algorithms (McClean-Romero-Babbush-Aspuru-Guzik 2015) — Paper Notes]] — Theory companion by the same group; suggests $M_\ell \propto |w_\ell|$ allocation without proving optimality; this paper provides the proof and shows $n$-rep constraints push $\Lambda$ much lower
- [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes]]
- [[Strategies for Quantum Computing Molecular Energies Using the UCC Ansatz (Romero, Babbush et al 2018) — Paper Notes]]
- [[Quantum Simulation of Electronic Structure with Linear Depth and Connectivity (Kivlichan, McClean et al 2018) — Paper Notes]]
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes]] — Same group, same year; fault-tolerant side of the same research programme
- [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes]] — Dual-basis measurement reduction is an orthogonal approach to reducing VQE shot cost
- [[Postponing the Orthogonality Catastrophe (Tubman, Mejuto-Zaera, Babbush et al 2018) — Paper Notes]] — Addresses the state-preparation side of VQE/QPE that this paper's measurement reduction complements
- [[Increasing the Representation Accuracy of Quantum Simulations of Chemistry without Extra Quantum Resources (Takeshita, Rubin, Babbush, McClean 2019) — Paper Notes]] — Uses RDM measurement infrastructure developed here as input to VQSE and orbital relaxation for basis-set correction
- [[Efficient and Noise Resilient Measurements for Quantum Chemistry on Near-Term Quantum Computers (Huggins, McClean, Rubin, Babbush et al 2021) — Paper Notes]] — Benchmarks the RDM constraint approach against [[Basis Rotation Grouping for VQE Measurement]]; shows the LP variance reduction doesn't substantially reduce actual variance despite tightening bounds
- [[Compressing Many-Body Fermion Operators under Unitary Constraints (Rubin, Lee, Babbush 2021) — Paper Notes|Rubin, Lee, Babbush 2021]]
- [[Decoding Quantum Errors with Subspace Expansions (McClean, Jiang, Rubin, Babbush, Neven 2019) — Paper Notes|McClean, Jiang, Rubin, Babbush, Neven 2019]]
- [[Discontinuous Galerkin Discretization for Quantum Simulation of Chemistry (McClean, Babbush, Lin et al 2019) — Paper Notes|McClean, Babbush, Lin et al 2019]]
- [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes|Berry, Gidney, Motta, McClean, Babbush 2019]]
- [[Unbiasing Fermionic Quantum Monte Carlo with a Quantum Computer (Huggins, Babbush et al 2021) — Paper Notes|Huggins, Babbush et al 2021]]
- [[Nearly Optimal Quantum Algorithm for Estimating Multiple Expectation Values (Huggins, Wan, McClean, Babbush et al 2022) — Paper Notes|Huggins, Wan, McClean, Babbush et al 2022]]

### Related trick cards
- [[Variational Quantum Eigensolver (VQE)]]
- [[Pauli Expectation Value Estimation]]
- [[Unitary Coupled Cluster (UCC) Ansatz]]
- [[Symmetry Reduction in Qubit Hamiltonians]]
- [[Variance Reduction via N-Representability Constraints (LP Method)]]
- [[2-RDM Physicality Restoration by PSD Projection]]
