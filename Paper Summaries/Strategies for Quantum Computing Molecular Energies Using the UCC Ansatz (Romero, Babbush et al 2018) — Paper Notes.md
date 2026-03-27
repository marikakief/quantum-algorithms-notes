> **Source:** Jonathan Romero, Ryan Babbush, Jarrod R. McClean, Cornelius Hempel, Peter J. Love, Alán Aspuru-Guzik, *Strategies for quantum computing molecular energies using the unitary coupled cluster ansatz*, arXiv:1701.02691, Quantum Science and Technology **4**, 014008 (2018)
> **Links:** [arXiv](https://arxiv.org/abs/1701.02691) · [Journal](https://doi.org/10.1088/2058-9565/aad3e4)
> **Tags:** #VQE #UCC #quantum-chemistry #variational #NISQ #gradient-estimation #active-space #circuit-depth

---

## The computational problem

Same base problem as [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes]]: approximate the ground-state energy of the second-quantized molecular Hamiltonian

$$H = \sum_{pq} h_{pq} a^\dagger_p a_q + \frac{1}{2}\sum_{pqrs} h_{pqrs} a^\dagger_p a^\dagger_q a_r a_s$$

to chemical accuracy ($\approx 1.59\times10^{-3}$ Hartree), using a near-term device without error correction.

The paper's specific focus: given that the [[Unitary Coupled Cluster (UCC) Ansatz|UCCSD ansatz]] is the chemically motivated choice for [[Variational Quantum Eigensolver (VQE)|VQE]], what are practical strategies to reduce its circuit depth and measurement overhead on real hardware?

## What the paper does

Introduces four strategies to make UCC-VQE more practical: (1) a Trotterized product-form circuit that factorizes UCC per-excitation exactly for single excitations and approximately for doubles, (2) a threshold-based prescreening of parameters using MP2 amplitudes that can cut parameter count roughly in half with negligible accuracy loss, (3) active space restriction (CAS-UCC) to reduce qubit and parameter counts for large molecules, and (4) an analytical gradient method that estimates energy gradients orders of magnitude more cheaply than numerical finite differences. Benchmarked numerically on a strongly correlated H4 system.

This is a practical/methodological paper rather than an asymptotic complexity result — it's squarely aimed at what you'd actually implement on a NISQ device circa 2017–2018.

## The algorithm / construction

### Overview

Standard VQE loop: prepare $|\psi(\vec{t})\rangle = U(\vec{t})|\Phi_0\rangle$, estimate $E(\vec{t}) = \langle H \rangle$ via [[Pauli Expectation Value Estimation|Pauli decomposition]], optimize $\vec{t}$ classically. The paper focuses on four improvements to the ansatz preparation and optimization steps.

### Strategy 1 — Trotterized UCC product circuit

The exact UCCSD unitary is $U = e^{T - T^\dagger}$ where $T = \sum_j t_j \tau_j$ sums over all single and double excitation operators. The full exponential is expensive to implement directly. Trotterize per excitation operator:

$$U_\text{Trot}(\vec{t}) = \left(\prod_j e^{t_j(\tau_j - \tau_j^\dagger)}\right)^\rho \tag{Eq. 21}$$

with $\rho$ Trotter repetitions. For each excitation term, $\tau_j - \tau_j^\dagger$ maps (via JW or BK) to a sum of Pauli strings:

$$\tau_j - \tau_j^\dagger = \frac{i}{2}\sum_k P_{jk} \tag{Eq. 23}$$

**Key structural insight (Appendix A):** For *single* excitations, all Pauli subterms $P_{jk}$ of the same excitation $\tau_j$ mutually commute. Therefore the Trotter factorization is *exact* for singles:

$$e^{t_j(\tau_j - \tau_j^\dagger)} = \prod_k e^{i t_j P_{jk}} \quad \text{(exact for singles)} \tag{Eq. 25}$$

For *double* excitations, subterms don't generally commute, so this is a first-order approximation.

**Empirical finding (H4 benchmark, Table I):** $\rho = 1$ or $\rho = 2$ achieves essentially the same non-parallelity error (NPE) and wavefunction overlap as the formally exact UCC unitary. Typical NPE $\approx 1$–$2$ kcal/mol for the trapezoidal PES path; wavefunction overlap $\approx 0.994$–$0.999$. The variational optimizer absorbs the Trotter error by adjusting the parameters. This is the practical justification for using $\rho = 1$ on hardware.

**Gate count scaling:**
- JW mapping: each excitation costs $O(N)$ CNOT gates; UCCSD has $O(N^2\eta^2)$ parameters → total $O(N^3\eta^2)$ gates
- [[Bravyi-Kitaev Transformation|BK mapping]]: each excitation costs $O(\log N)$ gates → total $O(N^2\eta^2 \log N)$ gates (asymptotically better)
- Ion-trap (Mølmer-Sørensen gates): $\approx 2$ entangling operations per variational parameter

### Strategy 2 — MP2 amplitude prescreening

The UCCSD parameter count is $O(N^2\eta^2)$ for doubles plus $O(N\eta)$ for singles. Many amplitudes are negligibly small in the weakly correlated regime. Prune parameters whose MP2 estimate falls below threshold $d$:

$$\theta^{ab}_{ij}{}^{\!\text{MP2}} = \frac{h_{ijba} - h_{ijab}}{\epsilon_i + \epsilon_j - \epsilon_a - \epsilon_b}, \quad \theta^a_i{}^{\!\text{MP2}} = 0$$

Retain excitation $j$ only if $|t_j^\text{MP2}| \geq d$. See [[MP2-Based Amplitude Prescreening for UCC]].

**H4 results (Table II):** At $d = 10^{-3}$, 26 of 52 parameters retained; max PES deviation $< 7\times10^{-4}$ kcal/mol (well within chemical accuracy). At $d = 10^{-2}$, 24 parameters retained; max deviation $\leq 0.07$ kcal/mol for parallel/trapezoidal paths. Parameter reduction also reduces energy evaluations by $\approx 3\times$ (from $\sim 3500$ to $\sim 1200$ for L-BFGS-B with trapezoidal path).

### Strategy 3 — Active space restriction (CAS-UCC)

For large molecules, partition spin-orbitals into an inactive set $I$ (always doubly occupied) and an active set $A$. Restrict the cluster operator to active-space excitations only:

$$T_A = \sum_{i=1}^{\eta_A} T^{(i)} \quad \text{(excitations within active space only)} \tag{Eq. 48}$$

The effective active-space Hamiltonian is obtained by averaging over the inactive reference:

$$\tilde{H}_A = \langle \Phi^I_0 | H | \Phi^I_0 \rangle \tag{Eq. 50}$$

Run VQE-UCC on $\tilde{H}_A$ with $\eta_A$ electrons in $N_A$ active orbitals. Parameter scaling drops to $O(\eta_A^2 N_A^2)$; qubit count drops from $N$ to $N_A$. See [[Active Space Restriction for VQE (CAS-UCC)]].

**Active space selection heuristics:** natural orbital occupancies from MP2 (choose orbitals with occupation $\notin \{0,2\}$); unrestricted HF natural orbitals (UNOs); entanglement-based selection from small DMRG runs.

### Strategy 4 — Analytical gradient estimation

For the product-of-Pauli-exponentials ansatz

$$U(\vec{t}) = \prod_j \prod_{k=1}^{N_j^S} e^{ic_{jk} t_j P_{jk}} \tag{Eq. 33}$$

the exact energy gradient is:

$$\frac{\partial E}{\partial t_j} = 2\sum_i h_i \sum_k c_{jk} \,\text{Im}\!\left\langle \Phi_0 \middle| V_{jk}^\dagger(\vec{t})\, O_i\, U(\vec{t}) \middle| \Phi_0 \right\rangle \tag{Eq. 36}$$

where $V_{jk}(\vec{t})$ is the same product ansatz with operator $P_{jk}$ inserted at position $jk$ in the product. Each imaginary overlap is measured with a Hadamard test: ancilla $|0\rangle \to (|0\rangle + |1\rangle)/\sqrt{2}$, controlled-$P_{jk}$ and controlled-$O_i$ on the state register, ancilla measured in the $Y$ basis. See [[Analytical Gradient Estimation for Variational Quantum Circuits]].

**Sampling cost comparison:**
- Analytical gradient for parameter $j$: $\tilde{m}_j \leq 4\left(\sum_i |h_i|\right)^2 / \tilde{\varepsilon}_j^2$ measurements (Eq. 44)
- Numerical central-difference gradient (step $\delta$): $\tilde{m}_j^\text{num} \approx 4\left(\sum_i |h_i|\right)^2 / (2\delta\,\tilde{\varepsilon}_j)^2$ measurements (Eq. 46–47)

Analytical is cheaper by a factor $(2\delta)^2$. Since finite-difference steps must be small ($\delta \ll 0.5$ for accuracy), analytical gradients win by orders of magnitude in measurement count.

**Numerical validation (Table III, H4, control error $\Delta\Theta = 0.01$):** Analytical gradient requires $\sim 26$–$33$ gradient calls vs $\sim 32$–$42$ for numerical gradient. Energy errors are comparable, suggesting analytical gradients converge faster per gradient call.

## Key results

**Gate count scaling (UCCSD, product-form circuit):**

$$\text{Gates} = \begin{cases} O(N^3 \eta^2) & \text{Jordan-Wigner} \\ O(N^2 \eta^2 \log N) & \text{Bravyi-Kitaev} \end{cases}$$

**Parameter count (UCCSD):** $(N-\eta)^2\eta^2 + (N-\eta)\eta \approx O(N^2\eta^2)$

**Trotterization finding:** Single Trotter step ($\rho = 1$) achieves wavefunction overlap $\approx 0.994$–$0.999$ with exact UCC for H4 PES. NPE (non-parallelity error) $\approx 1$–$2$ kcal/mol, within chemical accuracy for trapezoidal and parallel paths.

**MP2 prescreening:** $d = 10^{-3}$ threshold retains 50% of parameters with PES deviation $< 7\times10^{-4}$ kcal/mol (sub-chemical-accuracy error). Energy evaluation count reduces $\approx 3\times$ with L-BFGS-B optimizer.

**Sampling cost ratio (analytical vs numerical gradient):**

$$\frac{\tilde{m}_j^\text{num}}{\tilde{m}_j^\text{analytical}} \approx \frac{1}{(2\delta)^2} \gg 1 \quad \text{for } \delta \ll 0.5$$

For $\delta = 0.05$: ratio $\approx 100$, i.e., analytical gradient needs $\sim 100\times$ fewer samples.

## Comparison with prior work

| Paper | Contribution | Relation to this work |
|---|---|---|
| [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes|Peruzzo et al. (2014)]] | First VQE experiment (photonic, HeH⁺) | Proposed the VQE framework used here |
| O'Malley/Babbush et al. (2016) | First scalable VQE hardware demo | Uses UCC ansatz on hardware; this paper refines the classical strategies |
| Wecker, Hastings, Troyer (2015) | VQE resource estimates, Trotter for chemistry | Established circuit depth concerns motivating this work |
| Whitfield, Biamonte, Aspuru-Guzik (2011) | UCC circuit construction via JW | Baseline circuit cost this paper improves on |
| McClean et al. (2016) | Variational hybrid algorithm theory | Framework used throughout |

The earlier O'Malley et al. paper demonstrated VQE on hardware but used a single-parameter ansatz (H₂, one excitation) with $\theta$-scanning. This paper addresses the multi-parameter regime — what to do when there are $O(N^2\eta^2)$ parameters — and provides concrete tools to reduce that burden.

## Limits / caveats

- **H4 only:** All numerical benchmarks use a 4-hydrogen system in STO-6G. The MP2 prescreening effectiveness and Trotter error absorption may not hold for systems with strong multi-reference character or larger active spaces.
- **Barren plateaus:** Not discussed in this paper (it predates the 2018 barren plateau paper). For large circuits with random initialization, gradients vanish exponentially — making the gradient estimation methods moot. MP2 initialization partially mitigates this.
- **Classical optimizer comparison:** Table I and II use L-BFGS-B and COBYLA only. For larger systems, gradient-based methods require either the analytical gradient circuit or finite differences — both add quantum circuit overhead.
- **Trotter error absorption is empirical:** The claim that $\rho = 1$ suffices relies on the variational optimizer absorbing the Trotter error. No formal guarantee is given; the ansatz effectively becomes a different parameterized family that no longer exactly implements UCC. Whether it retains the expressiveness needed for chemical accuracy on larger, more correlated systems is not proven.
- **Analytical gradient circuit overhead:** The Hadamard test requires one ancilla qubit and controlled operations, which may be costly on near-term hardware with limited connectivity. Not analyzed for specific hardware architectures.
- **Measurement scaling not improved:** The total number of measurements to estimate $E$ to precision $\varepsilon$ still scales as $(\sum|h_i|)^2/\varepsilon^2 = O(N^8/\varepsilon^2)$ in the worst case. The analytical gradient reduces *gradient* measurement cost, not energy estimation cost.

## Reusable ideas

1. [[Unitary Coupled Cluster (UCC) Ansatz]] — Full UCCSD construction, Trotterized product form, and commutativity of single-excitation subterms (enabling exact factorization per excitation).

2. [[Variational Quantum Eigensolver (VQE)]] — The VQE framework applied with UCCSD; optimizer comparisons (L-BFGS-B vs COBYLA vs Nelder-Mead) and MP2 initialization strategies.

3. [[MP2-Based Amplitude Prescreening for UCC]] — Screen UCC excitation operators by MP2 amplitude magnitude before adding them to the ansatz. Reduces parameter count $\sim 2\times$ with sub-chemical-accuracy overhead.

4. [[Analytical Gradient Estimation for Variational Quantum Circuits]] — Hadamard-test-based analytical gradient for product-of-Pauli-exponentials ansätze. Reduces gradient sampling cost by $(2\delta)^2$ relative to numerical gradients.

5. [[Active Space Restriction for VQE (CAS-UCC)]] — Restrict UCC to an active orbital subspace, reducing qubit count and parameter scaling from $O(N^2\eta^2)$ to $O(N_A^2\eta_A^2)$.

6. [[Bravyi-Kitaev Transformation]] — BK mapping reduces gate cost per UCCSD parameter from $O(N)$ (JW) to $O(\log N)$.

## References within this paper

| Ref | What it is | Vault note? |
|---|---|---|
| [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes|Peruzzo et al. (2014)]], Nat. Commun. 5, 4213 | Original VQE experiment | No |
| O'Malley, Babbush et al. (2016), PRX 6, 031007 | First scalable hardware VQE; UCCSD on 2 qubits | [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes]] |
| McClean, Romero, Babbush, Aspuru-Guzik (2016), NJP 18, 023023 | Framework for variational hybrid algorithms | No |
| Wecker, Hastings, Troyer (2015), PRA 92, 042303 | Resource estimates for VQE; Trotter analysis | No |
| Whitfield, Biamonte, Aspuru-Guzik (2011), Mol. Phys. 109, 735 | UCC circuit via JW mapping | No |
| Seeley, Richard, Love (2012), JCP 137, 224109 | Bravyi-Kitaev transformation | No |
| Bravyi, Gambetta, Mezzacapo, Temme (2017), arXiv:1701.08213 | Tapering qubits with symmetry | No (see [[Symmetry Reduction in Qubit Hamiltonians]]) |
| Aspuru-Guzik et al. (2005), Science 309, 1704 | Original quantum chemistry QPE proposal | No |
| Romero et al. (2017), arXiv:1612.02058 | Quantum autoencoders (same first author) | No |

## Cross-links

### Paper notes
- [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes]] — Original VQE proposal; this paper provides the practical strategies needed to scale Peruzzo's framework beyond the single-parameter HeH⁺ demonstration
- [[The Theory of Variational Hybrid Quantum-Classical Algorithms (McClean-Romero-Babbush-Aspuru-Guzik 2015) — Paper Notes]] — Theory companion by the same group that proposes the analytical gradient approach developed in full here; this paper also benchmarks COBYLA and Powell as superior to Nelder-Mead
- [[Simulated Quantum Computation of Molecular Energies (Aspuru-Guzik-Dutoi-Love-Head-Gordon 2005) — Paper Notes]] — Original QPE quantum chemistry proposal; VQE+UCC (this paper) is the near-term alternative to that QPE approach
- [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes]] — Hardware demo that this paper's strategies are designed to scale beyond; established VQE+UCC as the near-term approach
- [[Exponentially More Precise Quantum Simulation of Fermions in the CI Representation (Babbush et al 2018) — Paper Notes]] — Parallel work on fault-tolerant simulation via CI representation; contrast: this paper targets NISQ, that paper targets long-term fault tolerance
- [[Quantum Simulation of Electronic Structure with Linear Depth and Connectivity (Kivlichan, McClean et al 2018) — Paper Notes]] — Provides efficient Slater determinant state preparation (depth $\leq N/2$ on linear chain) directly applicable as the reference state for UCC circuits; also provides an efficient Trotter step for the Trotterized UCC product form
- [[Application of Fermionic Marginal Constraints to Hybrid Quantum Algorithms (Rubin, Babbush, McClean 2018) — Paper Notes]] — Concurrent near-term work from the same group; addresses the measurement overhead that ansatz design alone doesn't solve; uses $n$-representability constraints to reduce shot count $> 10\times$
- [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes]] — Dual-basis measurement reduction (2 circuits instead of $O(N^4)$) is an alternative to the ansatz-level strategies explored here; both reduce the practical cost of VQE
- [[Postponing the Orthogonality Catastrophe (Tubman, Mejuto-Zaera, Babbush et al 2018) — Paper Notes]] — Addresses the initial-state quality question for QPE that becomes relevant when scaling beyond VQE; multi-determinant prep extends the Givens rotation single-determinant approach
- [[Increasing the Representation Accuracy of Quantum Simulations of Chemistry without Extra Quantum Resources (Takeshita, Rubin, Babbush, McClean 2019) — Paper Notes]] — VQSE and orbital relaxation address the basis incompleteness error left by the CAS-UCC strategy introduced here
- [[Compressing Many-Body Fermion Operators under Unitary Constraints (Rubin, Lee, Babbush 2021) — Paper Notes]] — directly improves UCC circuit compilation via sum-of-squares decomposition; achieves ~$4\times$ fewer circuit layers than analytical factorizations for the Trotterized UCC product form this paper introduced
- [[Low Rank Representations for Quantum Simulation of Electronic Structure (Motta, Babbush, Chan et al 2018) — Paper Notes]] — [[Double Factorization of Two-Electron Integrals|Double factorization]] of the uCC operator reduces Trotter step from $O(N^4)$ to $O(N^3)$ gates (fixed molecule, increasing basis); first application of low-rank integral structure to UCC circuit compilation
- [[Decoding Quantum Errors with Subspace Expansions (McClean, Jiang, Rubin, Babbush, Neven 2019) — Paper Notes|McClean, Jiang, Rubin, Babbush, Neven 2019]]
- [[Efficient and Noise Resilient Measurements for Quantum Chemistry on Near-Term Quantum Computers (Huggins, McClean, Rubin, Babbush et al 2021) — Paper Notes|Huggins, McClean, Rubin, Babbush et al 2021]]
- [[Unbiasing Fermionic Quantum Monte Carlo with a Quantum Computer (Huggins, Babbush et al 2021) — Paper Notes|Huggins, Babbush et al 2021]]
- [[Efficient Quantum Computation of Molecular Forces and Other Energy Gradients (O'Brien, Babbush et al 2022) — Paper Notes|O'Brien, Babbush et al 2022]]
- [[Nearly Optimal Quantum Algorithm for Estimating Multiple Expectation Values (Huggins, Wan, McClean, Babbush et al 2022) — Paper Notes|Huggins, Wan, McClean, Babbush et al 2022]]

### Trick cards
- [[Unitary Coupled Cluster (UCC) Ansatz]]
- [[Variational Quantum Eigensolver (VQE)]]
- [[Bravyi-Kitaev Transformation]]
- [[Symmetry Reduction in Qubit Hamiltonians]]
- [[Pauli Expectation Value Estimation]]
- [[MP2-Based Amplitude Prescreening for UCC]]
- [[Analytical Gradient Estimation for Variational Quantum Circuits]]
- [[Active Space Restriction for VQE (CAS-UCC)]]
