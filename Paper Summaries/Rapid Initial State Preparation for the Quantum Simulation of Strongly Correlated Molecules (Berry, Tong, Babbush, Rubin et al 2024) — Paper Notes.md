> **Source:** Dominic W. Berry, Yu Tong, Tanuj Khattar, Alec White, Tae In Kim, Sergio Boixo, Lin Lin, Seunghoon Lee, Garnet Kin-Lic Chan, Ryan Babbush, Nicholas C. Rubin, *Rapid Initial State Preparation for the Quantum Simulation of Strongly Correlated Molecules*, arXiv:2409.11748, PRX Quantum **6**, 020327 (2025)
> **Links:** [arXiv](https://arxiv.org/abs/2409.11748) · [Journal](https://doi.org/10.1103/PRXQuantum.6.020327)
> **Tags:** #state-preparation #phase-estimation #quantum-chemistry #MPS #initial-state #FeMoco #fault-tolerant #QPE #window-functions #resource-estimation

---

## The computational problem

Estimate a ground-state energy $E_0$ of a strongly correlated molecular Hamiltonian to additive error $\epsilon$ and confidence $1-q$, when the prepared initial state has only imperfect squared overlap

$$p = |\langle \psi_0 | \phi_{\mathrm{init}} \rangle|^2.$$

The paper tackles the full end-to-end problem that many chemistry resource estimates duck:

1. prepare an initial state with large enough $p$ to make [[Iterative Phase Estimation (Kitaev)|phase estimation]] realistic,
2. filter measurement outcomes so that one gets a valid confidence interval for the **ground** energy rather than some low outlier from spectral leakage or an excited state, and
3. cost the whole thing for realistic Fe-S cluster instances, especially FeMoco.

The state family is MPS-like in the sense used in DMRG, with physical dimension $d$ and bond dimension $\chi$. The walk operator comes from [[Qubitization (Quantum Walk for Spectral Encoding)]], with block-encoding normalization $\lambda$.

## What the paper does

This paper closes a gap that has bothered the chemistry resource-estimation literature for years. Earlier FeMoco numbers often assumed near-perfect initial overlap. Here they actually cost how to get it. The arXiv preprint appeared in 2024 and the published version is PRX Quantum **6**, 020327 (2025).

There are two real contributions. First, they give a much cheaper way to compile the unitaries needed for MPS preparation, cutting the Toffoli count of that unitary-synthesis/MPS-preparation subroutine by about $7\times$ relative to the Low-Kliuchnikov-Schaeffer route used in earlier analyses. This is not a $7\times$ reduction of the full QPE workflow. Second, they treat ground-state identification as a confidence-interval problem, not a toy phase-estimation problem, and use window functions plus either repeated sampling or binary search with amplitude estimation to get realistic resource estimates.

My assessment: the MPS-preparation side matters more than the asymptotics of the filtering step. The $1/\sqrt{p}$ binary-search method is nice, but for the chemistry instances they study the main story is that the overlap can be made so high that plain sampling wins anyway. The paper is strongest where it is most concrete: FeMoco with modest bond-dimension MPS already looks good enough that the old “perfect overlap” assumption was not fantasy, just under-argued.

## The algorithm / construction

### 1. MPS preparation as partial-column unitary synthesis

They use the standard ancilla-register construction for preparing an MPS in left-canonical form. If the site tensors are written as matrices $A_j^{(n_j)}$, one embeds them into unitaries $G^{[j]}$ satisfying

$$G^{[j]}_{\alpha_j n_j,\alpha_{j-1} 0} = (A_j^{(n_j)})_{\alpha_{j-1},\alpha_j}.$$

The preparation circuit is a sequence of these unitaries acting on a physical qudit and a bond ancilla of dimension $\chi$. The key point is that for the middle tensors one does **not** need a full $d\chi \times d\chi$ unitary. Only $\chi$ columns matter, because one input leg is fixed to $|0\rangle$. So the problem is partial-column synthesis, not general unitary synthesis.

### 2. New general unitary synthesis primitive

For an $N_{\mathrm{un}}$-dimensional unitary, they start from the rectangular multiport-interferometer decomposition of Clements et al. rather than the triangular Reck-Zeilinger layout. Each layer is a block-diagonal array of $2 \times 2$ beam splitters, and each beam splitter is decomposed into

$$\text{phase} \cdot H \cdot \text{phase} \cdot H \cdot \text{phase}.$$

After commuting phases between layers, the unitary becomes alternating layers of:

- diagonal phase operators,
- Hadamards on the last qubit,
- increment/decrement operations to shift the odd/even pairing pattern.

The diagonal phases are loaded by [[QROM (Quantum Read-Only Memory)]], two phase layers at a time, so the synthesis cost is dominated by roughly $N_{\mathrm{un}}+1$ QROM calls rather than a huge generic synthesis overhead. Their explicit cost formulas are Eqs. (27) and (28), and numerically the method beats LKS by about a factor of $7$ over the parameter range they care about.

### 3. Half-column and $1/d$-column synthesis

For the MPS tensors they then exploit the fact that only a fraction of the columns are needed. If half the columns of a unitary are required, they write the relevant block as

$$
\begin{pmatrix}
A & ? \\
B & ?
\end{pmatrix}
=
\begin{pmatrix}
U_1 & 0 \\
0 & U_2
\end{pmatrix}
\begin{pmatrix}
D_1 & ? \\
D_2 & ?
\end{pmatrix}
\begin{pmatrix}
V & 0 \\
0 & V
\end{pmatrix},
$$

with $A = U_1 D_1 V$ and $B = U_2 D_2 V$. Operationally this becomes:

1. a unitary on the first $n-1$ qubits,
2. a controlled single-qubit rotation,
3. a qubit-controlled choice between two $(N_{\mathrm{un}}/2)$-dimensional unitaries.

Because adjacent MPS steps share these subspace unitaries, the initial unitary can be merged with the next step instead of paid for again. They then iterate the same idea for physical dimension $d > 2$, reducing synthesis of $\chi$ columns of a $d\chi \times d\chi$ unitary to $d-1$ repeated $d=2$ subproblems.

This is the cleanest reusable idea in the paper. It is not chemistry-specific.

### 4. Windowed phase estimation for confidence intervals

They treat QPE output as

$$\hat{\phi} = \phi + \Delta \phi,$$

with the error distribution determined by the control-state amplitudes. Instead of the usual flat window, they use [[Kaiser Window Amplitude Estimation|Kaiser-window]] or prolate-spheroidal windows to suppress the tails of the phase-error distribution.

For a target phase-error tail probability

$$\Pr(|\Delta \phi| \ge \epsilon_\phi \mid \phi) \le \delta,$$

the number of controlled walk-operator uses scales as

$$N = \frac{\ln(1/\delta)}{2\epsilon_\phi} + O\!\left(\frac{\ln\ln(1/\delta)}{\epsilon_\phi}\right),$$

for both Kaiser and prolate-spheroidal windows at leading order. The prolate window is asymptotically optimal, but the practical win over Kaiser is tiny, and once excited-state contamination is handled carefully the Kaiser window can even do slightly better in the relevant regime.

### 5. Sampling method

If the initial squared overlap is $p$, a simple strategy is to repeat QPE $n$ times and take the minimum estimated energy. The subtlety is that errors are asymmetric:

- being too **high** often just means one sampled an excited state,
- being too **low** can create a fake ground-state estimate.

Their basic error budget gives

$$[1-p(1-\delta/2)]^n + 1-(1-\delta/2)^n = q,$$

for final failure probability $q$, where $\delta$ is the per-shot tail probability outside the desired interval. Optimizing this yields asymptotic query complexity

$$
\frac{\lambda \ln(2/q)}{2p\epsilon}
\ln\!\left(\frac{\ln(2/q)}{pq}\right),
$$

which is Eq. (1) / Eq. (85) up to lower-order corrections.

They then sharpen the analysis by accounting for the location of excited states. This matters more than I expected. If an excited state lies near the ground state, it increases the chance of “good enough” estimates as well as the chance of bad low outliers, so a worst-case union bound is too pessimistic.

### 6. Binary search with amplitude estimation

To improve the scaling in $p$, they adapt the Lin-Tong style fuzzy-bisection approach. Here $p$ is the squared overlap; the corresponding amplitude overlap is $\sqrt{p}$. At each search step one distinguishes whether the ground energy lies in the lower or upper part of a shrinking interval. The decision problem is converted into amplitude estimation on the event that a windowed QPE outcome lands beyond a threshold $\bar\phi$.

With a shrinking factor optimized to $\omega = 1/\sqrt{2}$, the asymptotic walk-operator query complexity is

$$
7.77\,\frac{\lambda}{\sqrt{p}\,\epsilon}
\ln\!\left(\frac{4}{\sqrt{p}}\right)
\ln\!\left(\frac{\log_{\sqrt{2}}(\lambda/\epsilon)}{q}\right),
$$

which is Eq. (127). This is the advertised $1/\sqrt{p}$ improvement in squared-overlap notation, but the constant factor is large enough that it only beats sampling for about $p \lesssim 0.003$ in the paper's parameter choices.

### 7. Fe-S resource estimates and overlap extrapolation

For Fe$_2$S$_2$, Fe$_4$S$_4$, and FeMoco they use DMRG/MPS initial states. Since the exact overlap with the infinite-bond-dimension state is unavailable in the hard instances, they introduce an empirical overlap-extrapolation protocol based on two approximately linear relations:

$$
\log\!\bigl(1-|\langle \Phi(M')|\Phi(\infty)\rangle|^2\bigr)
\text{ vs. } (\log M')^2,
$$

and

$$
\log\!\bigl(|\langle \Phi(M')|\Phi(M'')\rangle|^2 - |\langle \Phi(M')|\Phi(\infty)\rangle|^2\bigr)
\text{ vs. } (\log M'')^2,
$$

for $M' \ll M''$.

This is empirical, not rigorous. But for the smaller Fe-S systems they validate it against cases where the high-bond-dimension result is accessible.

## Key results

### MPS preparation

- New unitary synthesis method: about **$7\times$ lower Toffoli cost** than prior LKS-based costing for the relevant synthesis subroutine and parameter regime.
- MPS preparation becomes a smaller part of the total cost than it was in previous estimates.
- For physical dimension $d=4$ the improvement over LKS is still around $3.5\times$.

### Windowed QPE

- Kaiser and prolate-spheroidal windows have the same leading-order cost for confidence intervals.
- The paper corrects an asymptotic expansion error in Slepian’s 1965 analysis.
- Once excited-state contamination is treated properly, Kaiser windows can be slightly better than prolate-spheroidal ones in the small-$p$ regime relevant to repeated sampling.

### Sampling vs binary search

For direct sampling,

$$
\text{queries} \approx
\frac{\lambda \ln(2/q)}{2p\epsilon}
\ln\!\left(\frac{\ln(2/q)}{pq}\right).
$$

For binary search,

$$
\text{queries} \approx
7.77\,\frac{\lambda}{\sqrt{p}\,\epsilon}
\ln\!\left(\frac{4}{\sqrt{p}}\right)
\ln\!\left(\frac{\log_{\sqrt{2}}(\lambda/\epsilon)}{q}\right).
$$

The binary-search method wins only for roughly

$$p \lesssim 0.003.$$

That crossover is one of the more practically useful outputs of the paper, but it is a model- and parameter-dependent numerical threshold rather than a universal constant.

### Extrapolated overlaps and MPS preparation costs

| System | Estimated squared overlap $p$ | Bond dimension | Spatial orbitals | MPS Toffolis | Logical qubits |
|---|---|---:|---:|---:|---:|
| Fe$_2^{\mathrm{III}}$Fe$_2^{\mathrm{II}}$ | 0.88 | 1000 | 36 | $4.22 \times 10^7$ | 359 |
| Fe$_4^{\mathrm{III}}$ | 0.92 | 1000 | 36 | $4.22 \times 10^7$ | 359 |
| FeMoco MPS1 | 0.99 | 6000 | 76 | $1.36 \times 10^9$ | 833 |
| FeMoco MPS2 | 0.95 | 4000 | 76 | $7.33 \times 10^8$ | 682 |
| FeMoco MPS3 | 0.98 | 4000 | 76 | $7.33 \times 10^8$ | 682 |

The paper sometimes switches between reporting overlap and squared overlap in prose. For FeMoco the abstract and summary discussion make clear that the relevant quantity for costing is **squared overlap**, and the practical message is that values around $0.9$ are plausible for moderate bond dimension.

### Total resource estimates

For confidence intervals of half-width $\epsilon = 10^{-3}$ Hartree:

| System | Method | $\lambda$ | Block-encoding Toffolis | Logical qubits | 95% total Toffolis | 99% total Toffolis |
|---|---|---:|---:|---:|---:|---:|
| Fe$_2^{\mathrm{III}}$Fe$_2^{\mathrm{II}}$ | THC | 168.7143 | 9,120 | 1,149 | $1.33 \times 10^{10}$ | $2.45 \times 10^{10}$ |
| Fe$_2^{\mathrm{III}}$Fe$_2^{\mathrm{II}}$ | DF | 154.7362 | 15,545 | 3,111 | $2.08 \times 10^{10}$ | $3.82 \times 10^{10}$ |
| Fe$_4^{\mathrm{III}}$ | THC | 164.1287 | 8,573 | 1,081 | $8.37 \times 10^{9}$ | $1.67 \times 10^{10}$ |
| Fe$_4^{\mathrm{III}}$ | DF | 150.2923 | 15,602 | 3,113 | $1.39 \times 10^{10}$ | $2.77 \times 10^{10}$ |
| FeMoco | THC | 781.8172 | 16,923 | 2,194 | $7.27 \times 10^{10}$ | $1.38 \times 10^{11}$ |
| FeMoco | DF | 582.4211 | 35,006 | 6,402 | $1.11 \times 10^{11}$ | $2.11 \times 10^{11}$ |

The headline number is FeMoco with THC and MPS2 overlap: about

$$7.3 \times 10^{10}$$

Toffolis at 95% confidence.

## Comparison with prior work

| Topic | Prior work | This paper | My read |
|---|---|---|---|
| Initial-state certification | [[Postponing the Orthogonality Catastrophe (Tubman, Mejuto-Zaera, Babbush et al 2018) — Paper Notes]] studies determinant and small multi-determinant overlaps | Uses DMRG/MPS states instead | Better fit for strongly correlated chemistry than product states, though it leans on heavy classical preprocessing |
| Quantum advantage framing | [[Is There Evidence for Exponential Quantum Advantage in Quantum Chemistry (Lee, Babbush, Chan et al 2022) — Paper Notes]] argues state preparation is the real bottleneck | Shows one important family where the bottleneck may be manageable | Not a rebuttal, but it does weaken the more pessimistic read for Fe-S chemistry |
| QPE window functions | [[Kaiser Window Amplitude Estimation]] optimizes constant factors for amplitude estimation | Extends the same style of window-function thinking to confidence-interval QPE and the binary-search routine | Solid constant-factor engineering, not a conceptual revolution |
| FeMoco fault-tolerant costing | [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes]] gives $3.2 \times 10^{10}$ Toffolis for Li-Hamiltonian FeMoco under idealized state-prep assumptions | Gives $7.27 \times 10^{10}$ Toffolis including MPS state prep and repeated QPE at 95% confidence | This is the honest version of the older headline number |
| Arbitrary-basis qubitization | [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes]] and [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes]] focus on walk-step cost | Keeps those block encodings, but stops pretending state preparation is free | Exactly the correction the literature needed |

## Limits / caveats

- The overlap extrapolation is empirical. The authors validate it on smaller Fe-S clusters, but there is no theorem saying it must remain trustworthy for FeMoco.
- The MPS overlap in FeMoco is with a **low-energy eigenstate candidate**, not automatically the true ground state. They explicitly cost the need to compare candidates.
- The binary-search method has prettier asymptotics in $p$, but for the chemistry instances they care about it mostly loses on constants.
- The unitary-synthesis method is impressive, but the paper does not prove an optimality statement. It is a strong engineering result, not the last word.
- The prolate-spheroidal window is formally optimal for tails, yet the practical advantage is marginal. So some of the asymptotic analysis is mathematically nice but operationally secondary.
- The classical preprocessing burden is substantial: DMRG calculations, candidate-state generation, and overlap extrapolation all happen before the quantum part starts. That does not kill the result, but it matters when someone tries to sell this as a pure quantum win.

## Reusable ideas

1. [[Partial-Column Unitary Synthesis for MPS Preparation]] — reduce MPS preparation to synthesizing only the columns that are actually accessed, then recurse through controlled subspace unitaries instead of compiling a full unitary every time.
2. [[Windowed QPE Sampling with Excited-State-Aware Error Budget]] — when QPE is repeated and one keeps the minimum energy estimate, the low-tail and high-tail errors should be budgeted separately because excited states mainly hurt in one direction.
3. [[MPS Overlap Extrapolation Protocol]] — estimate overlap with an effectively exact state from lower-bond-dimension overlaps using empirical linearity in $(\log M)^2$.
4. [[Kaiser Window Amplitude Estimation]] — the binary-search part of the paper leans directly on this style of amplitude estimation; existing card still covers the core primitive.

## References within this paper

| Reference | Why it matters here |
|---|---|
| [[Postponing the Orthogonality Catastrophe (Tubman, Mejuto-Zaera, Babbush et al 2018) — Paper Notes]] | Earlier serious treatment of imperfect initial overlap for chemistry QPE |
| Low, Kliuchnikov, Schaeffer (arXiv 2018; later publication date sometimes cited) | Prior unitary-synthesis method that this paper improves on by about $7\times$ in Toffoli count for the MPS-preparation subroutine |
| Schön, Solano, Verstraete, Cirac, Wolf (2005) | Standard ancilla-assisted MPS preparation framework being re-costed here |
| [[Kaiser Window Amplitude Estimation]] / Berry et al. (2024) | Source of the Kaiser-window perspective used for confidence-interval phase estimation |
| Lin and Tong (2020, 2022) | Binary-search / fuzzy-bisection perspective and related phase-estimation search methods |
| [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes]] | Baseline FeMoco THC resource estimate whose ideal-overlap assumption is being tested |
| Li et al. (2019) | FeMoco active-space Hamiltonian used for the main resource estimates |
| [[Expressing and Analyzing Quantum Algorithms with Qualtran (Harrigan, Khattar, Babbush, Rubin 2024) — Paper Notes]] | Used for the MPS Toffoli and qubit counts reported in Table I |
| Loaiza, Khah, Wiebe, Izmaylov (2023) | Source of the symmetry-shifting trick used to cut the effective $\lambda$ values |
| [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes]] | Earlier qubitization/QPE chemistry costing background |
| [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes]] | Prior arbitrary-basis chemistry costing background |

## Cross-links

### Paper notes
- [[Postponing the Orthogonality Catastrophe (Tubman, Mejuto-Zaera, Babbush et al 2018) — Paper Notes]]
- [[Is There Evidence for Exponential Quantum Advantage in Quantum Chemistry (Lee, Babbush, Chan et al 2022) — Paper Notes]]
- [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes]]
- [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes]]
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes]]
- [[Expressing and Analyzing Quantum Algorithms with Qualtran (Harrigan, Khattar, Babbush, Rubin 2024) — Paper Notes]]
- [[Reliably Assessing the Electronic Structure of Cytochrome P450 (Goings, Babbush, Rubin et al 2022) — Paper Notes]]
- [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes]]

### Trick cards
- [[Partial-Column Unitary Synthesis for MPS Preparation]]
- [[Windowed QPE Sampling with Excited-State-Aware Error Budget]]
- [[MPS Overlap Extrapolation Protocol]]
- [[Kaiser Window Amplitude Estimation]]
- [[QROM (Quantum Read-Only Memory)]]
- [[Unary Iteration]]
