_Comparison tables for quantum simulation circuit primitives: product formulas, Trotter circuits, and state preparation._

_Split from [[Hamiltonian Simulation — Comparison Tables]]._

---

### Best [[Product Formulas]]s by order (eigenvalue error)

Best-performing [[Product Formulas]]s for each order, as benchmarked in [[Selection and Improvement of Product Formulae for Best Performance of Quantum Simulation (Morales-Costa-Pantaleoni-Burgarth-Sanders-Berry 2025) — Paper Notes|Morales et al. (2025)]]. Performance metric is $M\zeta^{1/k}$ where $M$ = stages, $\zeta$ = eigenvalue error constant, $k$ = order. Lower is better.

| Order | Formula | Stages ($M$) | Processed? | $\zeta$ (eigenvalue) | $M\zeta^{1/k}$ | Source |
|---|---|---|---|---|---|---|
| 4th | BM4M6 | 6 | No | $2.4 \times 10^{-5}$ | 0.42 | Blanes-Moan (2002) |
| 4th | PPBCM4m6 | 6 | Yes | $2.4 \times 10^{-5}$ | 0.42 | Blanes-Casas-Murua (2006) |
| 6th | PPBCM6m9 | 9 | Yes | $4.3 \times 10^{-7}$ | 0.78 | Blanes-Casas-Murua (2006) |
| 8th | **YP8m8** | **17** | **Yes** | $\mathbf{8.1 \times 10^{-10}}$ | **1.24** | **[[Selection and Improvement of Product Formulae for Best Performance of Quantum Simulation (Morales-Costa-Pantaleoni-Burgarth-Sanders-Berry 2025) — Paper Notes|Morales et al. (2025)]]** |
| 8th | **Y8m10b** | **21** | **No** | $\mathbf{5.4 \times 10^{-10}}$ | **1.46** | **[[Selection and Improvement of Product Formulae for Best Performance of Quantum Simulation (Morales-Costa-Pantaleoni-Burgarth-Sanders-Berry 2025) — Paper Notes|Morales et al. (2025)]]** |
| 8th | SS8s19 (prior best) | 19 | No | $5.3 \times 10^{-8}$ | 2.34 | Sofroniou-Spaletta (2005) |
| 10th | SS10s35 | 35 | No | $3.1 \times 10^{-11}$ | 3.11 | Sofroniou-Spaletta (2005) |

Cross-order thresholds (random matrices, [[Eigenvalue Error as the Correct Product Formula Metric|eigenvalue error]]):

| Transition | Asymptotic $T/\varepsilon$ | Non-asymptotic $T/\varepsilon$ | Practical range |
|---|---|---|---|
| 4th → 6th | ~1,700 | — | NISQ |
| 6th → 8th | ~68,000 | ~$3.2 \times 10^7$ | Early fault-tolerant |
| 8th → 10th | ~$9.3 \times 10^{15}$ | Similar | Never (for quantum computing) |


### Trotter step circuit depth comparison (linear connectivity)

| Method | Trotter-step depth | Two-qubit gates | Connectivity | Reference |
|---|---|---|---|---|
| Naive JW Pauli decomposition | $O(N^4)$ | $O(N^5)$ | Linear | [[Simulated Quantum Computation of Molecular Energies (Aspuru-Guzik-Dutoi-Love-Head-Gordon 2005) — Paper Notes|Aspuru-Guzik et al. (2005)]] |
| Prior swap network | $2N$ | $O(N^2)$ | Planar | [Prior work, ref 40] |
| [[Quantum Simulation of Electronic Structure with Linear Depth and Connectivity (Kivlichan, McClean et al 2018) — Paper Notes\|Fermionic swap network]] | $N$ | $N(N{-}1)/2$ | **Linear** | [[Quantum Simulation of Electronic Structure with Linear Depth and Connectivity (Kivlichan, McClean et al 2018) — Paper Notes|Kivlichan et al. (2018)]] |
| [[Low Rank Representations for Quantum Simulation of Electronic Structure (Motta, Babbush, Chan et al 2018) — Paper Notes\|Double factorization]] | $O(N^2)$ | $O(N^2 \log N)$ asymptotic | **Linear** | [[Low Rank Representations for Quantum Simulation of Electronic Structure (Motta, Babbush, Chan et al 2018) — Paper Notes|Motta et al. (2018)]] |
| [[Discontinuous Galerkin Discretization for Quantum Simulation of Chemistry (McClean, Babbush, Lin et al 2019) — Paper Notes\|DG block-diagonal swap network]] | $O(N_b n_\kappa^3)$ | $O(N_b^2 n_\kappa^4)$ | **Linear** | [[Decoding Quantum Errors with Subspace Expansions (McClean, Jiang, Rubin, Babbush, Neven 2019) — Paper Notes|McClean et al. (2019)]] |
| FFFT (plane-wave basis) | $O(N \log N)$ | $O(N \log N)$ | Planar | — |


### Slater determinant state preparation comparison

| Method | Depth | Givens rotations | Connectivity | Reference |
|---|---|---|---|---|
| Wecker et al. | $O(N^2)$ | $N^2$ | Arbitrary | Wecker et al. (2015) |
| FFFT-based (plane waves) | $O(N)$ | $O(N \log N)$ | Planar | — |
| [[Givens Rotation Slater Determinant Preparation\|Parallel Givens (this work)]] | $\leq N/2$ | $\eta(N{-}\eta)$ (with symmetry) | **Linear** | [[Quantum Simulation of Electronic Structure with Linear Depth and Connectivity (Kivlichan, McClean et al 2018) — Paper Notes|Kivlichan et al. (2018)]] |


### Multi-determinant state preparation comparison

| Method | Gate cost | Ancilla | Notes | Reference |
|---|---|---|---|---|
| Compressed register + [[QROM (Quantum Read-Only Memory)\|QROM]] | $O(L)$ (register prep) + isometry | $\lceil \log L \rceil$ | Isometry via QROM or select-unitary | [[Postponing the Orthogonality Catastrophe (Tubman, Mejuto-Zaera, Babbush et al 2018) — Paper Notes\|Tubman et al. (2018)]] |
| [[Sequential Multi-Determinant State Preparation\|Sequential auxiliary-qubit]] | $O(nL)$ | $1$ | Reduced by Hamming ordering; no index register | [[Postponing the Orthogonality Catastrophe (Tubman, Mejuto-Zaera, Babbush et al 2018) — Paper Notes\|Tubman et al. (2018)]] |


### Trotter step count estimates for chemical accuracy

| Molecule | Orbitals | Est. Trotter steps (first-order) | Reference |
|---|---|---|---|
| H₂ (STO-6G) | 4 | $\sim 10$ | [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes|O'Malley et al. (2016)]] |
| LiH | 12 | $\sim 100$ | Wecker et al. (2015) |
| BeH₂ | 14 | $\sim 200$ | Wecker et al. (2015) |


---

**See also:** [[Quantum Chemistry — NISQ Experiments & VQE]] · [[Quantum Chemistry — Fault-Tolerant Resource Estimates]] · [[Quantum Chemistry — Technique Crosswalk]] · [[Hamiltonian Simulation — Time-Dependent Methods]]
