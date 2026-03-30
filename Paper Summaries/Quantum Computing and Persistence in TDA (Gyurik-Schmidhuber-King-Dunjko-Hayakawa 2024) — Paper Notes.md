> **Source:** Casper Gyurik, Alexander Schmidhuber, Robbie King, Vedran Dunjko, Ryu Hayakawa, *Quantum computing and persistence in topological data analysis*, arXiv:2410.21258 (2024)
> **Links:** [arXiv](https://arxiv.org/abs/2410.21258)
> **Tags:** #topological-data-analysis #persistent-homology #BQP-completeness #quantum-advantage #harmonic-persistence #combinatorial-Laplacian #guided-sparse-Hamiltonian

---

## The computational problem

**$\delta$-Harmonic Persistence:** Given two simplicial complexes $K_1 \subseteq K_2$ (both clique complexes on $n$ vertices), an integer $p$, and a succinct classical description of a harmonic representative $|\sigma\rangle \in \mathcal{H}_p(K_1)$ (a representative of a hole), decide whether

$$\|\operatorname{proj}_{\mathcal{H}_p(K_2)}(|\sigma\rangle)\| \geq \delta \quad \text{or} \quad \|\operatorname{proj}_{\mathcal{H}_p(K_2)}(|\sigma\rangle)\| < \frac{1}{\exp(n)}$$

with promises: (1) the spectral gap of $\Delta_p^{K_2}$ is at least $1/\text{poly}(n)$, and (2) the overlap is either large ($\geq \delta$) or exponentially small.

Informally: given a hole in $K_1$ described by its harmonic representative, does a hole with significant harmonic overlap persist in $K_2$?

## What the paper does

Proves that $\delta$-Harmonic Persistence is **BQP$_1$-hard and contained in BQP**, for all $\delta \in [1/\text{poly}(n), 1 - 1/\text{poly}(n)]$. Here BQP$_1$ is BQP with perfect completeness (the YES circuit accepts with certainty). Under standard complexity assumptions (BQP$_1 \neq$ BPP), this gives an **exponential quantum speedup** for determining whether a topological hole persists across length scales.

My assessment: this is the strongest result in the TDA advantage story to date. Previous work established that *estimating Betti numbers* admits quantum speedup, but the more natural TDA question — *does a specific hole persist?* — had resisted complexity-theoretic characterisation. This paper settles it (conditionally). The key shift is from counting holes (a spectral estimation problem, susceptible to [[Analyzing Prospects for Quantum Advantage in Topological Data Analysis (Berry, Su, Babbush et al 2024) — Paper Notes|dequantization]]) to deciding overlap of a known harmonic with the harmonic space of a larger complex (a decision problem naturally encoding BQP computations).

## The algorithm / construction

### BQP containment (the algorithm)

The quantum algorithm is straightforward:

1. Prepare $|\sigma\rangle$ in the eigenvector register (possible because the subset-state description is efficiently preparable)
2. Apply [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|QPE]] on $U = e^{i\Delta_p^{K_2}}$ with precision $\frac{1}{2}\gamma(\Delta_p^{K_2})$
3. Measure the eigenvalue register
4. Repeat $O(\text{poly}(n))$ times
5. Output 1 if any measurement yields eigenvalue $\approx 0$; output 0 otherwise

This works because: if $\|\operatorname{proj}_{\mathcal{H}_p(K_2)}(|\sigma\rangle)\| \geq \delta \geq 1/\text{poly}(n)$, the probability of measuring a zero eigenvalue in each round is at least $\delta^2 \geq 1/\text{poly}(n)$, so $O(\text{poly}(n))$ repetitions detect it with high probability. If the overlap is exponentially small, we never see a zero eigenvalue. The spectral gap promise ensures QPE can distinguish zero from nonzero eigenvalues.

### BQP$_1$-hardness (the reduction)

This is where the real work is. Given a BQP$_1$ promise problem and a deciding circuit $U_x = U_T \cdots U_1$:

**Step 1: Circuit to Hamiltonian.** Use [[History-State Encoding with Unary Clock|Bravyi's construction]] to build a Hamiltonian $H_x = H_{\text{in}} + H_{\text{prop}} + H_{\text{clock}} + H_{\text{out}}$ on $N = m + 2(T+L)$ qubits (computation + clock), where $L = O(\text{poly}(n))$ identity gates are prepended ("pre-idling"). The gate set is restricted to $\{\text{CNOT}, U_{\text{Pyth}}, I\}$ where $U_{\text{Pyth}} = \frac{1}{5}\begin{pmatrix} 3 & 4 \\ -4 & 3 \end{pmatrix}$ is the Pythagorean gate (entries are rational with integer numerators — needed for the King-Kohler projector gadget construction).

The history state $|\psi_{\text{hist}}\rangle = \frac{1}{\sqrt{2(T+L)}} \sum_{t=1}^{T+L} (|C_t\rangle \otimes |\psi_{t-1}\rangle + |C_t'\rangle \otimes |\psi_t\rangle)$ satisfies:
- YES case ($x \in L_{\text{yes}}$): $|\psi_{\text{hist}}\rangle \in \ker H_x$
- NO case ($x \in L_{\text{no}}$): $\ker H_x = \{0\}$

**Step 2: Hamiltonian to simplicial complexes.** Using the [[Quantum Algorithms for Topological and Geometric Analysis of Data (Lloyd-Garnerone-Zanardi 2016) — Paper Notes|qubit graph]] and projector gadgets from King-Kohler (arXiv:2311.17234), construct:
- $K_1 = \text{Cl}(G_N)$ — the qubit graph (join of $N$ copies of the base 7-vertex graph), whose $(2N-1)$-th homology is isomorphic to $\mathbb{C}^{2^N}$
- $K_2 = \text{Cl}(\hat{G}_N)$ — obtained by adding gadget simplices that "fill" the holes corresponding to each projector $|\phi_i\rangle\langle\phi_i|$ in $H_x$

The gadget vertices have weight $\lambda = c\ell^{-1}g \ll 1$ (where $g$ is the spectral gap of $H_x$), keeping most weight on the qubit graph.

**Step 3: The guiding state.** Define the "pre-history" state:

$$|\psi_{\text{prehist}}\rangle = \frac{1}{\sqrt{2L}} \sum_{t=1}^{L} (|C_t, 0^m\rangle + |C_t', 0^m\rangle)$$

which encodes the first $L$ time steps (all identity gates acting on $|0^m\rangle$). Its image $|\sigma_{\text{prehist}}\rangle = s(|\psi_{\text{prehist}}\rangle)$ under the Hilbert space isomorphism $s$ is a [[Subset States for Succinct Description of High-Dimensional Chains|subset state]] with an efficient classical description — the key to making the input polynomial-sized.

**Step 4: Overlap analysis.**
- YES case: $|\sigma_{\text{prehist}}\rangle$ has overlap $\geq 1 - 1/\text{poly}(n)$ with $\mathcal{H}_{2N-1}(K_2)$, because:
  - $|\sigma_{\text{prehist}}\rangle$ overlaps $|\sigma_{\text{hist}}\rangle$ by $L/(L+T) = 1 - T/(L+T)$ (tunable by choosing $L$)
  - $|\sigma_{\text{hist}}\rangle$ is close to the harmonic space (it lives in $s(\ker H_x)$, an $O(g)$-perturbation of $\mathcal{H}_{2N-1}(K_2)$)
- NO case: $\mathcal{H}_{2N-1}(K_2) = \{0\}$ (no harmonic space at all), so the overlap is exactly 0

The spectral gap $\gamma(\Delta_{2N-1}^{K_2}) \geq 1/\text{poly}(n)$ follows from the King-Kohler spectral gap analysis combined with a new argument for the case when the harmonic space is nonempty (Lemma 7).

## Key results

**Theorem 1.** For all $\delta \in [1/\text{poly}(n), 1 - 1/\text{poly}(n)]$, $\delta$-Harmonic Persistence is BQP$_1$-hard and contained in BQP.

**Corollary 1.** The related problem $\delta$-Harmonics (deciding whether a chain has large overlap with the harmonic space of a *single* complex) is also BQP$_1$-hard and contained in BQP.

**Theorem 2.** Low-Energy Overlap (deciding overlap of a state with the low-energy subspace of a log-local Hamiltonian) is BQP-complete. This generalises Harmonic Persistence by dropping the restriction to combinatorial Laplacians.

**Theorem 3.** Kernel Overlap (overlap with the kernel of a log-local Hamiltonian, with spectral gap promise) is BQP$_1$-hard and contained in BQP.

## Comparison with prior work

| Paper | Problem | Complexity | Advantage? |
|---|---|---|---|
| [[Quantum Algorithms for Topological and Geometric Analysis of Data (Lloyd-Garnerone-Zanardi 2016) — Paper Notes\|LGZ (2016)]] | Normalised Betti numbers | QPE-based, not complexity-classified | Claimed exponential, later questioned |
| [[Towards Quantum Advantage via Topological Data Analysis (Gyurik-Cade-Dunjko 2022) — Paper Notes\|Gyurik-Cade-Dunjko (2022)]] | LLSD / normalised Betti | DQC1-hard (general complexes) | Evidence for advantage, not proof for clique complexes |
| [[Quantum Algorithm for Persistent Betti Numbers (Hayakawa 2022) — Paper Notes\|Hayakawa (2022)]] | Persistent Betti numbers | Efficient quantum algorithm | No hardness result |
| [[Complexity-Theoretic Limitations on Quantum Algorithms for TDA (Schmidhuber-Lloyd 2023) — Paper Notes\|Schmidhuber-Lloyd (2023)]] | Betti numbers (exact/approx) | #P-hard / NP-hard | Negative — even quantum can't solve |
| [[Analyzing Prospects for Quantum Advantage in Topological Data Analysis (Berry, Su, Babbush et al 2024) — Paper Notes\|Berry et al. (2024)]] | Normalised Betti numbers | Improved algorithm + partial dequantization | Advantage regime narrow |
| King-Kohler (2023) | Clique homology (decide $\beta_k > 0$) | QMA$_1$-hard | Too hard for BQP |
| **This paper** | **Harmonic Persistence** | **BQP$_1$-hard $\cap$ BQP** | **Yes — exponential, conditional on BQP$_1 \neq$ BPP** |

The shift from Betti number estimation to persistence decision is what makes the complexity land exactly in BQP. Counting holes is either too easy (normalised Betti numbers — possibly classically tractable in many regimes) or too hard (non-normalised — QMA$_1$-hard). Deciding whether a *specific* hole persists hits the sweet spot.

## Limits / caveats

1. **Weighted complexes only.** The proof requires weighted clique complexes (gadget vertices have weight $\lambda \ll 1$). The conjecture that the result extends to unweighted complexes is open. Weighted TDA is a growing area ([Kara 2020], [Baccini et al. 2022]) but not the default setting.

2. **False positives and false negatives relative to canonical persistence.** Harmonic Persistence is not identical to the standard definition of persistence:
   - *False positives:* A hole might not canonically persist but has support on exponentially small eigenvalues of $\Delta_p^{K_2}$ that QPE can't distinguish from zero
   - *False negatives:* A hole might canonically persist but get exponentially deformed, giving negligible overlap with the harmonic space of $K_2$
   The authors conjecture these don't arise in typical instances (e.g., random Vietoris-Rips complexes), but this is unproven.

3. **BQP$_1$ vs BQP.** The hardness is for BQP$_1$ (perfect completeness), not BQP. This is a minor technicality — BQP$_1$ is widely believed equal to BQP, but it's an open question.

4. **Subset state input.** The input harmonic must be a subset state with efficient classical description. Not all harmonics have this form — but the ones arising from the reduction do.

5. **Spectral gap promise.** The algorithm needs the spectral gap $\gamma(\Delta_p^{K_2}) \geq 1/\text{poly}(n)$, which is promised, not computed. Estimating spectral gaps is itself hard in general.

6. **Open: BQP-completeness for clique complexes.** Theorem 2 shows Low-Energy Overlap is BQP-complete for general Hamiltonians. Whether BQP-completeness (not just BQP$_1$-hardness) holds when restricted to combinatorial Laplacians of clique complexes remains open.

## Reusable ideas

1. **[[Subset States for Succinct Description of High-Dimensional Chains]]** — Encoding exponentially many high-dimensional simplices via a polynomial-sized product-of-edge-sets description, enabling efficient input specification for problems on exponentially large chain spaces.

2. **[[Pre-History State as Guided Sparse Hamiltonian Input]]** — Using the first $L$ (identity-gate) steps of a history state as a guiding state that (a) has large overlap with the full history state, (b) is a subset state with efficient description, and (c) can be tuned to achieve $1 - 1/\text{poly}(n)$ overlap by choosing $L$.

3. **[[Kernel Overlap as BQP-Complete Primitive]]** — The observation that deciding overlap of a state with the *kernel* of a sparse Hamiltonian (rather than estimating ground-state energy) is a natural BQP$_1$-complete problem, distinct from the guided sparse Hamiltonian problem.

## References within this paper

- [[Quantum Algorithms for Topological and Geometric Analysis of Data (Lloyd-Garnerone-Zanardi 2016) — Paper Notes|Lloyd, Garnerone, Zanardi (2016)]] — The original quantum TDA algorithm; this paper's starting point
- [[Towards Quantum Advantage via Topological Data Analysis (Gyurik-Cade-Dunjko 2022) — Paper Notes|Gyurik, Cade, Dunjko (2022)]] — DQC1-hardness for spectral density estimation; partial evidence for advantage
- [[Quantum Algorithm for Persistent Betti Numbers (Hayakawa 2022) — Paper Notes|Hayakawa (2022)]] — First quantum algorithm for persistent Betti numbers
- [[Complexity-Theoretic Limitations on Quantum Algorithms for TDA (Schmidhuber-Lloyd 2023) — Paper Notes|Schmidhuber, Lloyd (2023)]] — NP-hardness / #P-hardness for Betti number computation
- [[Analyzing Prospects for Quantum Advantage in Topological Data Analysis (Berry, Su, Babbush et al 2024) — Paper Notes|Berry, Su, Babbush et al. (2024)]] — Improved algorithms + dequantization; narrow advantage regime
- King, Kohler (2023, arXiv:2311.17234) — QMA$_1$-hardness of clique homology; provides the qubit graph and projector gadget constructions used here
- Crichigno, Kohler (2022, arXiv:2209.11793) — QMA$_1$-hardness of clique homology (earlier version); unweighted construction
- Cade, Folkertsma, Gharibian, Hayakawa, Le Gall, Morimae, Weggemans (ICALP 2023) — Improved hardness for [[Guided Sparse Hamiltonian Framework|guided local Hamiltonian problem]]; pre-idling technique used here
- Bravyi (2011) — Circuit-to-Hamiltonian construction (quantum 2-SAT analogue); the specific [[History-State Encoding with Unary Clock|history state encoding]] used
- Basu, Cox (FOCS 2022) — Harmonic persistent homology; defines the isomorphism $f_p^K$ between homology and harmonic homology and the commutative diagram for persistence
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|Gilyén, Su, Low, Wiebe (2019)]] — QSVT; noted as a way to relate the guided sparse Hamiltonian problem to Low-Energy Overlap via spectral projection

## Cross-links

### Paper notes
- [[Quantum Algorithms for Topological and Geometric Analysis of Data (Lloyd-Garnerone-Zanardi 2016) — Paper Notes]] — Original quantum TDA algorithm; this paper provides the complexity-theoretic justification
- [[Towards Quantum Advantage via Topological Data Analysis (Gyurik-Cade-Dunjko 2022) — Paper Notes]] — DQC1-hardness evidence; this paper's BQP$_1$-hardness is the definitive upgrade
- [[Quantum Algorithm for Persistent Betti Numbers (Hayakawa 2022) — Paper Notes]] — Algorithm for persistent Betti numbers; this paper addresses the complementary question of hardness for persistence
- [[Complexity-Theoretic Limitations on Quantum Algorithms for TDA (Schmidhuber-Lloyd 2023) — Paper Notes]] — Negative results for Betti number computation; contrasts with positive result here
- [[Analyzing Prospects for Quantum Advantage in Topological Data Analysis (Berry, Su, Babbush et al 2024) — Paper Notes]] — Improved Betti number algorithm + dequantization; this paper identifies a problem that resists dequantization
- [[Complexity and Algorithms for Euler Characteristic of Simplicial Complexes (Roune-Sáenz-de-Cabezón 2013) — Paper Notes]] — Classical #P-completeness of Euler characteristic; broader context for classical intractability of simplicial topology

### Trick cards
- [[Subset States for Succinct Description of High-Dimensional Chains]] — New trick from this paper
- [[Pre-History State as Guided Sparse Hamiltonian Input]] — New trick from this paper
- [[Kernel Overlap as BQP-Complete Primitive]] — New trick from this paper
- [[Simplex-to-Qubit Encoding for Simplicial Complexes]] — Encoding underlying the construction
- [[History-State Encoding with Unary Clock]] — Bravyi's history state construction used in the reduction
- [[Guided Sparse Hamiltonian Framework]] — Related framework; this paper's Kernel Overlap and Low-Energy Overlap are variants
- [[Persistent Laplacian for Tracking Topological Persistence]] — Related spectral approach to persistence (Hayakawa's trick)
- [[Kernel Dimension Estimation via QPE on Mixed States]] — The containment-in-BQP algorithm uses a variant of this
