
> **Paper:** Quantum simulation with sum-of-squares spectral amplification
> **arXiv:** [2505.01528](https://arxiv.org/abs/2505.01528)
> **Authors:** Robbie King, Guang Hao Low, Ryan Babbush, Rolando D. Somma, Nicholas C. Rubin (Google Quantum AI / Caltech)
> **Date read:** 2026-03-12
> **Tags:** #hamiltonian-simulation #quantum-algorithms #SOS #spectral-amplification

---

## One-Line Take

Combine SOS decompositions (to tighten the lower bound on ground state energy) with spectral amplification (to exploit the nonlinearity of √x near zero) → query complexity drops from $\lambda/\varepsilon$ to $\sqrt{\Delta \lambda}/\varepsilon$ for low-energy problems.

---

## The Problem

Standard quantum simulation of $H = \sum_j g_j \sigma_j$ (LCU decomposition) has query complexity scaling with the $\ell_1$-norm $\lambda_{\text{LCU}} = \sum |g_j|$. This is tight in general but wasteful for **low-energy physics** — if we only care about states near the ground state, the relevant energy scale is much smaller than $\lambda$.

---

## The Two Ingredients

### Ingredient 1: Spectral Amplification (SA)

**Core idea:** If $H' \geq 0$, write $H' = H_{\text{SA}}^\dagger H_{\text{SA}}$ (a "square root" factorisation). Small eigenvalues $E$ of $H'$ become eigenvalues $\sqrt{E}$ of $H_{\text{SA}}$, and since $\sqrt{E} \gg E$ for small $E$, the low-energy sector is **amplified** relative to the rest of the spectrum.

**Concretely:** Given $H' = \sum_j B_j^\dagger B_j$, define the rectangular operator:

$$H_{\text{SA}} = \sum_{j} |j\rangle \otimes B_j = \begin{pmatrix} B_0 \\ B_1 \\ \vdots \\ B_{R-1} \end{pmatrix}$$

Then $H_{\text{SA}}^\dagger H_{\text{SA}} = H'$ and eigenphases of the associated quantum walk operator become $\arccos(E/\lambda)$. Near the spectral boundary ($E \approx 0$), arccos is steep — a coarse phase estimate translates to a fine energy estimate.

**Improvement:** Phase estimation cost goes from $O(\lambda/\varepsilon)$ to $O(\sqrt{E\lambda}/\varepsilon)$ for estimating eigenvalue $E$ of $H'$.

### Ingredient 2: Sum-of-Squares (SOS) Decomposition

**The problem with naive SA:** To make $H$ positive semidefinite, the simplest approach is termwise shifting: $H + \lambda_{\text{LCU}} \cdot \mathbb{1} = 2\sum_j |g_j| \Pi_j$. But then the gap parameter $\Delta_{\text{LCU}} = E_0 + \lambda_{\text{LCU}}$ is typically $\sim \lambda_{\text{LCU}}$ for frustrated systems. SA gives $\sqrt{\Delta \lambda} \approx \lambda$ — **no improvement**.

**The SOS fix:** Instead of termwise shifting, find a **tighter** PSD decomposition:

$$H + \beta \cdot \mathbb{1} = \sum_{j=0}^{R-1} B_j^\dagger B_j$$

where $-\beta$ is optimised to be as close as possible to the true ground state energy $E_0$. The $B_j$ are general operators (not just scaled projectors), built from an operator algebra of chosen degree.

---

## The SOS Trick in Detail

### Step 1: Choose an operator algebra

Pick a basis $\vec{X}^{(k)}$ of operators up to some degree $k$. Examples:
- **Pauli degree-$k$:** All Pauli monomials up to weight $k$ → $L = O(N^k)$ elements
- **Majorana degree-$k$:** Products of up to $k$ Majorana operators (natural for fermionic systems)

Each $B_j$ is a linear combination: $B_j = \vec{b}_j \cdot \vec{X}^{(k)}$.

### Step 2: Formulate as SDP

The decomposition is equivalent to finding a **Gram matrix** $G \succeq 0$ such that:

$$H + \beta \cdot \mathbb{1} = \vec{X}^{(k)\dagger} \, G \, \vec{X}^{(k)}$$

The objective is $\min \beta$ subject to:
- $G \succeq 0$
- The operator identity above holds coefficient-by-coefficient (expand both sides in the operator basis, match each coefficient → linear constraints on $G$)
- $\beta = \text{Tr}(G)$ (for traceless operator bases)

This is a standard **semidefinite program** of size $L \times L$, solvable in $\text{poly}(L)$ time.

### Step 3: Extract the $B_j$ operators

Eigendecompose the optimal $G = \sum_j \mu_j \vec{a}_j \vec{a}_j^\dagger$. Then:

$$B_j = \sqrt{\mu_j} \, \vec{a}_j^\dagger \vec{X}^{(k)}$$

The rank $R$ of $G$ gives the number of terms. Often $R \ll L$.

### Step 4: Build the block-encoding

Construct $H_{\text{SOSSA}} = \sum_j |j\rangle \otimes B_j$ with normalisation $\sqrt{\lambda_{\text{SOS}}}$ where:

$$\lambda_{\text{SOS}} = \sum_j \|\vec{b}_j\|_1^2$$

The final query complexity for phase estimation is $O(\sqrt{\Delta_{\text{SOS}} \cdot \lambda_{\text{SOS}}}/\varepsilon)$, where $\Delta_{\text{SOS}} = E_0 + \beta$.

---

## Why It Works: The Key Inequality

The improvement occurs when:

$$\Delta_{\text{SOS}} \cdot \lambda_{\text{SOS}} \;\ll\; \lambda_{\text{LCU}}^2$$

The SOS optimisation makes $\Delta_{\text{SOS}}$ small (tight lower bound). The price is that $\lambda_{\text{SOS}}$ may be larger than $\lambda_{\text{LCU}}$ (the $B_j$ operators are more complex than individual Pauli terms). **The product matters, not the individual factors.**

---

## The Trade-Off: Higher Degree Isn't Always Better

Increasing the SOS degree $k$ tightens $\beta$ (smaller $\Delta_{\text{SOS}}$) but makes the $B_j$ more complex (larger $\lambda_{\text{SOS}}$). The improvement can **saturate or reverse**:

| SOS degree | $\Delta_{\text{SOS}}$ | $\lambda_{\text{SOS}}$ | $\sqrt{\Delta \cdot \lambda}$ |
|------------|----------------------|----------------------|-------------------------------|
| Termwise (degree 1) | $O(\lambda_{\text{LCU}})$ | $\lambda_{\text{LCU}}$ | $\lambda_{\text{LCU}}$ (no improvement) |
| Degree 2 (Majorana) on SYK | $O(N)$ | $O(N^2)$ | $O(N^{3/2})$ ✅ |
| Degree 3 (Hastings-O'Donnell) on SYK | $O(\sqrt{N})$ | $O(N^{7/2})$ | $O(N^2)$ ❌ (back to LCU) |

**Lesson:** The optimal SOS degree depends on the system. For SYK, degree 2 hits the sweet spot.

---

## Application: SYK Model

$$H_{\text{SYK}} = \frac{1}{\sqrt{\binom{N}{4}}} \sum_{a<b<c<d} g_{abcd} \, \gamma_a \gamma_b \gamma_c \gamma_d$$

| Method | Query complexity | Speedup |
|--------|-----------------|---------|
| Standard LCU | $O(N^2/\varepsilon)$ | — |
| SOSSA (degree-2 Majorana) | $O(N^{3/2}/\varepsilon)$ | $\sqrt{N}$ |

The $B_j$ operators are degree-2 Majorana polynomials, block-encoded via **double factorisation** (diagonalise each $B_j$'s quadratic part using Gaussian unitaries). In the paper’s SYK setting, per-query gate costs are comparable enough that the query improvement carries to end-to-end asymptotics.

Caveat: this SYK scaling is a high-probability random-instance statement and the improvement is fundamentally a query-model result first; hardware-level constants depend on compilation choices.

---

## Algorithmic Details Worth Noting

### Summary of query complexities (Table 1 of the paper)

| Task | LCU | SOSSA |
|------|-----|-------|
| Energy estimation | $\Theta(\lambda/\varepsilon)$ | $\Theta(\sqrt{\Delta\lambda}/\varepsilon)$ (Thm 5) |
| Phase estimation | $O((\lambda/\sqrt{p}\,\varepsilon)\log(1/p))$ | $O((\sqrt{\Delta\lambda}/\sqrt{p}\,\varepsilon)\log(1/p))$ (Thm 7) |
| Time evolution | $\Theta(\lambda t + \log(1/\varepsilon))$ | $\Theta(\sqrt{\Delta\lambda}\,t + \sqrt{\lambda/\Delta}\log(1/\varepsilon))$ |

For phase estimation, $p = |\langle\psi|\psi_0\rangle|^2$ is the initial overlap with the ground state.

### Adaptive Energy Estimation (Theorem 5)

The energy estimation algorithm doesn't require knowing $\Delta$ in advance. Two-step binary search:
1. **Coarse:** Phase estimation at accuracy $\varepsilon' = \Theta(\sqrt{\varepsilon/\lambda})$ — either resolves $E$ directly (if near boundary) or gives a multiplicative estimate of $\lambda + E$.
2. **Fine:** Geometrically decreasing accuracy rounds with union-bounded failure probabilities.

Final cost: $\Theta(\sqrt{\max\{\varepsilon, \Delta_{\text{SOS}}\} \cdot \lambda}/\varepsilon)$ — interpolates smoothly between $\Theta(\lambda/\varepsilon)$ (worst case, $\Delta \sim \lambda$) and $\Theta(\sqrt{\lambda/\varepsilon})$ (near the spectral boundary, $\Delta \sim \varepsilon$).

### Gapped Phase Estimation

Decides whether an eigenphase lies in one of two intervals separated by a gap $\delta$. Uses $O((1/\delta)\log(1/q))$ queries with **no ancillary qubits** beyond those needed for the walk operator — implemented via a Chebyshev polynomial approximation to a step function applied through controlled $U$/$U^\dagger$ calls. Clean subroutine used as the inner binary-search step in the adaptive energy estimation algorithm.

---

## Connections and Potential Applications

- **Quantum chemistry:** Double factorisation is already standard there. SOSSA adds the spectral amplification layer — could improve ground state energy estimation for molecular Hamiltonians.
- **Condensed matter:** Any gapped system where the SOS bound $\beta$ is tight benefits.
- **Complexity theory:** The SOS hierarchy is dual to the pseudomoment relaxation. The SOSSA framework gives a *quantum algorithm* whose performance is controlled by the *classical* SOS relaxation quality — interesting interplay.
- **Our project:** Could SOSSA-style spectral amplification help with block-encoding constructions for computational geometry problems? The support function oracle for mixed volumes might benefit if we need to resolve small spectral gaps.

---

## Open Questions

1. **Which systems have $\Delta_{\text{SOS}} \cdot \lambda_{\text{SOS}} \ll \lambda_{\text{LCU}}^2$?** The paper characterises SYK; what about Hubbard, Heisenberg, molecular Hamiltonians?
2. **Can the SDP step be made quantum?** Currently classical preprocessing. For exponentially large algebras, the SDP itself becomes intractable.
3. **Is there a general relationship between SOS degree and $\Delta \cdot \lambda$?** Or is it always system-dependent?

---

## Notes and context

SOSSA was simultaneously applied to real-world quantum chemistry in a companion paper: [[Fast Quantum Simulation of Electronic Structure by Spectrum Amplification (Low, King, Berry, Babbush, Somma, Rubin 2025) — Paper Notes|Low, King, Berry et al. (2025)]], where it achieved state-of-the-art gate counts for molecular phase estimation (FeMoCo-76: $9.99 \times 10^8$ Toffolis). That paper introduces the [[DFTHC Factorization]] and [[Rectangular Block-Encoding for Spectrum Amplification|rectangular block-encoding]] technique. The present paper provides the general theory and the SYK demonstration.

The SYK speedup (degree-2 Majorana) is a random-instance / high-probability statement. The gate costs per block-encoding query are not dramatically different from LCU, so the query-model improvement carries through to end-to-end gate counts in this case — but this should be checked per system.

## References

- **Babbush, R., et al. (2019).** arXiv:1902.02134 — Double factorisation technique for quantum chemistry
- **Low, G.H., et al. (2025).** arXiv:2502.15882 — SOSSA applied to molecular phase estimation (chemistry companion)
- **Somma, R.D. (2019).** Quantum eigenvalue estimation via time series — spectral amplification origins

## References within this paper

- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes|Low & Chuang (2019)]] — qubitization used in the block-encoding
- [[LCU Origins (Childs-Wiebe 2012) — Paper Notes|Childs & Wiebe (2012)]] — [[Linear Combination of Unitaries (LCU)|LCU]]
- [[Near-Optimal Ground State Preparation (Lin-Tong 2020) — Paper Notes|Lin & Tong (2020)]] — ground state preparation (comparison point)
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|Gilyén et al. (2019)]] — [[QSVT Meta-Template|QSVT]]

---

## Cross-References

- [[Hamiltonian Simulation — Comparison Tables]]
- [[Linear Combination of Unitaries (LCU)]]
- [[LCU Origins (Childs-Wiebe 2012) — Paper Notes]]
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]]
- [[SOS Spectral Amplification]]
- [[Double Factorisation for Block-Encoding]]
- [[Gapped Phase Estimation]]
- [[Fast Quantum Simulation of Electronic Structure by Spectrum Amplification (Low, King, Berry, Babbush, Somma, Rubin 2025) — Paper Notes]] — chemistry companion paper
- [[DFTHC Factorization]]
- [[Rectangular Block-Encoding for Spectrum Amplification]]
- [[SOS Decomposition via Semidefinite Programming for Chemistry]]
