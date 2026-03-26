> **Source:** Kianna Wan, William J. Huggins, Joonho Lee, Ryan Babbush, *Matchgate Shadows for Fermionic Quantum Simulation*, arXiv:2207.13723, Communications in Mathematical Physics **404**, 629–700 (2023)
> **Links:** [arXiv](https://arxiv.org/abs/2207.13723) · [Journal](https://doi.org/10.1007/s00220-023-04844-0)
> **Tags:** #classical-shadows #matchgate #fermionic-Gaussian #measurement #AFQMC #QMC #shadow-tomography #t-design #Slater-determinant #overlap-estimation

---

## The computational problem

Given copies of an unknown $n$-qubit state $\rho$, estimate expectation values $\operatorname{tr}(O_i \rho)$ for a collection of observables $\{O_i\}$ that are natural to fermionic quantum simulation: products of Majorana operators, fermionic Gaussian density matrices, and overlaps with Slater determinants.

The classical shadows framework of Huang, Kueng, and Preskill (2020) solves this generically by randomizing over unitaries, measuring in the computational basis, and classically post-processing the results. But the efficiency — both in sample complexity and classical post-processing cost — depends on the choice of unitary ensemble. The question: is there an ensemble tailored to fermionic problems that is efficient for both local fermionic observables *and* global properties like Gaussian-state fidelities and Slater determinant overlaps?

The motivation is concrete: the [[Unbiasing Fermionic Quantum Monte Carlo with a Quantum Computer (Huggins, Babbush et al 2021) — Paper Notes|QC-AFQMC algorithm]] of Huggins et al. (2021) uses Clifford classical shadows to estimate trial-walker overlaps $\langle \Psi_T | \phi_i \rangle$. The Clifford approach has $O(1)$ variance (good), but the classical post-processing to extract each overlap estimate from a Clifford shadow costs time *exponential* in $n$ (bad). This paper fixes that bottleneck.

## What the paper does

Introduces **matchgate shadows**: classical shadows constructed from random matchgate circuits, which are the qubit representations of fermionic Gaussian unitaries under the [[Bravyi-Kitaev Transformation|Jordan-Wigner transformation]]. The paper analyzes two ensembles — the continuous Haar distribution over all matchgate circuits $\mathcal{M}_n$ and the discrete uniform distribution over Clifford matchgate circuits $\mathcal{M}_n \cap \mathrm{Cl}_n$ — and proves their first three moments are identical. This "matchgate 3-design" property means both ensembles yield functionally equivalent classical shadows.

The main payoff: efficient classical post-processing ($O(n^3)$ to $O(n^4)$ per shadow sample) for estimating:

1. **Expectation values of local fermionic operators** — $\operatorname{tr}(\tilde{\gamma}_S \rho)$ for any product of $|S|$ Majorana operators, with variance $O(n^{|S|/2})$.
2. **Fidelities with Gaussian states** — $\operatorname{tr}(\varrho \rho)$ for any fermionic Gaussian density matrix $\varrho$, with variance $O(\sqrt{n} \log n)$.
3. **Overlaps with Slater determinants** — $\langle \psi | \phi \rangle$ for any pure state $|\psi\rangle$ and Slater determinant $|\phi\rangle$, with an explicitly computable variance bound that grows sublinearly in $n$ (numerically confirmed up to $n = 1000$).

This is a qualitative advantage over standard Clifford shadows, which force a choice: single-qubit Cliffords handle local observables efficiently but not global ones, while $n$-qubit Cliffords handle global properties but lose locality. Matchgate shadows do both simultaneously.

My assessment: this is a technically strong paper that solves a real problem. The matchgate 3-design result is clean and the Clifford algebra / Grassmann integral proof machinery is well-deployed. The direct practical impact is removing the exponential post-processing bottleneck from [[Unbiasing Fermionic Quantum Monte Carlo with a Quantum Computer (Huggins, Babbush et al 2021) — Paper Notes|QC-AFQMC]], making that algorithm genuinely scalable on the classical side. The broader contribution is establishing matchgate shadows as a fermion-native measurement primitive.

## The algorithm / construction

### Classical shadows recap

The general protocol: given copies of $\rho$, sample a unitary $U$ from a distribution $\mathcal{D}$, apply $U$, measure in the computational basis to get outcome $|b\rangle$, and store $(U, b)$. The measurement channel is:

$$\mathcal{M}(\rho) = \mathbb{E}_{U \sim \mathcal{D}} \sum_b \langle b | U \rho U^\dagger | b \rangle \, U^\dagger | b \rangle \langle b | U$$

The classical shadow estimator is $\hat{\rho} = \mathcal{M}^{-1}(U^\dagger |b\rangle\langle b| U)$. Expectation value estimates $\hat{o}_i = \operatorname{tr}(O_i \hat{\rho})$ are unbiased: $\mathbb{E}[\hat{o}_i] = \operatorname{tr}(O_i \rho)$.

The measurement channel $\mathcal{M}$ depends on the 2-fold twirl $\mathbb{E}_{U} \mathcal{U}^{\otimes 2}$; the variance depends on the 3-fold twirl $\mathbb{E}_{U} \mathcal{U}^{\otimes 3}$.

### Matchgate circuits

A matchgate circuit $U_Q \in \mathcal{M}_n$ corresponds to an orthogonal matrix $Q \in O(2n)$ via:

$$U_Q^\dagger \gamma_\mu U_Q = \sum_{\nu=1}^{2n} Q_{\mu\nu} \gamma_\nu$$

where $\gamma_1, \ldots, \gamma_{2n}$ are the [[Bravyi-Kitaev Transformation|Majorana operators]] defined by the Jordan-Wigner transformation. These circuits are generated by nearest-neighbour $XX$ rotations $e^{i\theta X \otimes X}$ and single-qubit $Z$ rotations on a line, plus $X_n$ on the last qubit.

The discrete Clifford matchgate group $\mathcal{M}_n \cap \mathrm{Cl}_n$ corresponds to the signed permutation matrices $B(2n) \subset O(2n)$.

### The matchgate 3-design (Theorem 1)

The first three moments of the Haar distribution over $\mathcal{M}_n$ and the uniform distribution over $\mathcal{M}_n \cap \mathrm{Cl}_n$ coincide:

$$\mathcal{E}^{(j)}_{\mathcal{M}_n} = \mathcal{E}^{(j)}_{\mathcal{M}_n \cap \mathrm{Cl}_n}, \quad j \in \{1, 2, 3\}$$

The proof strategy: show both twirl channels are orthogonal projectors (from the group structure), find their images by (a) using sign symmetries to eliminate basis elements, (b) showing the remaining basis elements within each $\Gamma_k$ sector contribute uniformly, and (c) verifying the putative fixed-point vectors are genuinely fixed using the Cauchy-Binet formula (for $j = 2$) or a generator-based argument (for $j = 3$).

The explicit expressions involve the symmetric vectors $|\Upsilon^{(2)}_k\rangle\!\rangle$ and $|\Upsilon^{(3)}_{k_1,k_2,k_3}\rangle\!\rangle$ built from Majorana products, decomposed according to the $2n+1$ irreducible representations of the matchgate group. The matchgate group has $2n + 1$ irreps (indexed by the subspaces $\Gamma_k$), compared to the Clifford group's single non-trivial irrep — this is what makes the matchgate analysis more involved.

### Measurement channel

Substituting the 2-fold twirl into the shadow formula:

$$\mathcal{M} = \sum_{\ell=0}^{n} \binom{n}{\ell} \binom{2n}{2\ell}^{-1} P_{2\ell}$$

where $P_k$ projects onto $\Gamma_k$ (products of exactly $k$ Majorana operators). The image of $\mathcal{M}$ is $\Gamma_{\text{even}} = \bigoplus_{\ell=0}^n \Gamma_{2\ell}$, so matchgate shadows produce unbiased estimates whenever either $\rho$ or the observables are even operators. This is natural for fermionic systems — physical observables conserve fermionic parity.

The inverse channel:

$$\mathcal{M}^{-1} = \sum_{\ell=0}^{n} \binom{2n}{2\ell} \binom{n}{\ell}^{-1} P_{2\ell}$$

### Efficient post-processing for local fermionic operators

For $O = \tilde{\gamma}_S$ (product of $|S|$ Majorana operators in some rotated basis $Q'$), the shadow estimate reduces to:

$$\operatorname{tr}\!\left(\tilde{\gamma}_S \, \mathcal{M}^{-1}(U_Q^\dagger |b\rangle\langle b| U_Q)\right) = \binom{2n}{|S|} \binom{n}{|S|/2}^{-1} \operatorname{pf}\!\left(i(Q' Q^T C_{|b\rangle} Q Q'^T)\big|_S\right)$$

This is an $O(|S|^3)$ Pfaffian computation. Variance: $O(n^{|S|/2})$ for constant $|S|$.

### Efficient post-processing for Gaussian state fidelities (Theorem 2)

For a Gaussian state $\varrho$ with covariance matrix $C_\varrho$, the projection $\operatorname{tr}(\varrho \, P_{2\ell}(\varrho_2))$ onto the $2\ell$-Majorana sector is the coefficient of $z^\ell$ in:

$$p_{\varrho, \varrho_2}(z) = \frac{1}{2^n} \operatorname{pf}(C'_\varrho) \, \operatorname{pf}\!\left(-C'^{-1}_\varrho + z (Q_1 C_{\varrho_2} Q_1^T)\big|_{[2r]}\right)$$

where $2r = \operatorname{rank}(C_\varrho)$. Computing all coefficients takes $O(r^3)$ via a derivative-based method that exploits the linear dependence of the Pfaffian's argument on $z$ (Appendix D). This uses $\partial^\ell \operatorname{pf}(A(z)) / \partial z^\ell$ via the recursion from $\operatorname{pf}(A)^2 = \det(A)$ and Jacobi's formula.

### Efficient post-processing for Slater determinant overlaps (Theorem 3)

For a $\zeta$-fermion Slater determinant $|\phi\rangle = \tilde{a}_1^\dagger \cdots \tilde{a}_\zeta^\dagger |0\rangle$ (specified by the orbital rotation matrix $V$), $\operatorname{tr}(|\phi\rangle\langle 0| \, P_{2\ell}(\varrho))$ is the coefficient of $z^\ell$ in:

$$q_{|\phi\rangle, \varrho}(z) = \frac{i^{\zeta/2}}{2^{n-\zeta/2}} \operatorname{pf}\!\left(C_{|0\rangle} + z W^* \tilde{Q} C_\varrho \tilde{Q}^T W^\dagger\right)\big|_{S_\zeta}$$

where $\tilde{Q}$ encodes the orbital rotation, $W$ is a basis change matrix, and $S_\zeta = [2n] \setminus \{1, 3, \ldots, 2\zeta - 1\}$. The matrix has size $2n - \zeta$, so polynomial interpolation gives all coefficients in $O((n - \zeta/2)^4)$ time.

The proof works by interpreting Majorana operators as Clifford algebra generators, using graded structure and the wedge product to isolate grade-$2\ell$ components, then applying Pfaffian identities (Fact 3) to evaluate the resulting exterior algebra expression.

### General framework via Grassmann integrals (Section V C)

For arbitrary products $A^{(1)} \cdots A^{(m)}$ where each factor is a linear combination of Majorana operators, a Gaussian unitary, or a Gaussian density matrix:

1. Map the trace $\operatorname{tr}(A^{(1)} \cdots A^{(m)})$ to a Grassmann integral via Theorem 4, using $m$ independent sets of $2n$ Grassmann variables.
2. Each Gaussian density matrix contributes an exponential $\exp(\frac{1}{2} \theta^T C \theta)$; each Gaussian unitary contributes similarly after Hamiltonian extraction.
3. Assemble into the standard form $g(B, M) = \int D\chi \, (B\chi)_1 \cdots (B\chi)_K \exp(\frac{1}{2} \chi^T M \chi)$.
4. Evaluate via Algorithm 2, which handles both invertible $M$ (direct Pfaffian formula: $g = \operatorname{pf}(M) \operatorname{pf}(-B M^{-1} B^T)$) and singular $M$ (recursive reduction).

This handles overlaps with arbitrary pure Gaussian states (not just Slater determinants) and opens the door to more exotic observables.

## Key results

| Quantity | Efficient post-processing? | Variance bound | Sample complexity for $\varepsilon$ precision |
|---|---|---|---|
| $\operatorname{tr}(\tilde{\gamma}_S \rho)$, $\|S\| = O(1)$ | $O(\|S\|^3)$ per sample | $O(n^{\|S\|/2})$ | $O(n^{\|S\|/2} / \varepsilon^2)$ |
| $\operatorname{tr}(\varrho \, \rho)$, $\varrho$ Gaussian | $O(n^3)$ per sample | $O(\sqrt{n} \log n)$ | $O(\sqrt{n} \log n / \varepsilon^2)$ |
| $\langle \psi | \phi \rangle$, $\|\phi\rangle$ Slater det. | $O(n^4)$ per sample | $\leq b(n, \zeta)$, sublinear in $n$ | Sublinear in $n$ |
| $\langle \psi | \varphi \rangle$, $\|\varphi\rangle$ pure Gaussian | $O(n^4)$ per sample | Not bounded (open) | — |

The variance bound $b(n, \zeta)$ is given explicitly by a triple sum involving multinomial coefficients (Eqs. 43–44). It's computable in $\operatorname{poly}(n)$ time and plotted up to $n = 1000$ in the paper, showing growth consistent with $O(\sqrt{n} \log n)$ or better across all tested $\zeta$ values.

### Matchgate 3-design

$$\mathcal{E}^{(j)}_{\mathcal{M}_n} = \mathcal{E}^{(j)}_{\mathcal{M}_n \cap \mathrm{Cl}_n} \quad \text{for } j = 1, 2, 3$$

The discrete ensemble ($|B(2n)| = 2^{2n} (2n)!$ elements) is a 3-design for the continuous matchgate group. This is analogous to the Clifford group being a unitary 3-design, but for the matchgate subgroup.

### Measurement channel identity

Both ensembles yield the same measurement channel. The same holds for the variance. So in practice, one can use whichever ensemble is easier to sample and implement on hardware.

## Comparison with prior work

| Shadow ensemble | Local fermionic observables | Gaussian fidelities | Slater det. overlaps | Classical post-processing |
|---|---|---|---|---|
| Single-qubit Clifford | Efficient (local qubit, not fermionic) | Inefficient | Inefficient | $O(1)$ per sample |
| $n$-qubit Clifford | Efficient (low-rank observables) | $O(1)$ variance | $O(1)$ variance | **Exponential** in $n$ |
| Zhao et al. (2021) | $O(n^{k/2})$ variance (canonical basis only) | Not addressed | Not addressed | $O(n^3)$ |
| **Matchgate shadows (this paper)** | $O(n^{k/2})$ variance (arbitrary basis) | $O(\sqrt{n} \log n)$ variance | Sublinear variance | $O(n^3)$–$O(n^4)$ |
| Low (2022), number-conserving | $O(\zeta^k)$ average variance for $k$-RDMs | Not directly comparable | Unclear worst-case | $O(n^3)$ |

The paper subsumes Zhao et al.'s results and goes substantially beyond them. O'Gorman (2022) addresses the same overlap estimation problem but without the matchgate 3-design result, leaving their variance analysis incomplete (the gap between Lemma 2 and Theorem 4 in that work). Low (2022) uses number-conserving Gaussian unitaries and achieves better *average-case* variance for $k$-RDMs, but the worst-case variance for overlap estimation is not bounded, and the protocol requires $\zeta$ ancillary modes.

## Limits / caveats

1. **Odd parity:** Matchgate shadows only yield unbiased estimates for even operators (or even states). Overlaps with odd-fermion-number Slater determinants require adding ancilla qubits to make the observable even. This is a minor overhead ($+1$ or $+2$ qubits) but adds implementation complexity.

2. **Variance for general Gaussian overlaps:** The paper bounds the variance for Gaussian density matrices and Slater determinant overlaps, but not for overlaps with arbitrary pure Gaussian states (via the Grassmann integral framework of Section V C). This is flagged as an open problem.

3. **No exponential separation from Clifford shadows:** The practical advantage is in classical post-processing cost ($\operatorname{poly}(n)$ vs. $\exp(n)$ for Clifford shadows in the overlap estimation setting), not in sample complexity. The Clifford shadow approach already has $O(1)$ variance for overlaps — it just can't compute the estimates efficiently.

4. **Circuit depth for continuous ensemble:** Sampling from the Haar measure over $O(2n)$ and compiling into a matchgate circuit requires $O(n^2)$ two-qubit gates in depth $O(n)$. This is deeper than single-qubit Clifford shadows but comparable to $n$-qubit Clifford circuits.

5. **Open: higher moments?** The 3-design result does not extend to higher moments — the paper asks whether an analogous result holds for $\mathcal{M}_n^* = \{U_Q : Q \in SO(2n)\}$ (parity-conserving subgroup). The 1-fold twirls already differ between $\mathcal{M}_n$ and $\mathcal{M}_n^*$.

6. **No noise robustness analysis:** The paper doesn't address what happens under hardware noise. Bravyi, Gosset, and Grier (2021) developed noise-robust Clifford shadows via randomized benchmarking connections. Whether similar techniques work for matchgate shadows is open. (The paper mentions this in the conclusion.)

## Reusable ideas

1. [[Matchgate 3-Design for Classical Shadows]] — Clifford matchgates $\mathcal{M}_n \cap \mathrm{Cl}_n$ (signed permutation matrices on Majorana operators) form a 3-design for the continuous matchgate group. This makes the discrete ensemble interchangeable with the continuous one for any application depending on up to the third moment.

2. [[Pfaffian Shadow Estimation for Fermionic Observables]] — Expectation values of Majorana products with respect to matchgate shadow samples reduce to Pfaffians of covariance-matrix submatrices: $O(|S|^3)$ per sample. The key identity: $\operatorname{tr}(\tilde{\gamma}_S \, \mathcal{M}^{-1}(U_Q^\dagger |b\rangle\langle b| U_Q))$ is a ratio of binomial coefficients times a Pfaffian of a restricted covariance matrix.

3. [[Generating-Function Pfaffian Trick for Gaussian Overlaps]] — Introduce a formal variable $z$ in front of the Gaussian state's bivectors, turning the projection $\operatorname{tr}(\varrho \, P_{2\ell}(\varrho_2))$ into the $z^\ell$ coefficient of a Pfaffian-valued polynomial. Computing all coefficients via Jacobi's formula recursion takes $O(r^3)$ time.

4. [[Grassmann Integral Framework for Free-Fermion Traces]] — Express $\operatorname{tr}(A^{(1)} \cdots A^{(m)})$ as a Grassmann integral using independent generator sets per operator, with an exponential coupling term. When each $A^{(i)}$ is Gaussian or a Majorana product, the integral reduces to the efficiently computable form $g(B, M)$, evaluated via Algorithm 2 (Pfaffians + recursive rank reduction).

## References within this paper

Key citations and their role:

- **Huang, Kueng, Preskill (2020)** — The classical shadows framework this paper builds on. Introduced the general protocol and analyzed Clifford ensembles.
- [[Unbiasing Fermionic Quantum Monte Carlo with a Quantum Computer (Huggins, Babbush et al 2021) — Paper Notes|Huggins, O'Gorman, Rubin, Reichman, Babbush, Lee (2021)]] — The [[Shadow Tomography for QMC Overlap Estimation|QC-AFQMC]] algorithm whose exponential post-processing bottleneck this paper removes.
- **Zhao, Rubin, Miyake (2021)** — Prior work on matchgate shadows from the alternating group $A(2n)$. Handles canonical-basis local observables but not overlaps or arbitrary-basis observables. The measurement channel turns out to be the same, despite different twirl channels.
- **O'Gorman (2022)** — Concurrent work on overlap estimation via matchgate shadows. Lacks the 3-design result, leaving a gap in the variance analysis for arbitrary Slater determinants.
- **Low (2022)** — Number-conserving fermionic Gaussian shadows. Better average-case variance for $k$-RDMs but unclear worst-case for overlaps; requires $\zeta$ ancillas.
- **Helsen, Nezami, Reagor, Walter (2021)** — Matchgate randomized benchmarking. The connection between shadows and benchmarking could yield noise-robust matchgate shadows (open question from this paper).
- **Webb (2016), Zhu (2017)** — Proofs that the Clifford group is a unitary 3-design. The matchgate 3-design result is the subgroup analogue.
- **Bravyi (2005)** — The $2n+1$ irreducible representations of the matchgate group. Explains why the matchgate analysis is harder than the Clifford case (one irrep vs. $2n + 1$).
- **Terhal & DiVincenzo (2002); Knill (2001)** — Free-fermion classical simulation. The post-processing methods here are new classical simulation results for free-fermion quantities.
- [[Nearly Optimal Quantum Algorithm for Estimating Multiple Expectation Values (Huggins, Wan, McClean, Babbush et al 2022) — Paper Notes|Huggins, Wan, McClean, O'Brien, Wiebe, Babbush (2022)]] — Same first and second authors (Wan, Huggins). The gradient-based multi-observable estimation approach, which operates in a different (fault-tolerant, high-precision) regime than classical shadows.

## Cross-links

### Paper notes in the vault

- [[Triply Efficient Shadow Tomography (King, Gosset, Kothari, Babbush 2024) — Paper Notes]] — Proves the $\Omega(n^k/\epsilon^2)$ single-copy lower bound that matchgate shadows saturate, then shows two-copy measurements beat it exponentially: $O((\log n) \cdot \mathrm{poly}_k(1/\epsilon))$ samples. The two-copy protocol supersedes matchgate shadows in sample complexity (as a function of $n$) for all $k$-body fermionic operators.
- [[Unbiasing Fermionic Quantum Monte Carlo with a Quantum Computer (Huggins, Babbush et al 2021) — Paper Notes]] — The direct motivation. Matchgate shadows replace the exponentially costly Clifford shadow post-processing step in QC-AFQMC.
- [[Nearly Optimal Quantum Algorithm for Estimating Multiple Expectation Values (Huggins, Wan, McClean, Babbush et al 2022) — Paper Notes]] — Shares lead authors (Huggins, Wan). The gradient encoding approach achieves $\tilde{O}(\sqrt{M}/\varepsilon)$ queries for $M$ observables in the fault-tolerant regime; matchgate shadows operate in the NISQ regime with $O(\log M / \varepsilon^2)$ samples.
- [[Power of Data in Quantum Machine Learning (Huang, Babbush, McClean et al 2021) — Paper Notes]] — Uses the same classical shadow formalism (Huang, Kueng, Preskill 2020) for [[Projected Quantum Kernel via Local Observables|projected quantum kernels]].
- [[Quantum Simulation of Exact Electron Dynamics Can Be More Efficient Than Classical Mean-Field Methods (Babbush, Huggins, Berry et al 2023) — Paper Notes]] — Introduces [[First-Quantized Classical Shadows for k-RDM|first-quantized classical shadows]] for $k$-RDMs; the matchgate shadow formalism here is the second-quantized complement.
- [[Quantum Simulation of Electronic Structure with Linear Depth and Connectivity (Kivlichan, McClean et al 2018) — Paper Notes]] — The [[Fermionic Swap Network|fermionic swap network]] and [[Givens Rotation Slater Determinant Preparation|Givens rotation]] circuits are the building blocks for implementing matchgate circuits on linear-connectivity hardware.
- [[Efficient and Noise Resilient Measurements for Quantum Chemistry on Near-Term Quantum Computers (Huggins, McClean, Rubin, Babbush et al 2021) — Paper Notes]] — [[Basis Rotation Grouping for VQE Measurement|Basis rotation grouping]] is an alternative fermionic measurement strategy (deterministic, $O(N)$ circuits) vs. matchgate shadows (randomized, single protocol for multiple observable types).
- [[Compressing Many-Body Fermion Operators under Unitary Constraints (Rubin, Lee, Babbush 2021) — Paper Notes]] — The [[Double Factorization of Two-Electron Integrals|double factorization]] machinery shares the Givens rotation / Gaussian unitary structure with matchgate circuits.

### Trick cards in the vault

- [[Shadow Tomography for QMC Overlap Estimation]] — The Clifford shadow approach this paper supersedes for overlap estimation. Should be updated to reference matchgate shadows as the resolution of its exponential post-processing caveat.
- [[Virtual Correlation via Matchgate Contraction]] — The matchgate tensor structure of Slater determinants that enables both the virtual correlation trick and the efficient post-processing here.
- [[Noise-Resilient Overlap Ratios in QC-QMC]] — The noise cancellation in overlap ratios applies equally to matchgate shadows as to Clifford shadows.
- [[First-Quantized Classical Shadows for k-RDM]] — First-quantized analogue; matchgate shadows are the second-quantized version.
- [[Givens Rotation Slater Determinant Preparation]] — Givens rotations generate the matchgate circuits used in the protocol.
- [[Fermionic Swap Network]] — Linear-connectivity implementation of matchgate circuits.
- [[Matchgate 3-Design for Classical Shadows]] — New trick card from this paper.
- [[Pfaffian Shadow Estimation for Fermionic Observables]] — New trick card from this paper.
- [[Generating-Function Pfaffian Trick for Gaussian Overlaps]] — New trick card from this paper.
- [[Grassmann Integral Framework for Free-Fermion Traces]] — New trick card from this paper.
