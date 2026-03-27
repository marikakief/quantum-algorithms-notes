> **Source:** Robbie King, Guang Hao Low, Ryan Babbush, Rolando D. Somma, Nicholas C. Rubin, *Quantum Simulation with Sum-of-Squares Spectral Amplification*, arXiv:2505.01528, PRL **136**(11), 110601 (2026)
> **Links:** [arXiv](https://arxiv.org/abs/2505.01528) · [PRL](https://doi.org/10.1103/PhysRevLett.136.110601)
> **Tags:** #hamiltonian-simulation #spectral-amplification #sum-of-squares #SYK #low-energy #phase-estimation #energy-estimation #block-encoding #qubitization

---

## The computational problem

Given a Hamiltonian $H = \sum_{j} g_j \sigma_j$ (a [[Linear Combination of Unitaries (LCU)|linear combination of unitaries]]) and a state $|\psi\rangle$ supported on low-energy eigenstates, perform one of three simulation tasks to precision $\varepsilon$:

1. **Energy estimation:** Estimate $E = \langle\psi|H|\psi\rangle$
2. **Ground-state phase estimation:** Estimate the ground-state energy $E_0$, given a trial state with overlap $p = |\langle\psi|\psi_0\rangle|^2 > 0$
3. **Time evolution:** Implement $e^{-iHt}$ on states of energy at most $E$

Standard methods using the [[Block-Encoding Composition Algebra|block-encoding]] $\mathrm{Be}[H/\lambda_{\text{LCU}}]$ have query complexities scaling linearly in $\lambda_{\text{LCU}} = \sum_j |g_j|$. The question: can the low-energy assumption be exploited to beat this?

---

## What the paper does

Introduces **SOSSA** — sum-of-squares spectral amplification — a framework that reduces query complexity from $O(\lambda/\varepsilon)$ to $O(\sqrt{\Delta_{\text{SOS}} \cdot \lambda_{\text{SOS}}}/\varepsilon)$ for low-energy simulation tasks. The key: combine a classical SDP-based [[SOS Decomposition via Semidefinite Programming for Chemistry|SOS decomposition]] (to get a tight lower bound $-\beta$ on the ground-state energy, making $\Delta_{\text{SOS}} = E + \beta$ small) with [[SOS Spectral Amplification|spectral amplification]] (to exploit the nonlinearity of $\sqrt{x}$ near zero). The paper provides self-contained proofs for energy and phase estimation, demonstrates query-optimality via matching lower bounds, and applies the framework to the [[Quantum Simulation of the Sachdev-Ye-Kitaev Model by Asymmetric Qubitization (Babbush, Berry, Neven 2019) — Paper Notes|SYK model]] where SOSSA achieves an asymptotic $\sqrt{N}$ speedup in system size.

This is the theory paper. The companion [[Fast Quantum Simulation of Electronic Structure by Spectrum Amplification (Low, King, Berry, Babbush, Somma, Rubin 2025) — Paper Notes|Low, King, Berry et al. (2025)]] applies SOSSA to real chemistry Hamiltonians with [[DFTHC Factorization|DFTHC factorization]], achieving state-of-the-art gate counts for molecular systems.

---

## The algorithm / construction

### Step 1: SOS representation

Write the shifted Hamiltonian as a sum of squares of general operators:

$$H + \beta\mathbb{1} = \sum_{j=0}^{R-1} B_j^\dagger B_j$$

where $-\beta$ is a lower bound on the ground-state energy. Each $B_j = \vec{b}_j \cdot \vec{X}^{(k)}$ is a polynomial in some operator algebra $\vec{X}^{(k)}$ of degree $k$ (e.g., Majorana monomials up to degree $k$, or Pauli strings up to weight $k$). The optimal $\beta$ is found by solving a semidefinite program — see [[SOS Lower Bound via Gram Matrix SDP]].

Termwise SA is the simplest special case: $B_j = |g_j|^{1/2} \Pi_j$ where $\Pi_j = \frac{1}{2}(\mathbb{1} + \text{sign}(g_j)\sigma_j)$ are projectors. This gives $\beta = \lambda_{\text{LCU}}$, which is loose for frustrated systems.

### Step 2: Spectral amplification

Define the rectangular "square root" operator:

$$H_{\text{SOSSA}} = \sum_{j=0}^{R-1} |j\rangle \otimes B_j = \begin{pmatrix} B_0 \\ B_1 \\ \vdots \\ B_{R-1} \end{pmatrix}$$

so that $H_{\text{SOSSA}}^\dagger H_{\text{SOSSA}} = H + \beta\mathbb{1}$. Block-encode $H_{\text{SOSSA}}/\sqrt{\lambda_{\text{SOS}}}$ via the construction in Lemma 3 (one call to each controlled $\mathrm{Be}[B_j/\|B_j\|]$ plus a PREPARE unitary loading the coefficients). Then use Lemma 4 to get a self-inverse block-encoding of $H'/(2\lambda) - \mathbb{1}$ from a single query to $\mathrm{Be}[H_{\text{SOSSA}}/\sqrt{\lambda_{\text{SOS}}}]$ and its inverse.

The [[Qubitization Iterate|qubitization]] walk operator on this block-encoding has eigenphases $\pm\arccos(E_j/\lambda - 1)$. Near the spectral boundary (small $E_j$), $\arccos$ is steep — a coarse phase estimate gives a fine energy estimate. This is the spectral amplification mechanism, generalising [[Spectral Gap Amplification (Somma-Boixo 2013) — Paper Notes|Somma-Boixo (2013)]] and [[Hamiltonian Simulation by Uniform Spectral Amplification (Low-Chuang 2017) — Paper Notes|Low-Chuang (2017)]].

### Step 3: Adaptive estimation (no prior on $\Delta$)

The algorithms for energy estimation (Theorem 5) and phase estimation (Theorem 7) don't require advance knowledge of $\Delta_{\text{SOS}}$. They use a two-step adaptive binary search:

1. **Coarse step:** [[Gapped Phase Estimation]] at accuracy $\varepsilon' = \Theta(\sqrt{\varepsilon/\lambda})$. This either certifies that $E \approx -\lambda$ (terminate) or gives a constant-factor multiplicative estimate of $\lambda + E$.
2. **Fine step:** Geometrically decreasing accuracy rounds with union-bounded failure probabilities, using the coarse estimate to set the number of rounds.

See [[Adaptive Spectral-Amplified Energy Estimation]] for the full trick.

---

## Key results

**Table I query complexities** (queries to block-encodings):

| Task | LCU | SOSSA |
|------|-----|-------|
| Energy estimation | $\Theta(\lambda/\varepsilon)$ | $\Theta(\sqrt{\Delta_{\text{SOS}} \lambda_{\text{SOS}}}/\varepsilon)$ |
| Phase estimation (overlap $p$) | $O\!\left(\frac{\lambda}{\sqrt{p}\,\varepsilon}\log\frac{1}{p}\right)$ | $O\!\left(\frac{\sqrt{\Delta_{\text{SOS}} \lambda_{\text{SOS}}}}{\sqrt{p}\,\varepsilon}\log\frac{1}{p}\right)$ |
| Time evolution | $\Theta(\lambda t + \log\frac{1}{\varepsilon})$ | $\Theta\!\left(\sqrt{\Delta_{\text{SOS}} \lambda_{\text{SOS}}}\,t + \sqrt{\frac{\lambda_{\text{SOS}}}{\Delta_{\text{SOS}}}}\log\frac{1}{\varepsilon}\right)$ |

**Theorem 5** (adaptive energy estimation): Without prior knowledge of $\Delta$, estimates $E = \langle\psi|H|\psi\rangle$ to error $\varepsilon$ with $O\!\left(\frac{\sqrt{\max\{\varepsilon, \lambda - |E|\}\cdot\lambda}}{\varepsilon}\log\frac{1}{q}\right)$ queries. Uses only 2 ancillary qubits beyond the block-encoding.

**Theorem 7** (adaptive phase estimation): Same adaptive flavour for ground-state energy. Query complexity $O\!\left(\frac{\sqrt{\max\{\varepsilon, \lambda + E\}\cdot\lambda}}{\sqrt{p}\,\varepsilon}\log\frac{1}{p}\log\frac{1}{q}\right)$.

**Theorem 8** (lower bound): $\Omega(\sqrt{\lambda\Delta}/\varepsilon)$ queries for phase estimation with spectral amplification, proved via reduction from the $K$-partition PARITY$\circ$OR problem. The upper bounds are tight.

**Corollary 2** (amplified amplitude estimation): For a projector $\Pi$, estimates $\langle\psi|\Pi|\psi\rangle$ to error $\varepsilon$ with $O\!\left(\frac{\sqrt{\max\{\varepsilon, \langle\psi|\Pi|\psi\rangle(1 - \langle\psi|\Pi|\psi\rangle)\}}}{\varepsilon}\log\frac{1}{q}\right)$ queries — improves on prior amplified amplitude estimation (Simon et al., 2024) by removing the requirement for a known upper bound $\Delta$.

---

## Comparison with prior work

| Method | Energy est. queries | Phase est. queries | Prior on $\Delta$ needed? |
|--------|--------------------|--------------------|--------------------------|
| Standard LCU (Knill-Ortiz-Somma '07, Berry et al. '18) | $\Theta(\lambda/\varepsilon)$ | $O(\lambda/\sqrt{p}\varepsilon)$ | No |
| Amplified amplitude est. (Simon et al. '24) | $O(\sqrt{\Delta\lambda}/\varepsilon \cdot \Delta/(\Delta - \lambda - E))$ | — | Yes |
| SOSSA Thm 4 (non-adaptive) | $O(\sqrt{\Delta\lambda}/\varepsilon)$ | $O(\sqrt{\Delta\lambda}/\varepsilon)$ | Yes |
| **SOSSA Thm 5/7 (adaptive)** | $O(\sqrt{\max\{\varepsilon, \Delta\}\lambda}/\varepsilon)$ | $O(\sqrt{\max\{\varepsilon, \Delta\}\lambda}/\sqrt{p}\varepsilon)$ | **No** |
| SA for time evolution (Zlokapa-Somma '24) | — | — | Yes |

Key improvements over prior art: (i) adaptive algorithms don't need $\Delta$ in advance; (ii) the amplified amplitude estimation result (Corollary 2) drops the multiplicative penalty $\Delta/(\Delta - \lambda - E)$ from Simon et al.; (iii) the phase estimation result generalises [[Fast Quantum Simulation of Electronic Structure by Spectrum Amplification (Low, King, Berry, Babbush, Somma, Rubin 2025) — Paper Notes|Low et al. (2025)]] to $p < 1$ and unknown $\Delta$.

---

## Application to SYK

The [[Quantum Simulation of the Sachdev-Ye-Kitaev Model by Asymmetric Qubitization (Babbush, Berry, Neven 2019) — Paper Notes|SYK model]]:

$$H_{\text{SYK}} = \frac{1}{\sqrt{\binom{N}{4}}} \sum_{a,b,c,d} g_{abcd} \gamma_a\gamma_b\gamma_c\gamma_d, \quad g_{abcd} \sim \mathcal{N}(0,1)$$

**Degree-2 Majorana SOS:** Using $B_j$ operators that are quadratic in the Majorana operators $\gamma_a$, the SDP gives $\beta = O(N)$ (Lemma 8, proved via the dual pseudomoment problem + random matrix theory: $\|J\| = O(N^{-1})$ w.h.p.).

Since the SYK spectrum lies in $[-c\sqrt{N}, c\sqrt{N}]$, the energy gap is $\Delta_{\text{SOS}} = O(N)$.

**Block-encoding via [[Double Factorisation for Block-Encoding|double factorization]]:** Each $B_j$ has a quadratic Majorana part that can be diagonalised by Gaussian unitaries (orthogonal rotation of the Majorana vector). This gives $\lambda_{\text{SOS}} = O(N^2)$ — see [[Majorana SOS with Double Factorization]].

| Quantity | LCU | Termwise SA | SOSSA (degree-2) |
|----------|-----|------------|-------------------|
| $\lambda$ | $N^2$ | $N^2$ | — |
| $\sqrt{\Delta\lambda}$ | — | $\sim N^2$ | $\sim N^{3/2}$ |
| Gate cost per query | $\Theta(N^4)$ | $\Theta(N^4)$ | $\Theta(N^4)$ |
| **Effective query norm** | $N^2$ | $N^2$ | $N^{3/2}$ |

**Asymptotic speedup: $\sqrt{N}$** in query complexity, which carries through to gate complexity since the per-query costs are all $\Theta(N^4)$.

**Degree-3 SOS doesn't help further:** Hastings-O'Donnell (2022) showed degree-3 Majorana SOS achieves $\beta = \Omega(\sqrt{N})$ (tighter), but $\lambda_{\text{SOS}} \sim N^{7/2}$ (much worse). The product $\sqrt{\Delta_{\text{SOS}} \lambda_{\text{SOS}}} = O(N^2)$ recovers the LCU scaling. The optimal SOS degree is system-dependent.

---

## Limits / caveats

1. **The product $\Delta \cdot \lambda$ is what matters, not $\Delta$ alone.** Tighter SOS bounds (larger algebra) give smaller $\Delta$ but larger $\lambda$. Increasing the SOS degree can make things *worse* — as the degree-3 SYK example shows.

2. **The block-encoding cost per query may increase.** More complex $B_j$ operators require more gates per query. The improvement only holds when the query reduction outweighs the per-query cost increase. For SYK at degree-2, the costs are comparable; for other systems this isn't guaranteed.

3. **Classical preprocessing cost.** The SDP scales as $\text{poly}(L)$ where $L = |\vec{X}^{(k)}|$ is the operator algebra size. For degree-$k$ Pauli/Majorana algebras on $N$ qubits/modes, $L = O(N^k)$. The SDP itself is polynomial-time but the constants matter for real instances.

4. **Random-instance statement for SYK.** The $\beta = O(N)$ bound (Lemma 8) holds with high probability over the random disorder $g_{abcd}$. The proof uses $\|J\| = O(N^{-1})$ from random matrix theory.

5. **No improvement for frustrated systems with poor SOS bounds.** If $\Delta_{\text{SOS}} \sim \lambda_{\text{LCU}}$ (the SDP can't do much better than termwise shifting), SOSSA reduces to standard LCU cost.

6. **Time evolution has a mixed result.** The $\sqrt{\Delta\lambda}\,t$ term improves, but the $\sqrt{\lambda/\Delta}\log(1/\varepsilon)$ term can *worsen* — the precision dependence degrades when $\Delta \ll \lambda$. This was already noted in Zlokapa-Somma (2024).

---

## Reusable ideas

1. [[SOS Spectral Amplification]] — The core SOSSA trick: SOS decomposition + rectangular "square root" operator + arccos nonlinearity for amplification.

2. [[Adaptive Spectral-Amplified Energy Estimation]] — Two-step binary search using [[Gapped Phase Estimation]] that removes the need for prior knowledge of $\Delta$, achieving instance-optimal query complexity.

3. [[SOS Lower Bound via Gram Matrix SDP]] — Classical preprocessing: express $H + \beta\mathbb{1} = \vec{X}^{(k)\dagger} G \vec{X}^{(k)}$ as an SDP, optimise $\beta$; dual is the pseudomoment problem; increasing algebra degree tightens the bound.

4. [[Majorana SOS with Double Factorization]] — For fermionic Hamiltonians: degree-2 Majorana SOS generators, diagonalised via Gaussian unitaries (orthogonal rotation of Majorana vector), giving efficient block-encodings with $\lambda_{\text{SOS}}$ controlled by Cauchy-Schwarz.

---

## References within this paper

- **[1]** Knill, Ortiz, Somma (2007) — energy estimation from block-encodings; $O(\lambda/\varepsilon)$ baseline
- **[2]** [[Rapid Initial State Preparation for the Quantum Simulation of Strongly Correlated Molecules (Berry, Tong, Babbush, Rubin et al 2024) — Paper Notes|Berry, Tong, Babbush, Rubin et al. (2024)]] — phase estimation from block-encoding; state prep comparison
- **[3]** [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|Low & Chuang (2017)]] — QSP for Hamiltonian simulation
- **[4]** [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes|Low & Chuang (2019)]] — [[Qubitization Iterate|qubitization]] framework
- **[5]** Zlokapa & Somma (2024) — SA for time evolution; precursor results used here
- **[9]** [[Exponential Improvement in Precision for Simulating Sparse Hamiltonians (Berry-Childs-Cleve-Kothari-Somma 2014) — Paper Notes|Berry, Childs, Cleve, Kothari, Somma (2015)]] — LCU method
- **[14]** [[Spectral Gap Amplification (Somma-Boixo 2013) — Paper Notes|Somma & Boixo (2013)]] — original spectral gap amplification for frustration-free Hamiltonians
- **[15]** [[Hamiltonian Simulation by Uniform Spectral Amplification (Low-Chuang 2017) — Paper Notes|Low & Chuang (2017)]] — uniform spectral amplification via QSP
- **[16]** [[Fast Quantum Simulation of Electronic Structure by Spectrum Amplification (Low, King, Berry, Babbush, Somma, Rubin 2025) — Paper Notes|Low, King, Berry et al. (2025)]] — chemistry companion paper applying SOSSA
- **[17]** Simon, Degroote, Moll, Santagati, Streif, Wiebe (2024) — amplified amplitude estimation; prior work improved here
- **[23]** [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|Gilyén, Su, Low, Wiebe (2019)]] — [[QSVT Meta-Template|QSVT]]
- **[30]** von Burg, Low, Häner, Steiger, Reiher, Roetteler, Troyer (2021) — double factorization of Coulomb integrals
- **[31]** Hastings & O'Donnell (2022) — degree-3 SOS for SYK; shows limitations of higher-degree SOS
- **[39]** [[Amplitude Amplification and Estimation|Brassard, Hoyer, Mosca, Tapp (2002)]] — amplitude estimation baseline
- **[40]** Low & Su (2024) — gapped phase estimation with optimal query complexity
- **[48]** [[Application of Fermionic Marginal Constraints to Hybrid Quantum Algorithms (Rubin, Babbush, McClean 2018) — Paper Notes|Rubin, Babbush, McClean (2018)]] — fermionic marginal constraints; related SOS/SDP approach

---

## My assessment

This is a clean, well-structured theory paper that gives SOSSA a proper self-contained foundation. The PRL format forced the authors to be concise — the core idea fits in 5 pages with all the heavy lifting in appendices.

The adaptive energy estimation (Theorem 5) is the standout algorithmic contribution. The two-step binary search is genuinely clever: the first step at accuracy $\sqrt{\varepsilon/\lambda}$ simultaneously handles both cases (energy near the boundary → terminate; energy away → get a multiplicative estimate) without any prior. The union-bound engineering to get $O(\log(1/q))$ confidence scaling instead of $O(\log\log(\lambda/\varepsilon)/q)$ is a nice technical touch.

The SYK application is a good demonstration but the real value of SOSSA for practical quantum computing is in quantum chemistry, as shown in the companion paper. The degree-2 Majorana SOS happens to hit the sweet spot for SYK — it's not obvious this will generalise easily to other condensed matter systems. The paper's honest treatment of the degree-3 failure case (where tightening $\beta$ doesn't help because $\lambda_{\text{SOS}}$ grows too fast) is appreciated.

The lower bounds (Theorem 8) via PARITY$\circ$OR are satisfying — they show SOSSA is query-optimal in the low-energy setting, not just an improvement over LCU. The reduction constructs Hamiltonians where SA is the natural approach and shows you can't do better, which closes the theoretical story cleanly.

One thing worth noting: this paper and the companion paper together represent a genuinely new paradigm for quantum simulation resource estimation. The idea that *classical SOS relaxation quality controls quantum speedup* creates a productive interplay — better classical bounds → better quantum algorithms. The SOS hierarchy from classical optimisation gets reinterpreted as a knob for quantum algorithm design.

---

## Cross-links

### Paper notes
- [[SOSSA — Sum-of-Squares Spectral Amplification]] — earlier informal note on the same paper (2505.01528)
- [[Fast Quantum Simulation of Electronic Structure by Spectrum Amplification (Low, King, Berry, Babbush, Somma, Rubin 2025) — Paper Notes]] — companion chemistry application paper
- [[Hamiltonian Simulation by Uniform Spectral Amplification (Low-Chuang 2017) — Paper Notes]] — precursor: uniform spectral amplification via QSP
- [[Spectral Gap Amplification (Somma-Boixo 2013) — Paper Notes]] — original spectral gap amplification for frustration-free systems
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]] — qubitization framework used throughout
- [[Quantum Simulation of the Sachdev-Ye-Kitaev Model by Asymmetric Qubitization (Babbush, Berry, Neven 2019) — Paper Notes]] — prior SYK simulation; SOSSA improves the query complexity by $\sqrt{N}$
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]] — QSVT machinery underlying the walk operator
- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes]] — QSP for polynomial transformations
- [[Application of Fermionic Marginal Constraints to Hybrid Quantum Algorithms (Rubin, Babbush, McClean 2018) — Paper Notes]] — related SOS/RDM approach
- [[Compressing Many-Body Fermion Operators under Unitary Constraints (Rubin, Lee, Babbush 2021) — Paper Notes]] — related sum-of-squares decomposition for fermion operators

### Trick cards
- [[SOS Spectral Amplification]] — the main SOSSA trick
- [[Adaptive Spectral-Amplified Energy Estimation]] — adaptive binary search, no prior on $\Delta$
- [[SOS Lower Bound via Gram Matrix SDP]] — classical SDP for SOS decomposition
- [[Majorana SOS with Double Factorization]] — fermionic SOS compilation trick
- [[Gapped Phase Estimation]] — subroutine used in the adaptive algorithms
- [[Rectangular Block-Encoding for Spectrum Amplification]] — block-encoding technique
- [[SOS Decomposition via Semidefinite Programming for Chemistry]] — related SDP-based SOS
- [[Double Factorisation for Block-Encoding]] — double factorization used for SYK compilation
- [[Qubitization Iterate]] — walk operator construction
- [[Frustration-Free Gap Amplification via Walk Operator]] — Somma-Boixo precursor trick
- [[Uniform Spectral Amplification of Low-Energy Subspaces]] — Low-Chuang precursor trick
