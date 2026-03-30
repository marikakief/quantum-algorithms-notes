> **Source:** Marcus Cramer, Martin B. Plenio, Steven T. Flammia, Rolando Somma, David Gross, Stephen D. Bartlett, Olivier Landon-Cardinal, David Poulin, Yi-Kai Liu, *Efficient quantum state tomography*, Nature Communications 1:149, 2010
> **Links:** [arXiv:1101.4366](https://arxiv.org/abs/1101.4366) · [Nature Communications](https://doi.org/10.1038/ncomms1147)
> **Tags:** #tomography #MPS #tensor-networks #state-reconstruction #certification #parent-Hamiltonian #area-law

---

## The computational problem

**Full quantum state tomography** for an $N$-qudit system requires $O(d^{2N})$ measurements and exponential classical post-processing — clearly infeasible beyond $\sim 10$ qubits. The question: for states with bounded entanglement (specifically, states well-approximated by [[MPS Canonical Form for Efficient State Tracking|matrix product states]] with bond dimension $D$), can tomography be done with polynomial resources?

**Input:** Copies of an unknown state $\hat{\varrho}$ on $N$ qudits (each dimension $d$), arranged in a 1D chain.

**Output:** An MPS description of $\hat{\varrho}$ (or a close approximation), together with a rigorous fidelity certificate — *without any a priori assumptions about the state*.

## What the paper does

Presents two schemes for MPS tomography that both use only $O(N)$ local measurements and polynomial classical post-processing:

1. **Unitary scheme:** Sequentially disentangle the chain using $\kappa$-site unitaries ($\kappa = \lceil \log_d D \rceil + 1$), recovering the MPS circuit that prepared the state. Directly inverts the MPS-to-circuit construction from [[Quantum Circuits for Strongly Correlated Quantum Systems (Verstraete-Cirac-Latorre 2009) — Paper Notes|Verstraete-Cirac-Latorre (2009)]] and Schön et al. (2005).

2. **Local measurement scheme:** Measure all groups of $k$ neighbouring qudits ($k$ depending on bond dimension). Reconstruct an MPS estimate via a modified singular value thresholding (SVT) algorithm that exploits MPS structure for efficient diagonalisation at each step. Certify fidelity using a parent Hamiltonian witness.

Both schemes are rigorous: the accuracy of the reconstruction is certified directly from the data, without assumptions about the state being an MPS.

## The algorithm / construction

### Scheme 1: Sequential disentanglement (unitary control)

The MPS structure means that the reduced density matrix $\hat{\sigma}$ on the first $\kappa = \lceil \log_d D \rceil + 1$ sites has rank $\leq D < d^{\kappa - 1}$. This allows a unitary $\hat{U}_1$ on sites $1, \ldots, \kappa$ that disentangles site 1:

$$\hat{U}_1 |\phi\rangle = |0\rangle_1 \otimes |v\rangle_{2,\ldots,N}$$

The construction:

$$\hat{U} = \sum_{s=0}^{d-1} \sum_{s'=0}^{d^{\kappa-1}-1} |s\rangle_1 \otimes |s'\rangle_{2,\ldots,\kappa} \langle \phi_{s d^{\kappa-1} + s' + 1}|_{1,\ldots,\kappa}$$

where $|\phi_1\rangle, \ldots, |\phi_R\rangle$ are the eigenvectors of $\hat{\sigma}$ (extended to a complete basis).

Repeat for each site: estimate the local reduced density matrix, find the disentangling unitary, apply it, move one site right. After $N - \kappa + 1$ steps, you've recovered:

$$\hat{U}_{N-\kappa+1} \cdots \hat{U}_1 |\phi\rangle = |0\rangle^{\otimes (N-\kappa+1)} \otimes |\eta\rangle$$

where $|\eta\rangle$ is a boundary state on the last $\kappa - 1$ sites. The sequence $\{U_i\}$ and $|\eta\rangle$ give the MPS decomposition directly.

**Error accumulation:** If each disentangling step has error $\varepsilon_i$ (from finite measurement precision or rank truncation), errors accumulate *linearly*:

$$\||\phi\rangle - |\psi\rangle\| \leq \sum_{i=1}^{N-\kappa+1} \varepsilon_i \leq N\varepsilon$$

Each $\varepsilon_i$ is directly measurable: it's the population of the non-zero states after measuring the disentangled qudit.

### Scheme 2: Local measurements + MPS-SVT

**Experimental step:** Perform tomography on all groups of $k$ neighbouring qudits, obtaining estimates $\hat{\sigma}_i \approx \hat{\varrho}_i$ with $\|\hat{\varrho}_i - \hat{\sigma}_i\|_{\text{tr}} \leq \varepsilon_i$.

**Classical post-processing:** Find an MPS $|\psi\rangle$ whose local reduced density matrices match the measured $\hat{\sigma}_i$.

The algorithm is a modification of singular value thresholding (SVT), adapted for MPS. Standard SVT requires diagonalising $2^N \times 2^N$ matrices — exponentially expensive. The key insight: at each SVT iteration, the matrix $\hat{Y}_n$ to diagonalise has the form $\sum_{m,i} a_{m,i} \hat{P}_m^i$ where $\hat{P}_m^i$ are local Pauli products — i.e., it's a local "Hamiltonian". Finding its top eigenstate is a standard DMRG/MPS optimisation problem, solvable in polynomial time.

**Algorithm:**
1. Set $\hat{R} = \sum_{m,i} \text{tr}[\hat{\sigma}_i \hat{P}_m^i] \hat{P}_m^i / 2^N$
2. Initialise $\hat{Y}_0 = 0$
3. At each step $n$: find the top eigenstate $|y_n\rangle$ of $\hat{Y}_n$ via DMRG, compute $\hat{X}_n$ from $|y_n\rangle$, update $\hat{Y}_{n+1} = \hat{Y}_n + \delta_n(\hat{R} - \hat{X}_n)$
4. Repeat until convergence

### Certification via parent Hamiltonian

A generic MPS $|\psi\rangle$ with bond dimension $D$ is the unique ground state of a local *parent Hamiltonian* $\hat{H} = \sum_i \hat{h}_i$ where each $\hat{h}_i$ is a projector onto the subspace orthogonal to the range of the MPS transfer map on $k$ sites. The energy gap $\Delta E$ is computable from the MPS data.

The fidelity bound follows from:

$$\langle\psi|\hat{\varrho}|\psi\rangle \geq 1 - \frac{\sum_i (\varepsilon_i + \text{tr}[\hat{h}_i \hat{\sigma}_i])}{\Delta E}$$

This is tight: if $\hat{\varrho} = |\psi\rangle\langle\psi|$ and measurements are exact, the bound gives fidelity 1. The parent Hamiltonian acts as a *witness* for its ground state.

**Gap lower bound:** Using the inequality $\hat{H}^2 \geq (1-\gamma)\hat{H}$ where $\gamma = \max_n \sum_{m: 1 \leq |n-m|k \leq 2k} \gamma_{n,m}$ and $\gamma_{n,m}$ is the smallest non-zero eigenvalue of $\hat{P}_n + \hat{P}_m$, one gets $\Delta E \geq 1 - \gamma$, computable in polynomial time.

## Key results

| Property | Scaling |
|---|---|
| Measurements (Scheme 1) | $O(N)$ local tomographies on $\kappa$-site blocks |
| Measurements (Scheme 2) | $O(N)$ local tomographies on $k$-site blocks |
| Classical post-processing | Polynomial in $N$ (DMRG per SVT iteration) |
| Error accumulation | Linear: total error $\leq N\varepsilon$ |
| Certification | Rigorous fidelity bound, no assumptions on $\hat{\varrho}$ |
| Mixed states | Handled via purification; $\kappa$ increases by $\log_d(M)$ for $M$-component mixtures |

**Numerical evidence:** For the critical Ising model, the infidelity $1 - f_{N,n}$ scales as $N^2/n$ where $n$ is the number of SVT iterations. For random Hamiltonians, similar scaling. For W-states with simulated noise (Gaussian, $\sigma = 0.005$), fidelity $> 0.94$ for up to 20 ions.

## Comparison with prior work

| Method | Measurements | Classical processing | Assumptions |
|---|---|---|---|
| Full state tomography | $O(d^{2N})$ | $O(d^{3N})$ | None |
| [[Shadow Tomography of Quantum States (Aaronson 2018) — Paper Notes\|Shadow tomography]] | $O(\text{polylog}\, M \cdot \log d / \varepsilon^4)$ for $M$ observables | Polynomial | Knows which observables to estimate |
| [[Classical shadows (Huang-Kueng-Preskill 2020)\|Classical shadows]] | $O(\log M / \varepsilon^2)$ for $M$ observables | Polynomial | Random measurement bases |
| **This paper — MPS tomography** | $O(N)$ local measurements | Polynomial (DMRG) | None — certifies from data |
| Compressed sensing | $O(rN \text{polylog}\, d^N)$ for rank $r$ | SDP (expensive) | Low-rank assumption |

The key advantage over shadow-based methods: this reconstructs the *full state description* (as an MPS), not just expectation values of specific observables. The key advantage over compressed sensing: MPS structure makes the classical processing polynomial in $N$.

## Limits / caveats

- **1D systems primarily.** The sequential disentanglement scheme directly generalises to tree tensor networks and MERA but not easily to 2D PEPS (where contraction is #P-hard in general). The certification method via parent Hamiltonians extends to frustration-free 2D systems with nearest-neighbour qubit couplings.
- **Bond dimension must be moderate.** The block size $\kappa = \lceil\log_d D\rceil + 1$ grows with $D$. For states requiring large $D$ (e.g., critical 2D systems), the "local" measurement blocks become large.
- **SVT convergence is empirical.** The standard SVT has a convergence proof; the MPS-modified version (using DMRG for eigenstate finding) does not. The paper provides numerical evidence but no guarantee.
- **Parent Hamiltonian certification fails for non-generic MPS.** GHZ-type states have degenerate parent Hamiltonians. The paper handles this by additionally measuring string operators, but the treatment is ad hoc.
- **Noise sensitivity.** The fidelity bound degrades as $\sum_i \varepsilon_i / \Delta E$. If the parent Hamiltonian gap $\Delta E$ is small (as it can be near quantum phase transitions), the certification becomes loose.

## Reusable ideas

1. **[[Sequential Disentanglement for MPS Tomography]]** — Recover the MPS circuit that prepared a state by sequentially applying disentangling unitaries based on local reduced density matrices. Each step peels off one site. Errors accumulate linearly.

2. **[[Parent Hamiltonian Witness for State Certification]]** — Certify the fidelity of a reconstructed MPS by constructing its parent Hamiltonian and using it as a witness. The gap $\Delta E$ is computable from the MPS, and the fidelity bound requires no assumptions about the true state.

3. **[[MPS-SVT for Polynomial State Reconstruction]]** — Modify the singular value thresholding algorithm to exploit MPS structure: replace the exponentially expensive diagonalisation step with DMRG (polynomial cost). Converts a general convex optimisation problem into a tractable MPS optimisation.

## References within this paper

- [[Efficient Classical Simulation of Slightly Entangled Quantum Computations (Vidal 2003) — Paper Notes|Vidal (2003)]] — efficient classical simulation via MPS (ref [18])
- Fannes, Nachtergaele, Werner (1992) — finitely correlated states / MPS definition (ref [9])
- Perez-Garcia, Verstraete, Wolf, Cirac (2007) — MPS representations and parent Hamiltonians (ref [10])
- Schön, Hammerer, Wolf, Cirac, Solano (2005) — sequential generation of entangled states = MPS circuit construction (ref [17])
- [[Shadow Tomography of Quantum States (Aaronson 2018) — Paper Notes|Aaronson (2018)]] — shadow tomography (later work, related approach)
- Gross, Liu, Flammia, Becker, Eisert (2009) — quantum compressed sensing (ref [25])
- Hastings (2007) — area law for 1D systems (ref [15])

## Cross-links

### Paper notes
- [[Efficient Classical Simulation of Slightly Entangled Quantum Computations (Vidal 2003) — Paper Notes]] — MPS framework that makes this tomography scheme possible
- [[Quantum Circuits for Strongly Correlated Quantum Systems (Verstraete-Cirac-Latorre 2009) — Paper Notes]] — the MPS-to-circuit construction that Scheme 1 inverts
- [[Shadow Tomography of Quantum States (Aaronson 2018) — Paper Notes]] — alternative approach: estimate observables without full reconstruction
- [[Classical shadows (Huang-Kueng-Preskill 2020)]] — random measurement approach to observable estimation
- [[Matchgate Shadows for Fermionic Quantum Simulation (Wan, Huggins, Lee, Babbush 2022) — Paper Notes]] — shadow tomography adapted for fermionic (MPS-like) structure
- [[Lieb-Robinson Bounds and the Generation of Correlations and Topological Quantum Order (Bravyi-Hastings-Verstraete 2006) — Paper Notes]] — area law preservation justifies why MPS are good approximations for physically relevant states
- [[Rapid Initial State Preparation for the Quantum Simulation of Strongly Correlated Molecules (Berry, Tong, Babbush, Rubin et al 2024) — Paper Notes]] — MPS state preparation on quantum hardware; MPS overlap certification relates to this work's certification ideas
- [[The Learnability of Quantum States (Aaronson 2006) — Paper Notes]] — PAC learning of quantum states, different formulation of efficient state learning

### Trick cards
- [[Sequential Disentanglement for MPS Tomography]]
- [[Parent Hamiltonian Witness for State Certification]]
- [[MPS-SVT for Polynomial State Reconstruction]]
- [[MPS Canonical Form for Efficient State Tracking]]
- [[Local Gate Update on MPS via Schmidt Redecomposition]]
