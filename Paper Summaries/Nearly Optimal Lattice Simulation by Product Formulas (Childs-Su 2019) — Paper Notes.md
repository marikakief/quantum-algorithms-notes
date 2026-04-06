> **Source:** Andrew M. Childs and Yuan Su, *Nearly optimal lattice simulation by product formulas*, Phys. Rev. Lett. **123**, 050503 (2019); arXiv:1901.00564
> **Links:** [arXiv](https://arxiv.org/abs/1901.00564) · [PRL](https://doi.org/10.1103/PhysRevLett.123.050503)
> **Tags:** #hamiltonian-simulation #product-formulas #lattice #commutators #near-optimal #locality

---

## What the paper does

Proves that [[Product Formulas]] can simulate $n$-qubit nearest-neighbour lattice Hamiltonians with gate complexity $(nt)^{1+o(1)}$ — **nearly matching the $\tilde{\Omega}(nt)$ lower bound** of Haah-Hastings-Kothari-Low. This is the strongest rigorous justification for the practical effectiveness of Trotter methods on physically relevant systems.

The key insight: product-formula error for lattice Hamiltonians scales as $O(nt^{p+1})$ — **linear in $n$** — rather than the $O(n^{p+1} t^{p+1})$ scaling of generic (non-local) bounds. The improvement comes from analysing the *local error structure* of the product formula: nested commutators of lattice terms are mostly zero due to non-overlapping supports.

---

## The computational problem

**Input:** An $n$-qubit 1D lattice Hamiltonian $H = \sum_{j=1}^{n-1} H_{j,j+1}$ with $\max_j \|H_{j,j+1}\| \leq 1$, evolution time $t$, accuracy $\varepsilon$.

**Output:** A quantum circuit implementing $\tilde{U}$ with $\|\tilde{U} - e^{-iHt}\| \leq \varepsilon$.

---

## The algorithm

Group terms in an **even-odd pattern**:

$$H_{\text{odd}} = H_{1,2} + H_{3,4} + \cdots, \qquad H_{\text{even}} = H_{2,3} + H_{4,5} + \cdots$$

All terms within $H_{\text{odd}}$ commute (non-overlapping supports), and similarly for $H_{\text{even}}$. The first-order formula is:

$$S_1(t) = e^{-itH_{\text{even}}} e^{-itH_{\text{odd}}}$$

Higher orders use the standard Suzuki recursion:

$$S_{2k}(t) = S_{2k-2}(p_k t)^2\, S_{2k-2}((1-4p_k)t)\, S_{2k-2}(p_k t)^2$$

with $p_k = 1/(4 - 4^{1/(2k-1)})$.

---

## The local error analysis

### First-order case

Differentiate $S_1(t)$ and use the variation-of-constants formula:

$$S_1(t) - e^{-iHt} = \int_0^t d\tau_1 \int_0^{\tau_1} d\tau_2\; e^{-i(t-\tau_1)H} e^{-i\tau_1 H_{\text{even}}} e^{i\tau_2 H_{\text{even}}} [H_{\text{even}}, H_{\text{odd}}] e^{-i\tau_2 H_{\text{even}}} e^{-i\tau_1 H_{\text{odd}}}$$

The key estimate:

$$\|[H_{\text{even}}, H_{\text{odd}}]\| = \left\|\sum_{k=1}^{n/2} [H_{2k-2,2k-1} + H_{2k,2k+1},\; H_{2k-1,2k}]\right\| = O(n)$$

Each odd term $H_{2k-1,2k}$ commutes with all even terms **except** its two neighbours. So the commutator is a sum of $O(n)$ local terms rather than $O(n^2)$. Therefore:

$$\|S_1(t) - e^{-iHt}\| = O(nt^2)$$

### Higher-order case (canonical form)

The paper works with **canonical product formulas** $S(t) = S_s(t) \cdots S_1(t)$ where each stage $S_j(t) = e^{-itb_j B} e^{-ita_j A}$. The error admits the integral representation:

$$S(t) - e^{-iHt} = \int_0^t e^{-i(t-\tau)H} S(\tau) T(\tau)\, d\tau$$

where $T(\tau)$ encodes the local error. The paper derives a **simplified local error representation** (improving on Descombes-Thalhammer 2010):

$$T(\tau) = -i\sum_{k=1}^s \left[\prod_{l=k-1}^1 S_l^\dagger(\tau)\; (c_k A + d_{k-1} B)\; \prod_{l=1}^{k-1} S_l(\tau) - \prod_{l=k}^1 S_l^\dagger(\tau)\; (c_k A + d_{k-1} B)\; \prod_{l=1}^{k} S_l(\tau)\right]$$

with $c_k = \sum_{j=1}^k a_j$ and $d_k = \sum_{j=1}^k b_j$.

**Bounding $T(\tau)$:** Each term in $T$ involves conjugation by stage operators and commutation with $H_{\text{even}}$ or $H_{\text{odd}}$. The support of the operator grows by at most 2 sites with each conjugation (because only overlapping terms contribute). After $p$ commutator/conjugation layers, the support is at most $O(sk)$ — bounded independent of $n$.

This yields:

$$\|S_{2k}(t) - e^{-iHt}\| = O(n t^{2k+1})$$

---

## Key results

| Formula order | Error per step | Segments $r$ for accuracy $\varepsilon$ | Gate complexity |
|---|---|---|---|
| 1st order | $O(nt^2)$ | $O(nt^2/\varepsilon)$ | $O((nt)^2/\varepsilon)$ |
| $(2k)$-th order | $O(nt^{2k+1})$ | $O(t(nt/\varepsilon)^{1/(2k)})$ | $O((nt)^{1+1/(2k)}/\varepsilon^{1/(2k)})$ |
| Any $\delta > 0$ (choose $k = \lceil 1/(2\delta)\rceil$) | — | — | $O((nt)^{1+\delta})$ |

**Main result:** For any $\delta > 0$, the gate complexity is $(nt)^{1+\delta}$ with constant accuracy. Combined with the $\tilde{\Omega}(nt)$ lower bound of [[Quantum Algorithm for Simulating Real Time Evolution of Lattice Hamiltonians (Haah-Hastings-Kothari-Low 2021) — Paper Notes|HHKL]], this is **nearly optimal**.

---

## Comparison with prior work

| Method | Gate complexity | Locality required? | Ancilla-free? |
|---|---|---|---|
| Naïve Trotter (prior bounds) | $O(n(nt)^{1+1/(2k)}/\varepsilon^{1/(2k)})$ | No (works for any $H$) | Yes |
| Commutator bound ([[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes\|Childs et al. 2019]]) | Improved but still superlinear in $n$ | Partially | Yes |
| [[Faster Quantum Simulation by Randomization (Childs-Ostrander-Su 2019) — Paper Notes\|Randomized Trotter (Childs-Ostrander-Su 2019)]] | Better $L$-dependence but not linear in $n$ | No | Yes |
| [[Quantum Algorithm for Simulating Real Time Evolution of Lattice Hamiltonians (Haah-Hastings-Kothari-Low 2021) — Paper Notes\|HHKL (Lieb-Robinson)]] | $O(nT\,\text{polylog}(nT/\varepsilon))$ | Yes (lattice) | Yes (with PF blocks) |
| **This paper** | $(nt)^{1+o(1)}$ | Yes (lattice) | Yes |

Practically, Figure 1 in the paper shows that the 4th-order bound from this analysis matches empirical performance to within one order of magnitude — a dramatic improvement over prior rigorous bounds which were off by many orders.

---

## Ordering robustness

The first-order formula has the same $O(nt^2)$ error bound **regardless of term ordering** (not just even-odd). The argument: any permutation of lattice terms can be transformed to even-odd ordering by at most $2n$ swaps of adjacent exponentials, each costing $O(t^2)$ when supports overlap, giving total reordering error $O(nt^2)$.

Whether this ordering robustness extends to higher-order formulas is left open.

---

## Generalizations

The analysis extends to:

| Setting | Gate complexity |
|---|---|
| Periodic boundary conditions | $(nt)^{1+o(1)}$ (decompose into 3 commuting groups) |
| Constant-range interactions (range $\ell$) | $(nt)^{1+o(1)}$ (decompose into $\ell$ groups) |
| $D$-dimensional lattice | $((L^D t)^{1+o(1)})$ where $L$ is linear size |
| Time-dependent Hamiltonians | $(nt)^{1+o(1)}$ using time-dependent Suzuki formulas |

---

## Applications noted

- **Jordan-Lee-Preskill QFT simulation:** This paper provides rigorous justification for the $(nt)^{1+o(1)}$ claim in their lattice QFT simulation (Science 2012), which was previously stated without proof.
- **Tensor network representations:** The $(nt)^{1+o(1)}$ circuit depth implies a tensor network representation of lattice evolution with bond dimension $2^{n^{o(1)} t^{1+o(1)}}$.
- **Noisy simulation:** In the noisy case with gate error $\beta$, the optimal segment count is $r = (\alpha_{2k}/\beta)^{1/(2k+1)}$. This paper reduces $\alpha$ from $O((nt)^{2k+1})$ to $O(nt^{2k+1})$, improving performance even with imperfect gates.

---

## Limits / caveats

- **Lattice structure required.** The improved bounds depend on geometric locality — specifically, that commutators of non-overlapping terms vanish. For general sparse Hamiltonians without this structure, the old bounds still apply.
- **Asymptotic, not explicit.** The "nearly optimal" claim uses $k \to \infty$. For any fixed $k$, there's still a polynomial gap. The practical message is to use moderate $k$ (4th order seems optimal for current system sizes).
- **Constant prefactors grow with $k$.** The Suzuki recursion introduces factors like $5^{k-1}$ in the number of stages, so very high-order formulas have large constants even if the asymptotic exponent is small.
- **Comparison with HHKL.** The [[Lieb-Robinson Decomposition for Lattice Simulation]] approach of HHKL also achieves near-optimal complexity but with a much larger constant prefactor (Fig. 1 in supplementary: HHKL is ~50× more expensive than pure product formulas for $n \leq 300$). Product formulas win practically.

---

## Reusable ideas

1. [[Local Error Representation for Lattice Product Formulas]] — represent product-formula error as an integral where the integrand consists of commutators nested with unitary conjugations, with the number of terms independent of $n$ and $t$. The commutator structure then gives the correct locality-dependent bound.
2. [[Even-Odd Term Ordering for Lattice Simulation]] — grouping lattice terms into even and odd sets so each group consists of commuting terms, enabling efficient parallelisation and simplified error analysis.

---

## References within this paper

- [[Universal Quantum Simulators (Lloyd 1996) — Paper Notes|Lloyd (1996)]] — original product-formula simulation proposal
- [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes|Berry-Ahokas-Cleve-Sanders (2005)]] — higher-order Suzuki bounds (the "old" bounds this paper improves)
- Suzuki (1991) — the recursive integrator construction
- Descombes-Thalhammer (2010) — local error analysis framework; this paper simplifies their approach
- [[Quantum Algorithm for Simulating Real Time Evolution of Lattice Hamiltonians (Haah-Hastings-Kothari-Low 2021) — Paper Notes|Haah-Hastings-Kothari-Low (2018)]] — Lieb-Robinson-based near-optimal simulation and $\tilde{\Omega}(nt)$ lower bound
- Jordan-Lee-Preskill (2012) — lattice QFT simulation claiming $(nt)^{1+o(1)}$ without proof
- [[Faster Quantum Simulation by Randomization (Childs-Ostrander-Su 2019) — Paper Notes|Childs-Ostrander-Su (2019)]] — randomized product formulas (complementary improvement)
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes|Childs-Su-Tran-Wiebe-Zhu (2019)]] — commutator-based Trotter error theory (sister paper)
- [[Chemical Basis of Trotter-Suzuki Errors in Quantum Chemistry Simulation (Babbush-McClean-Wecker-Aspuru-Guzik-Wiebe 2015) — Paper Notes|Babbush et al. (2015)]] — empirical evidence that Trotter bounds are loose

---

## Cross-links

### Paper notes
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]] — sister paper developing the general commutator framework; this paper specialises it to lattice geometry for sharper bounds
- [[Faster Quantum Simulation by Randomization (Childs-Ostrander-Su 2019) — Paper Notes]] — complementary improvement via randomization; doesn't achieve linear-in-$n$ scaling
- [[Quantum Algorithm for Simulating Real Time Evolution of Lattice Hamiltonians (Haah-Hastings-Kothari-Low 2021) — Paper Notes]] — alternative near-optimal algorithm via Lieb-Robinson decomposition; larger constant prefactor
- [[Faster Digital Quantum Simulation by Symmetry Protection (Tran-Su-Childs-Wiebe 2021) — Paper Notes]] — uses symmetry to further reduce Trotter error (builds on this paper's framework)
- [[Universal Quantum Simulators (Lloyd 1996) — Paper Notes]] — the original product-formula proposal that this paper vindicates for lattice systems
- [[Nearly Tight Trotterization of Interacting Electrons (Su-Huang-Campbell 2021) — Paper Notes]] — extends this paper's local error analysis to fermionic (electronic structure) Hamiltonians
- [[Chemical Basis of Trotter-Suzuki Errors in Quantum Chemistry Simulation (Babbush-McClean-Wecker-Aspuru-Guzik-Wiebe 2015) — Paper Notes]] — empirical evidence that product formulas are better than generic bounds suggest
- [[Selection and Improvement of Product Formulae for Best Performance of Quantum Simulation (Morales-Costa-Pantaleoni-Burgarth-Sanders-Berry 2025) — Paper Notes]] — optimised higher-order product formulas; the local error analysis here applies to those formulas too

### Trick cards
- [[Local Error Representation for Lattice Product Formulas]]
- [[Even-Odd Term Ordering for Lattice Simulation]]
- [[Trotter Commutator-Scaling Bound]] — the general commutator framework from the sister paper
- [[Order-Condition Cancellation in Product Formulas]] — used in the higher-order analysis
- [[Product-Formula Time-Slicing for Local Hamiltonians]] — the basic idea this paper makes rigorous
- [[Lieb-Robinson Decomposition for Lattice Simulation]] — the competing approach that also achieves near-optimality
- [[Commutator-Sensitive Lieb-Robinson Bound]] — related locality exploitation
