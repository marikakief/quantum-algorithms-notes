> **Source:** Ian D. Kivlichan, Jarrod McClean, Nathan Wiebe, Craig Gidney, Alán Aspuru-Guzik, Garnet Kin-Lic Chan, Ryan Babbush, *Quantum Simulation of Electronic Structure with Linear Depth and Connectivity*, arXiv:1711.04789, Phys. Rev. Lett. **120**, 110501 (2018)
> **Links:** [arXiv](https://arxiv.org/abs/1711.04789) · [Journal](https://doi.org/10.1103/PhysRevLett.120.110501)
> **Tags:** #quantum-simulation #electronic-structure #Trotter #fermionic-swap-network #Slater-determinant #linear-connectivity #Jordan-Wigner #variational #phase-estimation

---

## The computational problem

Given the second-quantized electronic structure Hamiltonian

$$H = \sum_{p,q} T_{pq}\, a^\dagger_p a_q + \sum_p U_p\, n_p + \sum_{p \neq q} V_{pq}\, n_p n_q$$

(where $a^\dagger_p, a_p$ are fermionic creation/annihilation operators and $n_p = a^\dagger_p a_p$), implement a first-order Trotter step $e^{-iHt}$ on $N$ spin-orbitals, using a minimal linearly-connected qubit architecture, minimising both circuit depth and two-qubit gate count. Separately: prepare an arbitrary single Slater determinant (specified by a single-particle unitary $u \in U(N)$) on the same linear architecture, also minimising depth.

The Jordan–Wigner mapping is used throughout: qubit $p$ represents whether spin-orbital $p$ is occupied.

## What the paper does

The paper introduces the **fermionic swap network**, a circuit structure that implements a complete first-order Trotter step of $H$ in depth exactly $N$ using $N(N-1)/2$ two-qubit entangling gates — all on a linearly connected qubit chain. It also shows that arbitrary Slater determinants can be prepared in at most $N/2$ depth via parallel nearest-neighbour Givens rotations. Both results achieve or beat the resource costs of all prior approaches, with *less* connectivity than most of them require.

This paper is significantly more practical than the LCU-based approaches in [[Exponentially More Precise Quantum Simulation of Fermions in the CI Representation (Babbush et al 2018) — Paper Notes|Babbush et al. (2018)]] and [[Bounding the Costs of Quantum Simulation of Many-Body Physics in Real Space (Kivlichan, Wiebe, Babbush, Aspuru-Guzik 2017) — Paper Notes|Kivlichan et al. (2017)]]: Trotter + fermionic swap network is competitive for near-term hardware. The swap network result is likely optimal (they conjecture, but haven't proved, a matching lower bound).

## The algorithm / construction

### Jordan–Wigner setup

Under the Jordan–Wigner encoding, qubit $p$ stores the parity of orbital $p$. The canonical ordering $\sigma$ records which orbital is mapped to which qubit position — this ordering changes dynamically during the circuit.

### The fermionic simulation gate

For a pair of adjacent orbitals $p$ and $q$ (adjacent in the current canonical ordering), the **fermionic simulation gate** $F_t$ is:

$$F_t(p, q) = e^{-i V_{pq} n_p n_q t} \cdot e^{-i T_{pq} (a^\dagger_p a_q + a^\dagger_q a_p) t} \cdot f_{p,q}^{\text{swap}}$$

where $f_{p,q}^{\text{swap}}$ is the **fermionic swap** — the operator that exchanges orbitals $p$ and $q$ while preserving fermionic antisymmetry:

$$f_{p,q}^{\text{swap}} = 1 + a^\dagger_p a_q + a^\dagger_q a_p - a^\dagger_p a_p - a^\dagger_q a_q$$

It satisfies $f_{p,q}^{\text{swap}} a^\dagger_p (f_{p,q}^{\text{swap}})^\dagger = a^\dagger_q$, i.e., it genuinely swaps the orbital labels (not just the qubit states). When $q = p + 1$ in the JW ordering (which the network enforces at the moment of application), this is a 2-local qubit operation.

In the qubit basis $\{|00\rangle, |01\rangle, |10\rangle, |11\rangle\}$ for adjacent orbitals, the matrix of $F_t$ is:

$$F_t = \begin{pmatrix} 1 & 0 & 0 & 0 \\ 0 & -i\sin(T_{pq} t) & \cos(T_{pq} t) & 0 \\ 0 & \cos(T_{pq} t) & -i\sin(T_{pq} t) & 0 \\ 0 & 0 & 0 & -e^{-iV_{pq}t} \end{pmatrix}$$

This gate simultaneously implements the hopping, the density-density interaction, and the fermionic swap — three operations for the price of one two-qubit gate. Each $F_t$ can be compiled into at most 3 standard entangling gates (CNOT/CZ) plus single-qubit rotations.

### The swap network schedule

The key insight: run **odd-even transposition sort** (parallel bubble sort) on the orbital indices. This permutation schedule is:

- **Layer A (odd-even):** Apply $F_t$ between qubit positions $(2j+1, 2j+2)$ for $j = 0, 1, \ldots, \lfloor(N-2)/2\rfloor$
- **Layer B (even-odd):** Apply $F_t$ between qubit positions $(2j+2, 2j+3)$ for $j = 0, 1, \ldots, \lfloor(N-2)/2\rfloor$

Alternate A and B for a total of $N$ layers. After $N$ layers, the canonical ordering has reversed, and **every unordered pair of orbitals has been adjacent exactly once**. Since $F_t$ for pair $(p,q)$ is applied precisely when $(p,q)$ are adjacent, each interaction term is simulated exactly once — completing a first-order Trotter step.

Single-qubit rotations $e^{-i U_p n_p t}$ for the diagonal $U_p$ terms are applied in a single layer (they commute with everything diagonal in the orbital basis and can be inserted cheaply).

**Total gate count:** $N(N-1)/2$ fermionic simulation gates (one per ordered pair), $N$ depth.

```
Orbitals:  [0] [1] [2] [3] [4]   ← initial ordering in qubits
Layer 1A:   F(0,1)  F(2,3)       ← pairs (0,1) and (2,3) simulated
Layer 1B:       F(1,2)  F(3,4)   ← pairs (1,2) and (3,4) simulated
Layer 2A:   F(0,2)  ...          ← etc., permuted ordering
...                              ← continue for N layers total
```

### Second-order (symmetric) Trotter step

For a symmetric Trotter step of total time $2t$: run the first-order sequence for time $t$, but on the **final gate** double the interaction time to $2t$ and omit the fermionic swap. Then mirror the remaining gates in reverse order. This gives the standard $e^{-iHt/2} e^{-iHt/2}$ symmetrisation without additional gate overhead (the last gate bridges both halves).

### Slater determinant preparation via Givens rotations

To prepare a Slater determinant $U(u)|0\rangle^{\otimes N}$ (where $u \in U(N)$ is the single-particle unitary and $|0\rangle^{\otimes N}$ represents the occupied reference), decompose $u$ into nearest-neighbour Givens rotations via a parallel column-zeroing schedule.

A **Givens rotation** $G_{pq}(\theta, \phi)$ acts on orbitals $p, q$ as:

$$G_{pq} = \begin{pmatrix} \cos\theta & -e^{i\phi}\sin\theta \\ e^{-i\phi}\sin\theta & \cos\theta \end{pmatrix}$$

and is a nearest-neighbour two-qubit gate when $q = p+1$.

Full decomposition:
- $N(N-1)/2$ Givens rotations total (one per sub-diagonal element)
- With spin symmetry ($u$ block-diagonal in spin-up/spin-down): run spin-up and spin-down Givens sequences in parallel on alternate qubits → depth $2N-3$
- With particle-number symmetry (only $\eta$ particles): only $\eta(N-\eta)$ rotations needed → depth at most $\eta - 1 < N/2$
- Combined: depth $\leq N/2$ for arbitrary Slater determinants

See [[Givens Rotation Slater Determinant Preparation]] for the full technique.

### Hubbard model (Appendix A)

For the 2D Hubbard model on a $\sqrt{N} \times \sqrt{N}$ lattice, a modified swap-layer schedule (interleaving spin-up and spin-down qubits in a JW ordering tailored to the lattice) achieves Trotter step depth $O(\sqrt{N})$ on a linear qubit chain. This beats all prior approaches on a linear array and beats planar approaches in qubit count (they require qubit doubling; see caveat).

## Key results

**Theorem 1 (Trotter step cost):** A first-order Trotter step of the Hamiltonian $H$ (Eq. (1)) for $N$ spin-orbitals on a linearly connected qubit chain requires:
- Circuit depth: exactly $N$
- Two-qubit entangling gates: exactly $N(N-1)/2$

**Theorem 2 (Slater determinant preparation):** Arbitrary Slater determinants for $N$ spin-orbitals can be prepared with depth $\leq N/2$ on a linearly connected qubit chain, using $N(N-1)/2$ nearest-neighbour Givens rotations.

**Conjecture (optimality):** No explicit Trotter step of the general $H$ (Eq. (1)) can be implemented with fewer two-qubit entangling gates, even on an architecture with arbitrary connectivity. (Not proved; formal lower bound is an open problem.)

**Hubbard result:** For the 2D Hubbard model on a $\sqrt{N} \times \sqrt{N}$ lattice, depth $O(\sqrt{N})$ on a linear chain. Prior planar methods: $O(1)$ depth but require doubling qubit count.

## Comparison with prior work

| Method | Trotter-step depth | Two-qubit gates | Connectivity required |
|---|---|---|---|
| Naive JW (standard decomposition) | $O(N^2)$ | $O(N^4)$ | Linear (but many layers) |
| Prior swap-network approach ([40]) | $2N$ | $O(N^2)$ | Planar |
| **This work (fermionic swap network)** | **$N$** | **$N(N-1)/2$** | **Linear** |
| FFFT-based (plane wave basis) | $O(N \log N)$ | $O(N \log N)$ | Planar |

| Method | Slater-det. depth | Givens rotations | Connectivity |
|---|---|---|---|
| Wecker et al. [41] | $O(N^2)$ | $N^2$ | Arbitrary |
| FFFT (plane waves) | $O(N)$ | $O(N \log N)$ | Planar |
| **This work** | **$\leq N/2$** | **$\eta(N-\eta)$** | **Linear** |

The improvement is not just asymptotic — the constants are better too. Previous "swap network" approaches at planar connectivity needed 2N depth; this uses N depth with strictly less connectivity.

## Limits / caveats

- **Optimality conjecture unproved:** The gate-count optimality claim (no fewer than $N(N-1)/2$ gates for a general Trotter step) is stated as a conjecture. A formal lower bound is an open problem.
- **No Trotter error analysis:** The paper does not analyse Trotter error for this specific gate ordering. Whether this ordering is favourable or unfavourable for commutator-based error depends on the system. The authors recommend numerical study as future work.
- **Jordan–Wigner only:** The technique relies heavily on the JW canonical ordering changing via fermionic swaps. It doesn't obviously extend to Bravyi-Kitaev or other encodings.
- **Hubbard periodic boundary conditions:** The $O(\sqrt{N})$ Hubbard result doesn't handle periodic boundary conditions.
- **Planar qubit-doubling trade-off:** For the Hubbard model, the Verstraete-Cirac planar mapping achieves lower depth by doubling qubits. At moderate $N$, the constant factor matters.
- **Linear connectivity assumption is the point:** The result is specifically tailored to linear (1D nearest-neighbour) architectures. On all-to-all connected hardware the comparison is different — though even then, $N(N-1)/2$ two-qubit gates is a reasonable lower bound.

## Reusable ideas

1. [[Fermionic Swap Network]] — Odd-even transposition sort as a Trotter-step scheduling primitive: combine fermionic swap + Hamiltonian evolution into a single two-qubit gate per pair, achieving exactly $N$ depth and $N(N-1)/2$ gates on a linear chain.

2. [[Givens Rotation Slater Determinant Preparation]] — Decompose a single-particle unitary into nearest-neighbour Givens rotations in a parallel schedule; exploit spin and particle-number symmetry to achieve depth $\leq N/2$ on a linear chain.

## References within this paper

| Ref | What it is | Vault note? |
|---|---|---|
| Aspuru-Guzik et al. (2005), Science 309 | Original quantum chemistry simulation proposal; Trotter + phase estimation | No |
| Wecker, Hastings, Troyer (2015) | Prior Trotter-step analysis; $N^2$ Slater-det. gates [41] | No |
| Babbush et al. (2016), NJP | Second-quantized Taylor series simulation ($\tilde{O}(N^4\|H\|t)$) | No |
| [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes|Babbush et al. (2018)]], arXiv:1506.01029 | CI-representation first-quantized simulation | [[Exponentially More Precise Quantum Simulation of Fermions in the CI Representation (Babbush et al 2018) — Paper Notes]] |
| Kivlichan, Wiebe, Babbush, Aspuru-Guzik (2017) | Real-space simulation with truncated Taylor series; same group, different approach | [[Bounding the Costs of Quantum Simulation of Many-Body Physics in Real Space (Kivlichan, Wiebe, Babbush, Aspuru-Guzik 2017) — Paper Notes]] |
| O'Malley, Babbush et al. (2016) | First hardware VQE experiment; motivates need for practical Trotter circuits | [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes]] |
| [40] (prior swap network + planar connectivity) | Achieves 2N depth with planar connectivity; this work halves depth with less connectivity | No |
| [46] Verstraete & Cirac (2005) | Planar encoding of Hubbard model; $O(1)$ depth but doubles qubit count | No |
| Givens (1954) | Classical Givens rotations (QR decomposition); adapted here to the quantum/fermionic setting | No |

## Cross-links

### Paper notes
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]] — provides the commutator-scaling framework for Trotter error analysis; this paper explicitly notes the absence of Trotter error analysis for the swap-network ordering, which Childs et al. later supply in the general case
- [[Chemical Basis of Trotter-Suzuki Errors in Quantum Chemistry Simulation (Babbush-McClean-Wecker-Aspuru-Guzik-Wiebe 2015) — Paper Notes]] — earlier Babbush–McClean paper showing that operator-norm bounds overestimate Trotter cost by orders of magnitude; motivates why the fermionic swap network ordering (studied numerically in Kivlichan 2020) may have much lower real-world error than norm bounds predict
- [[Faster Digital Quantum Simulation by Symmetry Protection (Tran-Su-Childs-Wiebe 2021) — Paper Notes]] — symmetry-protection technique applicable to the Hubbard model Trotter step constructed here; inserting symmetry kicks between swap-network layers could reduce error without additional gate cost
- [[Improved Fault-Tolerant Quantum Simulation of Condensed-Phase Correlated Electrons via Trotterization (Kivlichan, Gidney, Babbush et al 2020) — Paper Notes]] — directly uses the fermionic swap network Trotter step as one of two competing circuit constructions; also compares its Trotter error norm to the split-operator approach
- [[Bounding the Costs of Quantum Simulation of Many-Body Physics in Real Space (Kivlichan, Wiebe, Babbush, Aspuru-Guzik 2017) — Paper Notes]] — different Kivlichan et al. paper (1608.05696); uses LCU/Taylor series for real-space simulation; complementary approach to the same group's goals
- [[Exponentially More Precise Quantum Simulation of Fermions in the CI Representation (Babbush et al 2018) — Paper Notes]] — LCU-based first-quantized simulation; asymptotically better precision scaling but not directly relevant to NISQ Trotter circuits
- [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes]] — hardware motivation; first VQE+Trotter experiment; needs efficient Trotter steps exactly like this paper provides
- [[Strategies for Quantum Computing Molecular Energies Using the UCC Ansatz (Romero, Babbush et al 2018) — Paper Notes]] — VQE strategies paper; the Slater determinant state preparation here is directly relevant as the initial state for UCC
- [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes]] — companion work (overlapping authors: Babbush, McClean, Neven) achieving low-depth Trotter steps via a different route: plane-wave dual basis + FFFT on planar lattice, targeting periodic systems; compare $O(N)$-depth Trotter step here (linear chain) with $O(N)$-depth step there (planar lattice, periodic Hamiltonian with $\Theta(N^2)$ terms)
- [[Postponing the Orthogonality Catastrophe (Tubman, Mejuto-Zaera, Babbush et al 2018) — Paper Notes]] — extends single-Slater-determinant preparation to the multi-determinant case; the Givens rotation technique from this paper is the single-determinant building block that paper builds upon
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes]] — fault-tolerant approach using qubitization; cites this paper's swap network for Trotter-based alternatives
- [[Application of Fermionic Marginal Constraints to Hybrid Quantum Algorithms (Rubin, Babbush, McClean 2018) — Paper Notes]] — addresses the measurement overhead for VQE that this paper's efficient Trotter circuits enable
- [[Quantum Simulation of Chemistry with Sublinear Scaling in Basis Size (Babbush, Berry, McClean, Neven 2019) — Paper Notes]] — first-quantized approach that avoids the second-quantized Trotter framework entirely; complementary regime
- [[Low Rank Representations for Quantum Simulation of Electronic Structure (Motta, Babbush, Chan et al 2018) — Paper Notes]] — Uses the [[Fermionic Swap Network]] and [[Givens Rotation Slater Determinant Preparation|Givens rotation]] circuits from this paper as the building blocks for [[Double Factorization of Two-Electron Integrals|double factorization]] Trotter steps; reduces per-step cost from $O(N^2)$ to $O(N^2 \log N / L)$ by exploiting low-rank integral structure
- [[Compressing Many-Body Fermion Operators under Unitary Constraints (Rubin, Lee, Babbush 2021) — Paper Notes]] — builds directly on the Givens rotation + swap network circuit primitives from this paper; uses them as the physical implementation of sum-of-squares operator decompositions for UCC compilation
- [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes]] — uses the [[Givens Rotation Slater Determinant Preparation|Givens rotation]] technique from this paper as [[QROM-Loaded Givens Rotation Networks|QROM-loaded basis rotations]] within the THC qubitization SELECT oracle; the non-orthogonal THC basis changes are implemented as coherently controlled Givens rotation networks
- [[Efficient and Noise Resilient Measurements for Quantum Chemistry on Near-Term Quantum Computers (Huggins, McClean, Rubin, Babbush et al 2021) — Paper Notes]] — Uses this paper's [[Givens Rotation Slater Determinant Preparation|Givens rotation network]] as the measurement circuit primitive for [[Basis Rotation Grouping for VQE Measurement]], applying a basis change before computational-basis measurement
- [[Discontinuous Galerkin Discretization for Quantum Simulation of Chemistry (McClean, Babbush, Lin et al 2019) — Paper Notes]] — Generalizes the [[Fermionic Swap Network]] to [[Block-Diagonal Swap Network for Trotter Steps|block-diagonal Hamiltonians]], achieving Trotter-step depth $O(N_b n_\kappa^3)$ that interpolates between this paper's $O(N)$ (diagonal) and $O(N^3)$ (general) extremes
- [[Improved Fault-Tolerant Quantum Simulation of Condensed-Phase Correlated Electrons via Trotterization (Kivlichan, Gidney, Babbush et al 2020) — Paper Notes|Kivlichan, Gidney, Babbush et al 2020]]
- [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes|Berry, Gidney, Motta, McClean, Babbush 2019]]
- [[Unbiasing Fermionic Quantum Monte Carlo with a Quantum Computer (Huggins, Babbush et al 2021) — Paper Notes|Huggins, Babbush et al 2021]]
- [[Efficient Quantum Computation of Molecular Forces and Other Energy Gradients (O'Brien, Babbush et al 2022) — Paper Notes|O'Brien, Babbush et al 2022]]
- [[Matchgate Shadows for Fermionic Quantum Simulation (Wan, Huggins, Lee, Babbush 2022) — Paper Notes|Wan, Huggins, Lee, Babbush 2022]]
- [[Quantum Simulation of Exact Electron Dynamics Can Be More Efficient Than Classical Mean-Field Methods (Babbush, Huggins, Berry et al 2023) — Paper Notes|Babbush, Huggins, Berry et al 2023]]

### Trick cards
- [[Fermionic Swap Network]] — the central technique of this paper
- [[Givens Rotation Slater Determinant Preparation]] — state prep technique
- [[Trotterized Time Evolution for Chemistry]] — what this paper improves on; the fermionic swap network gives a concrete, optimised circuit for the Trotter step
- [[Bravyi-Kitaev Transformation]] — alternative fermion encoding; this paper's trick is JW-specific, so BK is not used here
- [[Variational Quantum Eigensolver (VQE)]] — the fermionic swap network Trotter step and Slater determinant preparation are directly applicable as VQE subroutines
- [[Iterative Phase Estimation (Kitaev)]] — the efficient Trotter step also improves the per-step cost in phase estimation approaches
