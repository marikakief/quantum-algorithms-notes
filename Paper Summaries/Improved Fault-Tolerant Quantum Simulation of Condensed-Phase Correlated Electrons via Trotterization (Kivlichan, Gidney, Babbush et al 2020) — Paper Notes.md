> **Source:** Ian D. Kivlichan, Craig Gidney, Dominic W. Berry, Nathan Wiebe, Jarrod McClean, Wei Sun, Zhang Jiang, Nicholas Rubin, Austin Fowler, Alán Aspuru-Guzik, Hartmut Neven, Ryan Babbush, *Improved Fault-Tolerant Quantum Simulation of Condensed-Phase Correlated Electrons via Trotterization*, arXiv:1902.10673, Quantum **4**, 296 (2020)
> **Links:** [arXiv](https://arxiv.org/abs/1902.10673) · [Journal](https://doi.org/10.22331/q-2020-07-16-296)
> **Tags:** #quantum-simulation #Trotter #fault-tolerant #Hubbard #electronic-structure #plane-wave #surface-code #phase-estimation #condensed-phase #Hamming-weight-phasing #resource-estimation

---

## The computational problem

Estimate ground state energies of condensed-phase correlated electron systems — the Hubbard model and the plane-wave electronic structure Hamiltonian (including jellium and periodic materials like lithium, diamond, silicon, graphite) — on a fault-tolerant quantum computer, using second-order Trotter-Suzuki [[product formula]]s combined with phase estimation.

The specific question: can optimized low-order Trotter methods compete with the [[Qubitization (Quantum Walk for Spectral Encoding)|qubitization]]-based approach of [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes|Babbush, Gidney et al. (2018)]] when targeting condensed-phase systems where *relative* (extensive) precision is physically meaningful?

## What the paper does

Shows that second-order Trotter formulas, when combined with [[Hamming Weight Phasing for Equiangular Rotations|Hamming weight phasing]] and careful error budgeting, can match or beat LCU-based methods for condensed-phase simulations — particularly when targeting extensive error $\Delta E = \Theta(N)$ (energy per unit cell rather than absolute energy). The headline results:

- **Hubbard model** with extensive error: $O(1)$ T complexity (the number of Trotter steps *decreases* with system size, until $N > 10^5$). With intensive error: $O(N^{3/2}/\Delta E^{3/2})$.
- **Jellium / plane-wave electronic structure** with extensive error: $O(N^2)$ T complexity. With intensive error: $O(N^{7/2}/\Delta E^{3/2})$.
- Classically intractable instances can be error-corrected with **a few hundred thousand physical qubits** at $p = 10^{-3}$ physical error rates, completing in under an hour.

This is a strong practical result. The qubitization methods of [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes|Babbush, Gidney et al. (2018)]] scale as $O(\lambda N / \Delta E)$ where $\lambda = O(N)$ for Hubbard and $\lambda = O(N^2)$ for plane-wave chemistry. For intensive error ($\Delta E = \Theta(1)$), qubitization wins. For extensive error ($\Delta E = \Theta(N)$), Trotter wins — and by a lot for the Hubbard model.

My take: this paper is the strongest argument that Trotter isn't dead for fault-tolerant simulation. The extensive-vs-intensive error framing is the real insight — it changes which method is optimal, and for condensed-matter problems extensive error is often the physically appropriate target. The resource estimates are pessimistic (upper bounds on Trotter error norm, not state-dependent), so the actual advantage could be even larger.

## The algorithm / construction

### Hamiltonians

Both target Hamiltonians take the form:
$$H = \sum_{pq} T_{pq} a^\dagger_p a_q + \sum_p U_p n_p + \sum_{p \neq q} V_{pq} n_p n_q$$

- **Plane-wave electronic structure** in the [[Plane-Wave Dual Basis|dual basis]] of [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes|Babbush et al. (2018)]]: kinetic energy diagonal in the plane-wave basis, potential energy diagonal in the dual basis. Coefficients from Eq. (3) of the paper. Specific materials differ only through the external potential $U_p$ (nuclear charges), which enters as a single layer of single-qubit rotations.

- **Hubbard model**: $T_{pq}$ nonzero only for nearest neighbours, $V_{pq}$ is on-site only ($U \cdot n_{i\uparrow} n_{i\downarrow}$).

### Two Trotter step circuits

Both use the second-order (symmetric) Trotter formula:
$$e^{-iHt} \approx \left(\prod_{\ell=1}^L e^{-iH_\ell t/2} \prod_{\ell=L}^1 e^{-iH_\ell t/2}\right)$$

**1. [[Fermionic Swap Network]] Trotter step** (from [[Quantum Simulation of Electronic Structure with Linear Depth and Connectivity (Kivlichan, McClean et al 2018) — Paper Notes|Kivlichan, McClean et al. (2018)]]): Uses odd-even transposition sort on a linear qubit chain. Each fermionic simulation gate simultaneously simulates hopping $T_{pq}$, density-density $V_{pq}$, and performs the fermionic swap — all in one nearest-neighbour two-qubit gate. The second-order step runs the network forward then backward. Arbitrary rotations per step: $4(N-1)^2$ (spinless jellium), $(N-1)(3N-4)$ (spinful jellium), $9N$ (Hubbard).

**2. Split-operator Trotter step** (from [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes|Babbush et al. (2018)]]): Separates kinetic and potential energy, alternating between momentum and position basis via the [[Fermionic Fast Fourier Transform (FFFT)]] (when side length is a power of 2) or Givens rotations (otherwise). In momentum basis: kinetic energy is single-qubit rotations. In position basis: potential energy is $ZZ$ interactions. The second-order step is:
$$e^{-iHt} \approx C \left[\prod_p e^{-iT_p Z_p t/2}\right] C^\dagger \left[\prod_p e^{-i\tilde{U}_p Z_p t} \prod_{pq} e^{-i\tilde{V}_{pq} Z_p Z_q t}\right] C \left[\prod_p e^{-iT_p Z_p t/2}\right] C^\dagger$$

### Hamming weight phasing

The core circuit optimization. When $n$ identical rotations $R_z(\theta)$ appear in parallel, compute their Hamming weight on ancilla qubits and apply $\lfloor \log_2 n + 1 \rfloor$ rotations to the Hamming weight register instead of $n$ individual rotations. Cost: $n - 1$ Toffoli gates (or $4n - 4$ T gates) and $n - 1$ ancilla qubits for the full version. See [[Hamming Weight Phasing for Equiangular Rotations]].

The paper extends this in two directions:
- **Limited ancilla**: with $r < n-1$ ancilla, combine $r+1$ rotations at a time in $\lceil n/(r+1) \rceil$ batches. Alternatively, a $\sqrt{n}$-grouping method uses only $\lfloor\sqrt{n}\rfloor + \lfloor\log_2 n\rfloor$ ancilla at $\sim 2\times$ the Toffoli cost.
- **Rotation catalysis**: for rotations $R_z(m\pi/2^k)$ (which appear in the FFFT for side lengths $\leq 16$), catalyze two $\sqrt{T}$ gates using only 5 T gates and a reusable seed state $\sqrt{T}|+\rangle$. See [[Rotation Catalysis via Hamming Weight Phasing]].

### Asymptotic improvement for split-operator potential

For the split-operator step with translation-invariant interactions ($\tilde{V}_{p,q} = \tilde{V}_{p+s,q+s}$), the paper orders the $ZZ$ terms by displacement vector $s$. All $N/2$ interactions in each layer have the same angle, so Hamming weight phasing applies to the full layer. This reduces the split-operator potential energy cost from $O(N^2 \log(1/\epsilon))$ arbitrary rotations to $O(N^2 + N\log N \log(1/\epsilon))$. See [[Translation-Invariant Hamming Weight Grouping]].

### Trotter error analysis

An improved second-order Trotter error bound (tighter by constant factors than Wecker et al. 2014):
$$\|e^{-iHt} - e^{-iH_\text{eff}t}\| \leq W t^3$$
where the Trotter error norm $W$ is:
$$W = \frac{1}{12} \sum_{b=1}^{L-1} \left(\left\|\sum_{c>b}\sum_{a>b} [[H_b, H_c], H_a]\right\| + \frac{1}{2}\left\|\sum_{c>b} [[H_b, H_c], H_b]\right\|\right)$$

This bound is non-perturbative (holds for all $t$, not just small $t$), and matches the $1/12$ factor from the BCH expansion. The energy eigenvalue shift from Trotterization satisfies $|E_n - E_n^\text{eff}| \leq W t^2 + O(W^3 t^8)$.

Numerics: the split-operator step consistently has $2$–$3\times$ smaller Trotter error norm than the fermionic swap network. The Trotter error norm scales as $O(N)$ for the Hubbard model and $O(N^3)$ for jellium.

### Phase estimation cost

Combining Trotter error, phase estimation precision, and rotation synthesis error:

$$\text{T-count} = (N_r N_\text{HT} + N_d) N_\text{PE}$$

where:
- $N_\text{PE} \approx 0.76\pi \sqrt{W}/(\Delta E_\text{PE} \sqrt{\Delta E_\text{TS}})$ — number of Trotter step applications
- $N_\text{HT} \approx 1.15 \log_2(N_r \sqrt{W}/(\Delta E_\text{HT}\sqrt{\Delta E_\text{TS}})) + 9.2$ — T gates per rotation from synthesis
- $N_r$ — arbitrary rotations per step
- $N_d$ — direct T/Toffoli gates per step

The total error budget is $\Delta E \geq \Delta E_\text{TS} + \Delta E_\text{PE} + \Delta E_\text{HT}$, numerically optimized for each system. Typically $\Delta E_\text{TS} > \Delta E_\text{PE} \gg \Delta E_\text{HT}$.

### The extensive-vs-intensive error distinction

The most important conceptual point. For condensed-phase systems, the physically meaningful error is often *per unit cell* (extensive: $\Delta E = \Theta(N)$), not absolute (intensive: $\Delta E = \Theta(1)$). This distinction dramatically favours Trotter:

- **Total T cost** scales as $\tilde{O}(N_r \sqrt{W}/\Delta E^{3/2})$.
- For Hubbard: $W = O(N)$, $N_r = O(N)$, so T cost $= O(N^{3/2}/\Delta E^{3/2})$.
  - Extensive error $\Delta E = \Theta(N)$: T cost $= O(N^{3/2}/N^{3/2}) = O(1)$.
  - Intensive error $\Delta E = \Theta(1)$: T cost $= O(N^{3/2})$.
- For jellium: $W = O(N^3)$, $N_r = O(N^2)$, so T cost $= O(N^{7/2}/\Delta E^{3/2})$.
  - Extensive error: T cost $= O(N^{7/2}/N^{3/2}) = O(N^2)$.
  - Intensive error: T cost $= O(N^{7/2})$.

Compare with qubitization from [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes|Babbush, Gidney et al. (2018)]]: $O(\lambda N/\Delta E)$ which is $O(N^2/\Delta E)$ for Hubbard and $O(N^3/\Delta E)$ for jellium. With extensive error, qubitization gives $O(N)$ for Hubbard (vs $O(1)$ for Trotter) and $O(N^2)$ for jellium (matching Trotter).

## Key results

**Theorem (Trotter error, improved):**
$$\|e^{-iHt} - e^{-iH_\text{eff}t}\| \leq Wt^3, \qquad |E_n - E_n^\text{eff}| \cdot t \leq \arctan\left(\frac{Wt^3}{\sqrt{4 - (Wt^3)^2} - (Wt^3)^2}\right)$$
for all eigenvalues $n$, valid whenever $Wt^3 \leq \sqrt{2}$.

**Resource estimates (extensive error, 0.5% of system energy):**

| System | Qubits | Toffoli | T gates | Physical qubits ($p=10^{-3}$) | Time |
|---|---|---|---|---|---|
| Hubbard 8×8 | 132 | $2.5 \times 10^5$ | $2.3 \times 10^7$ | $3.2 \times 10^5$ | 0.6 hr |
| Hubbard 16×16 | 518 | $4.3 \times 10^5$ | $2.5 \times 10^7$ | $9.4 \times 10^5$ | 0.7 hr |
| Jellium 3×3×3 | 58 | $4.4 \times 10^5$ | $3.1 \times 10^7$ | $1.9 \times 10^5$ | 0.8 hr |
| Jellium 5×5×5 | 254 | $5.0 \times 10^6$ | $4.0 \times 10^8$ | $6.0 \times 10^5$ | 11 hr |

**Resource estimates (intensive error, chemical accuracy / $\tau/100$):**

| System | Qubits | Toffoli | T gates | Physical qubits ($p=10^{-3}$) | Time |
|---|---|---|---|---|---|
| Hubbard 8×8 | 132 | $4.6 \times 10^7$ | $5.0 \times 10^9$ | $4.2 \times 10^5$ | 130 hr |
| Jellium 4×4×4 | 132 | $1.4 \times 10^8$ | $1.3 \times 10^{10}$ | $4.8 \times 10^5$ | 390 hr |

## Comparison with prior work

| Method | Hubbard T complexity (intensive) | Hubbard T complexity (extensive) | Jellium T complexity (intensive) | Jellium T complexity (extensive) |
|---|---|---|---|---|
| Qubitization ([[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes\|Babbush, Gidney et al. 2018]]) | $O(N^2)$ | $O(N)$ | $O(N^3)$ | $O(N^2)$ |
| **This paper (Trotter)** | $O(N^{3/2})$ | $O(1)$ | $O(N^{7/2})$ | $O(N^2)$ |
| Campbell (2019) randomized | $O(N^2)$ | $O(1)$ | $O(N^4)$ | $O(N^2)$ (claimed but not compiled) |
| Childs & Su (2019) high-order | $O(N^{3/2})$ | $O(1)$ (asymptotic, no concrete circuits) | — | — |

For the Hubbard model with extensive error: qubitization $O(N)$, this paper $O(1)$. That's the headline comparison. But qubitization dominates for intensive error (Hubbard and especially jellium).

## Limits / caveats

1. **Extensive error is not always appropriate.** For preparing superconducting ground states of the Hubbard model, the BCS gap is intensive — you'd need $\Delta E = \Theta(1)$. The paper argues this carefully and acknowledges both regimes have valid use cases.

2. **Trotter error bounds are pessimistic.** The error norm $W$ is an operator-norm bound, not state-dependent. Prior work shows these overestimate actual errors by orders of magnitude. The $O(1)$ Hubbard scaling persists until $N > 10^5$, but the bound looseness means real performance could be better *or* the crossover to qubitization could happen at different system sizes than predicted.

3. **$O(1)$ T complexity for Hubbard doesn't last forever.** At $N > 10^5$ orbitals, the number of phase estimation repetitions hits 1 and T count becomes linear in the number of Hamiltonian terms again. This limit is far beyond any system we'd simulate in the near or medium term.

4. **Split-operator step requires FFFT or Givens rotations.** The FFFT restricts to side lengths that are powers of 2; Givens rotations work generally but cost more arbitrary rotations. The fermionic swap network has no such restriction but consistently has worse Trotter error.

5. **Surface code estimates assume $10^{-3}$ or $10^{-4}$ error rates** and 1 µs surface code cycles with a single magic state factory. The decoder speed assumption (10 µs) is conservative — 100× slower than previously assumed.

## Reusable ideas

1. [[Hamming Weight Phasing for Equiangular Rotations]] — reduce $n$ identical parallel rotations to $\lfloor\log_2 n + 1\rfloor$ rotations on an ancilla Hamming weight register.
2. [[Translation-Invariant Hamming Weight Grouping]] — exploit translation invariance of Coulomb coefficients to group all interactions with the same displacement vector into a single Hamming-weight-phased layer.
3. [[Rotation Catalysis via Hamming Weight Phasing]] — catalyze $\sqrt{T}$ gates from a reusable seed state using only 5 T gates per pair, avoiding full rotation synthesis.
4. [[Extensive vs Intensive Error Targeting]] — choosing $\Delta E = \Theta(N)$ for condensed-phase systems dramatically reduces required Trotter steps and changes which simulation method is optimal.

## References within this paper

- [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes|Babbush, Wiebe, McClean et al. (2018)]]: introduced the plane-wave dual basis split-operator algorithm. This paper optimizes its Trotter step circuits.
- [[Quantum Simulation of Electronic Structure with Linear Depth and Connectivity (Kivlichan, McClean et al 2018) — Paper Notes|Kivlichan, McClean et al. (2018)]]: introduced the fermionic swap network. This paper uses it as one of two competing Trotter step circuits.
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes|Babbush, Gidney et al. (2018)]]: the qubitization approach this paper competes against for condensed-phase simulation. Scaling $O(\lambda N/\Delta E)$.
- Gidney, Quantum **2**, 74 (2018): original Hamming weight phasing paper. Extended here with limited-ancilla and rotation catalysis variants.
- Gidney & Fowler, Quantum **3**, 135 (2019): lattice surgery compilation and CCZ state distillation used for the surface code resource estimates.
- Childs & Su, PRL **123**, 050503 (2019): achieves similar asymptotic scaling for Hubbard using high-order formulas, but with $5^k$ constant-factor blowup for order $k$; no compiled circuits.
- Campbell, PRL **123**, 070503 (2019): randomized Trotter scaling $O(\lambda^2/\Delta E^2)$, matching this paper for extensive error. But randomization prevents Hamming weight phasing.
- Poulin, Hastings et al. (2015): prior commutator-based Trotter error bounds. This paper's bound is tighter by a factor of $1/12$.
- Wecker, Bauer et al. (2014): earlier Trotter error analysis. The directional control and error-operator-norm approach come from here.
- [[Postponing the Orthogonality Catastrophe (Tubman, Mejuto-Zaera, Babbush et al 2018) — Paper Notes|Tubman et al. (2018)]]: overlap analysis for initial state preparation, cited for the non-vanishing overlap assumption underlying QPE.
- [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes|Berry, Gidney, Motta, McClean, Babbush, Quantum **3**, 208 (2019)]]: sparse qubitization of electronic structure; the comparison baseline for T complexity.
- [[Quantum Simulation of Chemistry with Sublinear Scaling in Basis Size (Babbush, Berry, McClean, Neven 2019) — Paper Notes|Babbush, Berry, McClean, Neven (2019)]]: first-quantized interaction-picture approach with sublinear basis-size scaling.
- [[Low Rank Representations for Quantum Simulation of Electronic Structure (Motta, Babbush, Chan et al 2018) — Paper Notes|Motta, Babbush, Chan et al. (2018)]]: low-rank Trotter methods for molecular Hamiltonians (distinct from the plane-wave setting here).
- [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes|Su, Berry, Wiebe, Rubin, Babbush (2021)]]: subsequent first-quantized compilation that achieves better scaling than both approaches here, but for plane-wave chemistry specifically.

## Cross-links

### Paper notes
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]] — provides the general commutator-scaling framework for product-formula error; the $W$ factor derived here has the same nested-commutator structure, and Childs & Su (2019) independently achieve similar Hubbard scaling via high-order formulas
- [[Chemical Basis of Trotter-Suzuki Errors in Quantum Chemistry Simulation (Babbush-McClean-Wecker-Aspuru-Guzik-Wiebe 2015) — Paper Notes]] — earlier work showing operator-norm Trotter bounds are wildly loose for chemical Hamiltonians; this paper's tighter $1/12$-factor bound and extensive-error framing directly address the same looseness for condensed-phase systems
- [[Faster Digital Quantum Simulation by Symmetry Protection (Tran-Su-Childs-Wiebe 2021) — Paper Notes]] — complementary Trotter-error reduction strategy for periodic lattice systems (Hubbard, jellium) using symmetry kicks; orthogonal to the Hamming weight phasing approach developed here
- [[Higher Order Decompositions of Ordered Operator Exponentials (Wiebe-Berry-Høyer-Sanders 2010) — Paper Notes]] — the high-order product-formula framework underlying the Trotter steps analysed here
- [[Randomizing Multi-Product Formulas for Hamiltonian Simulation (Faehrmann-Steudtner-Kueng-Kieferová-Eisert 2022) — Paper Notes]] — multi-product randomized sampling; an alternative technique to reduce effective Trotter cost for the kind of chemistry Hamiltonians studied here
- [[Doubling the Order of Approximation via the Randomized Product Formula (Cho-Berry-Hsieh 2022) — Paper Notes]] — randomized order-doubling; could further reduce the Trotter step count for the Hubbard/jellium Hamiltonians analysed here
- [[Faster Algorithmic Quantum and Classical Simulations by Corrected Product Formulas (Bagherimehrab-Berry-Schleich-Aldossary-Angulo-Aspuru-Guzik 2024) — Paper Notes]] — deterministic corrected [[product formula]]s; extends the error-tightening direction of this paper with systematic corrector terms
- [[Selection and Improvement of Product Formulae for Best Performance of Quantum Simulation (Morales-Costa-Pantaleoni-Burgarth-Sanders-Berry 2025) — Paper Notes]] — optimized high-order [[product formula]]s; the resource counts derived here would benefit directly from the improved constant factors found there
- [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes]] — introduces the split-operator algorithm and plane-wave dual basis that this paper optimizes
- [[Quantum Simulation of Electronic Structure with Linear Depth and Connectivity (Kivlichan, McClean et al 2018) — Paper Notes]] — introduces the fermionic swap network Trotter step used here
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes]] — qubitization approach; the main comparison point
- [[Quantum Simulation of Chemistry with Sublinear Scaling in Basis Size (Babbush, Berry, McClean, Neven 2019) — Paper Notes]] — first-quantized interaction-picture alternative
- [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes]] — subsequent first-quantized compilation
- [[Low Rank Representations for Quantum Simulation of Electronic Structure (Motta, Babbush, Chan et al 2018) — Paper Notes]] — alternative Trotter-based approach (molecular orbital basis)
- [[Postponing the Orthogonality Catastrophe (Tubman, Mejuto-Zaera, Babbush et al 2018) — Paper Notes]] — initial state overlap analysis used as assumption
- [[Fault-Tolerant Quantum Simulation of Materials Using Bloch Orbitals (Rubin, Berry, Babbush et al 2023) — Paper Notes]] — qubitization-based alternative for periodic materials using Gaussian (Bloch orbital) basis instead of plane waves; DF qubitization at [2,2,2] for diamond: $6.7 \times 10^{10}$ Toffolis, compared to this paper's Trotter estimates on similar systems
- [[Analyzing Prospects for Quantum Advantage in Topological Data Analysis (Berry, Su, Babbush et al 2024) — Paper Notes|Berry, Su, Babbush et al 2024]]
- [[Expressing and Analyzing Quantum Algorithms with Qualtran (Harrigan, Khattar, Babbush, Rubin 2024) — Paper Notes|Harrigan, Khattar, Babbush, Rubin 2024]]
- [[Is There Evidence for Exponential Quantum Advantage in Quantum Chemistry (Lee, Babbush, Chan et al 2022) — Paper Notes|Lee, Babbush, Chan et al 2022]]
- [[Quantum Computation of Stopping Power for Inertial Fusion Target Design (Rubin, Berry, Babbush et al 2023) — Paper Notes|Rubin, Berry, Babbush et al 2023]]

### Trick cards
- [[Fermionic Swap Network]] — one of the two Trotter step circuits
- [[Fermionic Fast Fourier Transform (FFFT)]] — basis change in the split-operator step
- [[Plane-Wave Dual Basis]] — the basis underlying both Trotter steps
- [[Hamming Weight Phasing for Equiangular Rotations]] — key T-gate reduction technique (new card)
- [[Translation-Invariant Hamming Weight Grouping]] — potential energy optimization (new card)
- [[Rotation Catalysis via Hamming Weight Phasing]] — FFFT $\sqrt{T}$ optimization (new card)
- [[Extensive vs Intensive Error Targeting]] — error budget framing (new card)
- [[Trotterized Time Evolution for Chemistry]] — general Trotter framework
- [[Qubitization (Quantum Walk for Spectral Encoding)]] — the competing approach
