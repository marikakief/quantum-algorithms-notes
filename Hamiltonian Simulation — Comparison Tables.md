# Hamiltonian Simulation — Comparison Tables

> **Purpose:** Fast comparison across the Hamiltonian simulation papers in this vault.

## 1) Big-picture method comparison

| Paper | Core method | Precision dependence (headline) | Structural leverage | Main tradeoff |
|---|---|---|---|---|
| [[Universal Quantum Simulators (Lloyd 1996) — Paper Notes]] | First-order product formula | $O(\ell m^2 t^2/\varepsilon)$ operations | Locality: each $H_i$ acts on $O(1)$-dim space | $O(t^2/\varepsilon)$ step scaling; first-order only |
| [[Adiabatic Quantum State Generation and Statistical Zero Knowledge (Aharonov-Ta-Shma 2003) — Paper Notes]] | Trotter + coloring decomposition | Poly in $t$; $\mathrm{poly}(n, t, 1/\epsilon)$ circuit size | Sparse entry-coloring into 2×2 blocks | $(D+1)^2 n^6$ term count; first sparse simulation |
| [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes]] | Suzuki product formula + edge-coloring decomposition | $O(d^4 \tau^{1+1/2k} / \epsilon^{1/2k})$; near-linear via $k$-optimization | Local edge-coloring to 1-sparse pieces | $d^4$ sparsity cost; $5^{2k}$ constant blowup |
| [[Limitations on Simulation of Non-Sparse Hamiltonians (Childs-Kothari-Somma 2010) — Paper Notes]] | Oracle lower-bound / no-go analysis | Rules out generic $\mathrm{poly}(\|H\|t,\log N)$ in entry-oracle non-sparse setting | Norm-gap constructions + hardness embeddings | Model-specific (stronger oracles can bypass) |
| [[Black-Box Hamiltonian Simulation and Unitary Implementation (Berry-Childs 2011) — Paper Notes]] | Quantum-walk black-box simulation | Early linear-in-time black-box gains (pre-QSP era) | Walk isometry encoding + amplitude-assisted state prep | Error/normalization dependence weaker than modern methods |
| [[LCU Origins (Childs-Wiebe 2012) — Paper Notes]] | LCU + postselection | Non-polylog (early era) | LCU linear-combo structure | Foundational but success/failure handling overhead |
| [[Exponential Improvement in Precision for Hamiltonian-Evolution Simulation (Berry-Cleve-Somma 2013) — Paper Notes]] | Compressed fractional-query Trotter | $\text{polylog}(1/\varepsilon)$ — first! | Self-inverse decomp + Hamming weight truncation | Complex 6-step pipeline; superseded by STOC 2014 |
| [[Exponential Improvement in Precision for Simulating Sparse Hamiltonians (Berry-Childs-Cleve-Kothari-Somma 2014) — Paper Notes]] | Fractional queries + OAA | $\widetilde{O}(\log(1/\varepsilon))$ — provably optimal | OAA replaces fault correction; bipartite trick removes $\log^* n$ | Still goes through fractional queries; superseded by Taylor |
| [[Hamiltonian Simulation with Nearly Optimal Dependence on All Parameters (Berry-Childs-Kothari 2015) — Paper Notes]] | Bessel-LCU of walk steps | $\widetilde{O}(\log(1/\varepsilon))$, $\tau = d\|H\|_{\max}t$ | Bessel linearisation; walk gives linear $d$ | $\log\log$ factor; superseded by QSP |
| [[Simulating Hamiltonian Dynamics with a Truncated Taylor Series (Berry-Childs-Cleve-Kothari-Somma 2015) — Paper Notes]] | Truncated Taylor series + LCU + robust OAA | $\widetilde{O}(\log(1/\varepsilon))$ — near-optimal | Taylor truncation at $K = O(\log/\log\log)$; $\ln 2$ segmentation | $\log\log$ factor vs QSP-optimal; no commutator benefit |
| [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes]] | Truncated Dyson LCU | Polylog-ish factors but significant overhead | Time-ordered expansions | Coherent time-ordering cost |
| [[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes]] | Interaction-picture Dyson | Better when $A$ easy, $B$ small | Norm-splitting $H=A+B$ | Needs cheap controlled $e^{\pm iA\tau}$ |
| [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes]] | QSP | $O(\tau + \log(1/\epsilon)\log\log(1/\epsilon))$ | Polynomial/eigenphase transform | Phase synthesis complexity |
| [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]] | Qubitization + QSP | $O(\tau + \log(1/\epsilon))$ | SU(2) invariant-subspace + block-encoding | Requires efficient block-encoding |
| [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]] | QSVT meta-framework | Degree-driven; near-optimal across tasks | Singular-value polynomial transforms | Polynomial design + parity constraints |
| [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]] | Product-formula error theory | Polynomial in $1/\epsilon$ (usually) | Nested commutator structure | Not black-box optimal precision scaling |
| [[Recursive Trotterization with L1-Norm Reduction and Low-Rank Decomposition (2022) — Paper Notes]] | Structured Trotter step implementation | Regime-dependent; can be subquadratic / near-linear spacetime | Power-law locality, commuting groups, low-rank blocks | Gains require strong structure assumptions |
| [[Faster Digital Quantum Simulation by Symmetry Protection (Tran-Su-Childs-Wiebe 2021) — Paper Notes]] | Symmetry-protected product formulas | Improves effective error-vs-steps in symmetric settings | Group symmetries + error averaging/cancellation | Needs cheap symmetry gates and commuting conditions |
| [[Structured PDE Hamiltonian Simulation via QFT-QSFT-QCT (Quantum 2021-11-10-574) — Paper Notes]] | Structured diagonalization + QLSA | Polylog-precision pipeline when structure/smoothness holds | Circulant/tensor spectral structure + transform tricks | Strong structure and state-prep assumptions |
| [[Randomized Product Formulas for Hamiltonian Simulation (Quantum 2019-09-02-182) — Paper Notes]] | Randomized Suzuki/Trotter ordering | Improved $L$-dependence for PF error/gate tradeoffs | Permutation averaging + mixing lemma | High-order constants can be large |
| [[Time-Dependent Hamiltonian Simulation with L1-Norm Scaling (Quantum 2020-04-20-254) — Paper Notes]] | Continuous qDRIFT + rescaled Dyson | $\|H\|_{\infty,1}$-driven scaling (quadratic for c-qDRIFT; near-linear for rescaled Dyson) | Time-sampling + time-reparameterization | Channel randomization or heavier LCU machinery |
| [[Sublinear-T Block-Encodings for Second-Quantized Hamiltonians (arXiv 2510.08644) — Paper Notes]] | Advanced block-encoding engineering | $\tilde O(\sqrt L)$-style block-construction T-cost in key regimes | SELECT-SWAP + SWAP-UP + subspace diffusion | Clifford routing/oracle complexity can dominate |
| [[Quantum Eigenvalue Processing and Transformation for Non-Normal Matrices (arXiv 2401.06240) — Paper Notes]] | Non-normal eigenvalue transform framework | Heisenberg-type eigenvalue estimation; efficient non-normal transforms under conditioning assumptions | Generating-function history states + QLSA + Chebyshev/Faber bases | Conditioning/non-normality constants can dominate |
| [[SOSSA — Sum-of-Squares Spectral Amplification]] | SOS + spectral amplification | Low-energy query gain $\sim\sqrt{\Delta\lambda}/\epsilon$ | PSD shift + factorization | Preprocessing + model assumptions |
| [[Shadow Hamiltonian Simulation — Paper Notes]] | Shadow-state compressed dynamics | Depends on $H_S$ simulation | Invariant operator algebra (IP) | Needs closure + sparse accessible $H_S$ |

---

## 2) “When should I actually use this?”

| Situation | Best first candidate | Why |
|---|---|---|
| Need strongest asymptotic precision scaling | [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]] / [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]] | Near-optimal black-box scaling |
| Time-dependent $H(t)$ with explicit ordering constraints | [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes]] | Native coherent time-ordering machinery |
| $H=A+B$ where $A$ easy and large, $B$ hard but small | [[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes]] | Interaction-picture norm reduction |
| Local / near-commuting models; practical constants matter | [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]] | Commutator-aware Trotter bounds |
| Structured long-range (power-law/compressible) interactions | [[Recursive Trotterization with L1-Norm Reduction and Low-Rank Decomposition (2022) — Paper Notes]] | Sublinear-in-term implementation tricks |
| Ground-state/low-energy-focused estimation | [[SOSSA — Sum-of-Squares Spectral Amplification]] | Amplifies low-energy region |
| Observable-set dynamics with Lie-algebra closure | [[Shadow Hamiltonian Simulation — Paper Notes]] | Exponential state-space compression in favorable IP settings |

---

## 3) Technique crosswalk (shared motifs)

| Technique motif | Where it appears |
|---|---|
| History-state encoding + spectral gap | [[Adiabatic Quantum Computation is Equivalent to Standard Quantum Computation (Aharonov-van Dam-Kempe-Landau-Lloyd-Regev 2004) — Paper Notes]] |
| Edge-coloring / sparse decomposition | [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes]], [[Black-Box Hamiltonian Simulation and Unitary Implementation (Berry-Childs 2011) — Paper Notes]] |
| LCU / PREPARE-SELECT | [[LCU Origins (Childs-Wiebe 2012) — Paper Notes]], [[Simulating Hamiltonian Dynamics with a Truncated Taylor Series (Berry-Childs-Cleve-Kothari-Somma 2015) — Paper Notes]], [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes]], [[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes]], [[SOSSA — Sum-of-Squares Spectral Amplification]] |
| Polynomial transform view | [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes]], [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]], [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]] |
| Norm reduction by representation change | [[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes]], [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]], [[Recursive Trotterization with L1-Norm Reduction and Low-Rank Decomposition (2022) — Paper Notes]] |
| Structured compression (rank/algebra) | [[Recursive Trotterization with L1-Norm Reduction and Low-Rank Decomposition (2022) — Paper Notes]], [[Shadow Hamiltonian Simulation — Paper Notes]] |

---

## Related trick cards

- [[Trotter Commutator-Scaling Bound]]
- [[Product-Formula Error Representation Switching]]
- [[Recursive Interval Decomposition for Power-Law Hamiltonians]]
- [[Hierarchical Low-Rank Hamiltonian Decomposition]]
- [[Interaction-Picture Norm Reduction]]
- [[Qubitization Iterate]]
- [[QSVT Meta-Template]]
