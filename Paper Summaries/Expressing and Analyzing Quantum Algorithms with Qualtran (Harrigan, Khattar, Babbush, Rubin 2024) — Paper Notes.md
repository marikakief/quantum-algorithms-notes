> **Source:** Harrigan, Khattar, Yuan, Peduri, Yosri, Malone, Babbush, Rubin, *Expressing and Analyzing Quantum Algorithms with Qualtran*, arXiv:2409.04643 (2024)
> **Links:** [arXiv](https://arxiv.org/abs/2409.04643) · [GitHub](https://github.com/quantumlib/Qualtran)
> **Tags:** #software #resource-estimation #compilation #block-encoding #qubitization #QROM #quantum-algorithms #fault-tolerant

---

## What this paper is

This is a software paper — it introduces **Qualtran**, an open-source Python library for expressing, composing, and analyzing fault-tolerant quantum algorithms. It is *not* a new algorithm paper. What it does is codify the constructions scattered across dozens of resource estimation papers (many from the same Google Quantum AI group) into a reusable, testable, composable software framework. The paper doubles as a thorough survey of the algorithmic primitives that underpin modern quantum resource estimation.

My assessment: Qualtran is the most serious attempt yet at turning the "prose + appendix tables" style of quantum resource estimation into something reproducible and extensible. If it succeeds at community adoption, it could standardize how constant-factor resource estimates are reported and compared. The paper itself is more reference manual than research contribution — the novelty is organizational, not algorithmic.

---

## The computational problem

There isn't one in the usual sense. The problem Qualtran addresses is *meta-algorithmic*: given a quantum algorithm described as hierarchically composed subroutines, automatically derive gate counts, qubit counts, and physical resource estimates. The key challenge is that modern fault-tolerant algorithms operate at the teraquop scale ($10^{12}$ qubit-operations), making manual resource analysis tedious, error-prone, and difficult to reproduce.

---

## Design of the framework

### Bloqs: the core abstraction

The fundamental data structure is the **bloq** — a quantum subroutine characterized by a name and a signature (named input/output registers with quantum data types). A bloq is defined by its **decomposition** into simpler bloqs, forming a directed acyclic graph (compute graph) where edges represent quantum data flow.

Key design choices:
- **Hierarchical composition** — each decomposition step involves only tens to thousands of sub-bloqs, never the full $10^9$-gate unrolling. This makes the classical runtime tractable.
- **Quantum data types** — registers carry types like `QInt(8)`, `QFxp(6, 4)`, etc. with arbitrary bit-widths. Bloqs compose only when wire types match.
- **Linear logic enforcement** — every quantum wire is used exactly once (no-cloning, no-deletion), enforced at composition time. This catches a class of bugs that plague circuit-level frameworks.
- **No implicit qubit pool** — allocations and deallocations are explicit bloqs. Combined with named registers, this avoids qubit-identity bugs.

### Analysis protocols

1. **Call graphs** — DAG of caller→callee relationships with call counts. Can propagate symbolic (SymPy) parameters. Bloqs can provide either a full decomposition (with data flow) or a lightweight call-graph decomposition (just callee lists + counts).

2. **Gate counting** — recursive summation over the call graph with caching. Two strategies:
   - `BloqCounts`: user-specified target gate set
   - `QECGatesCost`: preconfigured for surface-code-relevant gates (T, Toffoli, Clifford, rotations, measurements)

3. **Qubit counting** — hierarchical min-max estimate: assumes sequential callee execution, adds idle bystander qubits. Potentially overestimates but scales to huge algorithms.

4. **Tensor simulation** — contracts the tensor network of constituent bloqs via Quimb. Works for small instances (e.g., verified a 64-qubit 32-bit adder using only $2^6$ classical memory).

5. **Classical simulation** — for bloqs implementing reversible logic, propagate classical values through the Python annotations. Supports fuzz testing against random inputs.

---

## Standard library of primitives

This is where the paper gets technically interesting. Sections III A–G walk through the building blocks that modern resource estimation papers keep re-deriving:

### Rotations (§III A)

Three strategies for single-qubit Z rotations, with different resource profiles:

| Strategy | Bloq | Non-Clifford cost |
|---|---|---|
| Direct Clifford+T synthesis (Bocharov et al.) | `ZPow`, `Rz` | $\lfloor 1.149 \log_2(1/\varepsilon) + 9.2 \rfloor$ T gates |
| Programmable ancilla (Jones et al.) | `ZPowUsingProgrammedAncilla` | $n$ resource states + $n \cdot \lfloor 1.149 \log_2(n/\varepsilon) + 9.2\rfloor$ T gates |
| [[Phase Gradient State for Controlled Rotations\|Phase gradient]] kickback | `ZPowConstViaPhaseGradient` | $(b_\text{grad} - 2)$ Toffolis; $b_\text{grad}$ catalytic |

Also includes **Quantum Variable Rotation (QVR)** — phases each basis state $|x\rangle$ by $e^{2\pi i x}$ — via either per-qubit `ZPow` decomposition or quantum-quantum addition into a [[Phase Gradient State for Controlled Rotations|phase gradient state]]. Supports [[Hamming Weight Phasing for Equiangular Rotations|Hamming weight phasing]] as a cost function.

### Unary iteration and indexed operations (§III B)

Implements $U = \sum_{i=0}^{L-1} |i\rangle\langle i| \otimes V_i$ — the quantum analogue of a for-loop. This is the [[Unary Iteration]] construction from [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes|Babbush et al. (2018)]], costing $L - 1$ Toffolis with $O(\log L)$ clean ancillae. The paper also describes:
- Khattar & Gidney's improved construction using skew trees ($1.25L + O(\sqrt{L})$ Toffoli with dirty ancillae)
- Sanders et al.'s variable-spaced optimization for sparse index sets: $\min(2^S - 1, S \cdot |L|)$ Toffolis
- `ApplyLthBloq` as the user-facing API, supporting multi-dimensional arrays (nested for-loops)

### Quantum lookup tables (§III C)

Four implementations of $O_x: \sum_i \alpha_i |i\rangle|0\rangle \to \sum_i \alpha_i |i\rangle|x_i\rangle$:

| Bloq | Toffoli cost | Ancilla |
|---|---|---|
| [[QROM (Quantum Read-Only Memory)\|QROM]] | $N - 2$ | $\lceil\log_2 N\rceil$ clean |
| `SelectSwapQROM` (Low et al.) | $2\lceil N/2^k\rceil + 4b(2^k - 1)$ | $2^k b$ dirty |
| [[QROAM (Space-Time Tradeoff for QROM)\|QROAMClean]] (Berry et al.) | $\lceil N/2^k\rceil + b(2^k - 1)$ | $(2^k-1)b$ clean |
| `QROAMCleanAdjoint` (Berry et al.) | $\lceil N/2^k\rceil + (2^k - 1)$ | $2^k$ clean |

All support multi-dimensional classical datasets and symbolic cost analysis. The parameter $k$ trades ancilla qubits for Toffoli count, with optimal $k \approx \frac{1}{2}\log_2(N/b)$ for QROAMClean.

### State preparation (§III D)

Two variants:
1. **Arbitrary quantum state** — `StatePreparationViaRotations` (Low et al.): reduces to $n$ lookups of rotation angles followed by controlled rotations. Cost dominated by $N$ total data lookups of $b = \log_2(n/\varepsilon)$ bits each.
2. **Purified density matrix** — `StatePreparationAliasSampling` ([[Coherent Alias Sampling for PREPARE|coherent alias sampling]] from [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes|Babbush et al. 2018]]): single lookup of $N$ entries of bitsize $n + \mu$ followed by comparison and conditional swap. Cheaper than (1) and sufficient for LCU/qubitization PREPARE oracles.

Both have sparse variants reducing to dense via Ramacciotti et al. and Berry et al. respectively.

### Block encoding (§III E)

Qualtran's block encoding library codifies the formalism from Gilyén et al.'s QSVT paper. A $(\alpha, a, \varepsilon)$-block encoding of operator $A$ is a unitary $B[A]$ such that $A/\alpha$ sits in the top-left block.

Primitive block encodings:
- `Unitary` — wraps any unitary as a $(1, 0, 0)$-block encoding
- `LCUBlockEncoding` — from SELECT/PREPARE oracles
- `SparseMatrix` — from row/column/entry oracles for $m$-sparse matrices

Composite operations: `TensorProduct`, `Product`, `Phase`, `LinearCombination`, `ChebyshevPolynomial` — each automatically propagates $\alpha$, $a$, $\varepsilon$ through the composition.

### Quantum signal processing (§III F)

`GeneralizedQSP` bloq implements the Motlagh & Wiebe construction: given a unitary $U$ and degree-$d$ polynomial $P$, implements a block encoding of $P(U)$ using $d$ calls to controlled-$U$ plus $d+1$ single-qubit rotations. Three methods for computing the complementary polynomial $Q$: factorization, convex optimization, and FFT-based.

### Phase estimation (§III G)

`TextbookQPE` and `QubitizationQPE` accept any `QPEWindowStateBase`. Three window states:

| Window | Optimal for | Query cost |
|---|---|---|
| Rectangular | Textbook baseline | $O(1/(\varepsilon\delta))$ |
| Sine (`LPResourceState`) | Holevo variance $V_H = \varepsilon^2$ | $O(\pi/\varepsilon)$ |
| Kaiser (`KaiserWindowState`) | Confidence interval $\Pr[\Delta\phi > \varepsilon] \le \delta$ | $O((1/\varepsilon)\ln(1/\delta))$ |

The [[Kaiser Window Amplitude Estimation|Kaiser window]] achieves the information-theoretic optimal $\Omega((1/\varepsilon)\log(1/\delta))$ cost from Mande & de Wolf.

---

## Case studies

### Case study 1: Hamiltonian simulation (§IV)

Two methods implemented via the block encoding + [[Qubitization (Quantum Walk for Spectral Encoding)|qubitization]] infrastructure:

1. **Chebyshev polynomial approach** — constructs block encodings of $\cos(Ht)$ and $\sin(Ht)$ via Jacobi-Anger expansion into [[Qubitization (Quantum Walk for Spectral Encoding)|Chebyshev polynomials]] of the walk operator
2. **GQSP approach** — `HamiltonianSimulationByGQSP` directly implements $(1, a+1, \varepsilon)$-block encoding of $e^{-iHt}$ via GQSP polynomial approximation with degree $O(\alpha t + \log(1/\varepsilon)/\log\log(1/\varepsilon))$

Concrete example: 2D Hubbard model ($4 \times 4$, $t=5$, $\varepsilon = 10^{-5}$) — 58 qubits, 139,476 AND gates, 14,905 rotations via GQSP.

### Case study 2: Quantum chemistry (§V)

Block encodings for all four second-quantized Hamiltonian factorizations (sparse, SF, DF, [[THC Non-Orthogonal Diagonal Coulomb Representation|THC]]) plus first-quantized. Table V compares Qualtran vs OpenFermion Toffoli counts for FeMoCo:

| Method | Qualtran block encoding | OpenFermion block encoding | Qualtran QPE | OpenFermion QPE |
|---|---|---|---|---|
| Sparse | 18,175 | 18,011 | $4.45 \times 10^{10}$ | $4.41 \times 10^{10}$ |
| SF | 25,679 | 24,349 | $1.24 \times 10^{11}$ | $1.18 \times 10^{11}$ |
| DF | 34,982 | 34,979 | $6.44 \times 10^{10}$ | $6.44 \times 10^{10}$ |
| THC | 17,150 | 16,891 | $3.24 \times 10^{10}$ | $3.19 \times 10^{10}$ |

Small discrepancies from implementation differences in basic primitives (e.g., comparators and swaps in Qualtran don't yet exploit measurement-based uncomputation everywhere).

The paper also demonstrates **symbolic Trotter cost analysis** for the Hubbard model, producing a closed-form expression (Eq. 12) that can be numerically optimized over the error budget allocation $\Delta_{TS} + \Delta_{HT} + \Delta_{PE} \le \varepsilon$.

### Case study 3: Cryptography (§VI)

RSA factoring and elliptic curve cryptography (ECC). The ECC construction follows Litinski (2023):
- Phase estimation of elliptic curve point addition
- Windowed arithmetic to reduce additions
- `ECAdd(n=256)` costs $\sim 7.8 \times 10^6$ Toffolis, dominated by `ModInv` ($\sim 24n^2$ Toffolis) and `ModMul` ($\sim 1.5 \times 10^6$)

Symbolic cost propagation through the call graph produces exact expressions for subroutine costs including all constant and linear terms.

---

## Physical cost models (§VII)

Qualtran translates logical gate/qubit counts to physical resources via surface-code models:

1. **Data block** — maps algorithm qubits to surface code tiles ($2d^2$ physical qubits each). Two model families: `SimpleDataBlock` (Gidney-Fowler, 50% routing overhead) and Litinski models (explicit routing, three space-time tradeoffs).

2. **Magic state factory** — distills noisy T/CCZ states ($p_\text{phys} \to p_\text{distil} \propto p_\text{phys}^3$); rate of production bottlenecks runtime. Multiple factory constructions available.

3. **QEC scheme** — logical error rate $p_l(d) = A(p_p/p^*)^{(d+1)/2}$. Presets: Fowler-Gidney ($A=0.1$, $p^*=0.01$) and Beverland ($A=0.03$, $p^*=0.01$).

4. **Physical parameters** — aggregate physical error rate and cycle time. Presets for superconducting, ion, and Majorana modalities.

The `PhysicalCostModel` combines all four components. Two optimization strategies: Gidney-Fowler grid search over code distances, or Beverland's error budget apportioning (1/3 each to rotation synthesis, distillation, and data storage).

---

## Limits / caveats

- **No new algorithms** — everything in the standard library was published previously. The contribution is systematization and software, not new theoretical results.
- **Surface-code-centric** — physical cost models assume surface code. Other QEC architectures are not yet supported (though the interfaces are extensible).
- **Overestimates qubit counts** — the sequential-execution assumption for callee bloqs adds bystander qubits that could be freed with more aggressive compilation. The paper acknowledges this.
- **Limited type system** — no quantum sum/product types, no quantum strings or lists. This limits expressiveness for certain algorithm classes.
- **Symbolic limitations** — SymPy handles most expressions, but $\lceil\log_2 x\rceil$ and similar quantum-information-specific operations need custom support.
- **No rewrite rules** — the framework can't automatically optimize or simplify compute graphs yet.
- **Adoption-dependent value** — if the community doesn't contribute bloqs, the library stagnates. The network effects only materialize with critical mass.

---

## Reusable ideas

1. [[Hierarchical Bloq Decomposition for Scalable Resource Counting]] — the pattern of recursive call-graph traversal with cached costs to handle $10^9$-gate algorithms in seconds
2. [[Symbolic Cost Propagation Through Call Graphs]] — propagating SymPy expressions through a DAG to produce closed-form cost formulas that can be numerically optimized
3. [[Dual Decomposition Strategy (Full vs Call-Graph)]] — letting subroutine authors provide either a full compute graph (for simulation) or a lightweight callee list (for cost estimation), and cross-checking between them
4. [[Error Budget Optimization via Symbolic Expressions]] — generating a multi-parameter symbolic cost function and numerically optimizing over the error allocation

---

## References within this paper

Key citations that connect to the vault:

- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes|Babbush, Gidney et al. (2018)]] — source of [[Unary Iteration]], [[QROM (Quantum Read-Only Memory)|QROM]], [[Coherent Alias Sampling for PREPARE|coherent alias sampling]], [[Qubitization (Quantum Walk for Spectral Encoding)|qubitization]] for chemistry
- [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes|Berry, Gidney, Motta, McClean, Babbush (2019)]] — source of [[QROAM (Space-Time Tradeoff for QROM)|QROAM]] and measurement-based uncomputation
- [[Compilation of Fault-Tolerant Quantum Heuristics for Combinatorial Optimization (Sanders, Berry, Babbush et al 2020) — Paper Notes|Sanders, Berry, Babbush et al. (2020)]] — variable-spaced QROM, QVR, [[Hamming Weight Phasing for Equiangular Rotations|Hamming weight phasing]]
- [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes|Lee, Berry, Babbush et al. (2021)]] — [[THC Non-Orthogonal Diagonal Coulomb Representation|THC]] factorization, multi-dimensional QROAM optimization
- [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes|Su, Berry, Wiebe, Rubin, Babbush (2021)]] — first-quantized block encodings
- [[Quantum Computation of Stopping Power for Inertial Fusion Target Design (Rubin, Berry, Babbush et al 2023) — Paper Notes|Rubin, Berry, Babbush et al. (2023)]] — non-BO dynamics block encoding
- [[Improved Fault-Tolerant Quantum Simulation of Condensed-Phase Correlated Electrons via Trotterization (Kivlichan, Gidney, Babbush et al 2020) — Paper Notes|Kivlichan, Gidney, Babbush et al. (2020)]] — Trotterized Hubbard model
- Low & Chuang (2019) — [[Qubitization (Quantum Walk for Spectral Encoding)|qubitization]] and Hamiltonian simulation by QSP
- Gilyén, Su, Low, Wiebe (2019) — [[Qubitization (Quantum Walk for Spectral Encoding)|QSVT]] framework, block encoding formalism
- Motlagh & Wiebe (2024) — Generalized QSP
- Jones, Whitfield et al. (2012) — programmable ancilla rotations
- Gidney & Fowler (2019) — CCZ factory and physical cost model
- Beverland et al. (2023) — alternative physical cost model
- Litinski (2023) — ECC construction and data block models
- Gidney (2018) — [[Hamming Weight Phasing for Equiangular Rotations|Hamming weight phasing]], phase gradient addition

---

## Cross-links

### Paper notes
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes]] — Qualtran's standard library is essentially a software implementation of this paper's constructions
- [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes]] — QROAM, measurement-based uncomputation
- [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes]] — THC implementation in Qualtran, FeMoCo benchmarks
- [[Compilation of Fault-Tolerant Quantum Heuristics for Combinatorial Optimization (Sanders, Berry, Babbush et al 2020) — Paper Notes]] — variable-spaced QROM, QVR primitives
- [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes]] — first-quantized algorithms in Qualtran
- [[Improved Fault-Tolerant Quantum Simulation of Condensed-Phase Correlated Electrons via Trotterization (Kivlichan, Gidney, Babbush et al 2020) — Paper Notes]] — Trotterized Hubbard case study
- [[Quantum Computation of Stopping Power for Inertial Fusion Target Design (Rubin, Berry, Babbush et al 2023) — Paper Notes]] — first-quantized dynamics in Qualtran
- [[Quantum Simulation of Realistic Materials in First Quantization Using Non-Local Pseudopotentials (Berry, Rubin, Babbush et al 2024) — Paper Notes]] — pseudopotential block encoding in Qualtran
- [[Analyzing Prospects for Quantum Advantage in Topological Data Analysis (Berry, Su, Babbush et al 2024) — Paper Notes]] — Kaiser window QPE
- [[Optimal Scaling Quantum Linear Systems Solver via Discrete Adiabatic Theorem (Costa, An, Sanders, Su, Babbush, Berry 2021) — Paper Notes]] — QLSP as QSP application
- [[Fault-Tolerant Quantum Simulation of Materials Using Bloch Orbitals (Rubin, Berry, Babbush et al 2023) — Paper Notes]] — periodic system block encodings
- [[Quantum Simulation of the Sachdev-Ye-Kitaev Model by Asymmetric Qubitization (Babbush, Berry, Neven 2019) — Paper Notes]] — asymmetric qubitization

### Trick cards
- [[Unary Iteration]] — core primitive, §III B
- [[QROM (Quantum Read-Only Memory)]] — core primitive, §III C
- [[QROAM (Space-Time Tradeoff for QROM)]] — core primitive, §III C
- [[Coherent Alias Sampling for PREPARE]] — state preparation, §III D
- [[Qubitization (Quantum Walk for Spectral Encoding)]] — core primitive, §IV A
- [[Phase Gradient State for Controlled Rotations]] — rotation strategy, §III A
- [[Hamming Weight Phasing for Equiangular Rotations]] — QVR primitive, §III A
- [[Kaiser Window Amplitude Estimation]] — QPE window, §III G
- [[THC Non-Orthogonal Diagonal Coulomb Representation]] — chemistry case study, §V
- [[Measurement-Based QROM Uncomputation]] — QROAMCleanAdjoint, §III C
- [[Hierarchical Bloq Decomposition for Scalable Resource Counting]] — new trick card
- [[Symbolic Cost Propagation Through Call Graphs]] — new trick card
- [[Dual Decomposition Strategy (Full vs Call-Graph)]] — new trick card
- [[Error Budget Optimization via Symbolic Expressions]] — new trick card
