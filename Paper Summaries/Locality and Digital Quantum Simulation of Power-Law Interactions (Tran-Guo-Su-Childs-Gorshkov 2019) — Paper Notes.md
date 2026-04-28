> **Source:** Minh C. Tran, Andrew Y. Guo, Yuan Su, James R. Garrison, Zachary Eldredge, Michael Foss-Rasmussen, Andrew M. Childs, Alexey V. Gorshkov, "Locality and digital quantum simulation of power-law interactions," Physical Review X **9**, 031006 (2019). arXiv:1808.05225
> **Links:** [arXiv](https://arxiv.org/abs/1808.05225) · [PRX](https://doi.org/10.1103/PhysRevX.9.031006)
> **Tags:** #hamiltonian-simulation #trotter #product-formulas #power-law #Lieb-Robinson #long-range #locality #digital-simulation

---

## The computational problem

Simulate the time evolution $e^{-iHt}$ of a $D$-dimensional lattice Hamiltonian with power-law interactions:

$$H = \sum_{i < j} \frac{J_{ij}}{r_{ij}^\alpha} h_{ij}$$

where $r_{ij}$ is the distance between sites $i$ and $j$, $\alpha > 0$ controls how quickly interactions decay, and $h_{ij}$ are bounded ($\|h_{ij}\| \le 1$) two-body operators.

The key question: for which values of $\alpha$ is the system "local enough" that it can be efficiently simulated by [[Product Formulas|product formulas]] (Trotterisation)? How many gates does the simulation require as a function of $n$ (system size), $t$ (time), $\alpha$, and $\epsilon$ (error tolerance)?

Prior work could simulate such systems, but with gate counts that were far from optimal. This paper derives tight bounds by linking the simulation error directly to [[Lieb-Robinson bound|Lieb-Robinson bounds]] for power-law systems.

---

## What the paper does

Two intertwined contributions:

**1. Tight Lieb-Robinson bounds for power-law interactions.**
Proves a Lieb-Robinson bound for $D$-dimensional lattices with $\alpha > D$ of the form:
$$\|[A_X(t), B_Y]\| \leq C \|A\|\|B\| \min(|X|, |Y|) \, e^{v|t| - \mu \, r^{1-D/\alpha}}$$
for $\alpha > 2D$ (the "sufficiently local" regime), and a polynomial version for $D < \alpha \le 2D$. The bound encodes a slower-than-linear "light cone" for large $\alpha$ and a light-cone that can spread polynomially for small $\alpha$.

**2. Product-formula error bounds that reflect the Lieb-Robinson structure.**
Derives simulation error bounds for $p$th-order product formulas that scale with the *locality* structure of $H$, rather than simply with $\|H\|^{p+1}$. For power-law Hamiltonians, the key quantity is an $\alpha$-dependent commutator norm that captures how terms at distance $r$ interfere.

The combination gives improved (and in regimes near-optimal) gate counts for digital simulation of power-law systems, with explicit $\alpha$-dependence throughout.

---

## The algorithm / construction

### Product formula setup

The Hamiltonian splits as $H = \sum_\gamma H_\gamma$ where the $H_\gamma$ are geometrically organised groups of terms. The $p$th-order Lie-Trotter-Suzuki formula $\mathcal{S}_p(\delta t)$ satisfies:

$$\left\| e^{-iH\delta t} - \mathcal{S}_p(\delta t) \right\| = O\!\left(\alpha_{\mathrm{comm}}^{(p)} \cdot \delta t^{p+1}\right)$$

where $\alpha_{\mathrm{comm}}^{(p)}$ is a sum of nested commutator norms of the Hamiltonian terms. For power-law systems, these commutator norms depend on $\alpha$ through how strongly distant pairs contribute.

### Lieb-Robinson-based commutator bounds

The central technical insight: for a power-law Hamiltonian, the commutator $[H_\gamma, H_{\gamma'}]$ between terms at distance $r$ is bounded by

$$\|[H_\gamma, H_{\gamma'}]\| \leq C / r^\alpha$$

and nested commutators decay even faster. Summing over all pairs at all distances:

$$\alpha_{\mathrm{comm}}^{(1)} = \sum_{i < j} \frac{1}{r_{ij}^\alpha} \cdot \text{(local structure factors)}$$

For $\alpha > D$ in $D$ dimensions, this sum converges. For $\alpha \le D$, it diverges and the system is truly non-local — simulation cost reflects the full Hamiltonian norm.

### Interaction picture decomposition

For small $\alpha$ (long-range regime), the paper uses an interaction picture split. Decompose $H = H_{\mathrm{near}} + H_{\mathrm{far}}$ where $H_{\mathrm{near}}$ contains interactions within distance $\ell$ and $H_{\mathrm{far}}$ contains the rest. Simulate $H_{\mathrm{near}}$ by Trotterisation and $H_{\mathrm{far}}$ analytically or via LCU; tune $\ell$ to optimise gate count.

This is the same decomposition used in [[Destructive Error Interference in Product-Formula Lattice Simulation (Tran-Chu-Su-Childs-Gorshkov 2020) — Paper Notes|Tran-Chu-Su-Childs-Gorshkov (2020)]] for nearest-neighbour lattices, but extended to the power-law setting where the "near" vs. "far" cutoff $\ell$ depends on $\alpha$.

### Gate count from Lieb-Robinson

The total number of Trotter steps $r$ needed to achieve error $\epsilon$ over time $t$ is:

$$r = O\!\left(\left(\frac{\alpha_{\mathrm{comm}}^{(p)} \, t^{p+1}}{\epsilon}\right)^{1/p}\right)$$

For a $D$-dimensional lattice of $n$ sites, $\alpha_{\mathrm{comm}}^{(p)}$ scales as:
- $O(n)$ for $\alpha > 2D$ (short-range regime: only $O(1)$ terms per site contribute appreciably)
- $O(n^{1 + D/\alpha} )$ for $D < \alpha \le 2D$ (intermediate regime)
- $O(n^2)$ for $\alpha \le D$ (long-range regime: all pairs contribute)

Each Trotter step costs $O(n^2)$ gates in the worst case (for all-to-all interactions), but the number of steps decreases with larger $\alpha$.

---

## Key results

**Theorem (informal, Lieb-Robinson for power-law):** For $\alpha > 2D$, the commutator between spatially separated regions obeys an exponential light cone. For $D < \alpha \le 2D$, the light cone spreads as a power law $t^{D/(\alpha-D)}$. For $\alpha \le D$, no useful light cone exists.

**Theorem (informal, simulation cost):** For a 1D power-law system ($D=1$) of $n$ sites with $\alpha > 1$:

| $\alpha$ regime | Gate count (first-order PF, error $\epsilon$, time $t$) |
|---|---|
| $\alpha > 2$ | $\tilde{O}(n t^2 / \epsilon)$ |
| $1 < \alpha \le 2$ | $\tilde{O}(n^{1+1/\alpha} t^{2} / \epsilon)$ |
| $\alpha \le 1$ | $\tilde{O}(n^2 t^2 / \epsilon)$ |

(Factors of $n$ in the short-range regime match the best known bounds for nearest-neighbour lattices.)

**Corollary:** For $\alpha > 2D$, the simulation cost of power-law Hamiltonians matches — up to polylogarithmic factors — the cost of nearest-neighbour Hamiltonians of the same size. Long-range interactions with $\alpha > 2D$ do not add significant overhead.

**Lower bounds:** The paper also proves complementary lower bounds on the simulation error of product formulas, showing that for small $\alpha$ the overhead is unavoidable (the bounds are near-tight in the large-$n$ limit).

---

## Comparison with prior work

| Work | Approach | Gate count (1D, $\alpha > 2$, first-order PF) |
|---|---|---|
| Hastings-Koma (2006) | LR bounds for power-law; no simulation | — |
| Foss-Rasmussen et al. (2017) | Direct norm bounds | $O(n^2 t^2/\epsilon)$ (ignores locality) |
| Guo, Huang, Wei (2016) | Interaction-picture split | Improved but not tight in $\alpha$ |
| **This paper** | LR-based commutator bounds | $O(n t^2/\epsilon)$ for $\alpha > 2$ — matches NN case |
| [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes\|Childs-Su-Tran-Wiebe-Zhu (2019)]] | Nested commutator bound (general) | General theory; this paper specialises it to power-law |
| [[Destructive Error Interference in Product-Formula Lattice Simulation (Tran-Chu-Su-Childs-Gorshkov 2020) — Paper Notes\|Tran-Chu-Su-Childs-Gorshkov (2020)]] | Destructive interference for NN | $O(n^3/\epsilon)$ gate count; tighter error analysis for $\alpha = \infty$ |

The main advance over prior power-law simulation work is the use of the Lieb-Robinson structure to tighten the commutator sums. Prior work either used the global $1$-norm $\|H\|_1$ directly (giving $n^2$ overhead even for large $\alpha$) or relied on interaction-picture splits without an $\alpha$-adaptive analysis.

---

## Limits / caveats

- **Markovian (product formula) framework.** The bounds apply to Trotterised (digital) simulation. Non-product-formula methods (qubitization, LCU) can achieve better scaling in some parameter regimes; the paper does not compare against those.
- **$D < \alpha \le D$: genuinely hard regime.** The bounds for $\alpha \le D$ are $O(n^2)$, matching the trivial worst case. No simulation speedup from locality in this regime.
- **1D focus for tight bounds.** The paper derives the tightest results in 1D ($D=1$). In higher dimensions the bounds are less sharp because the geometry of the lattice matters more.
- **Isotropic power law assumed.** The coupling decays as $r^{-\alpha}$ with the same exponent in all directions. Anisotropic or structured long-range interactions may require separate analysis.
- **Gap not required.** The simulation bounds are for all initial states, not just ground states. For ground-state problems, additional structure (e.g., frustration-freeness) can give better results.
- **Constant factors.** As in most Trotter analyses, the constants in the gate count are not computed explicitly. Practical performance may differ from the asymptotic bounds.

---

## Reusable ideas

1. **[[Lieb-Robinson Decomposition for Lattice Simulation]]:** Use the Lieb-Robinson bound to decompose simulation error into contributions at each length scale. Terms at distance $r$ contribute $\sim r^{-\alpha}$ to the commutator; summing geometrically gives the $\alpha$-dependent scaling.

2. **[[Recursive Interval Decomposition for Power-Law Hamiltonians]]:** Partition the lattice into hierarchical intervals; handle each scale separately. The near/far cutoff $\ell$ is tuned to balance the Trotter error of the near part against the cost of handling the far part exactly. See [[Recursive Interval Decomposition for Power-Law Hamiltonians]] for the modern gate-cost version.

3. **[[Commutator-Sensitive Lieb-Robinson Bound]]:** The commutator $[A_X(t), B_Y]$ decays faster than $A(t)$ alone when the relevant operators nearly commute. This is the mechanism behind the tightened bounds for large $\alpha$ — terms far apart nearly commute, and their contribution to the Trotter error is suppressed.

4. **$\alpha$-adaptive interaction picture:** For intermediate $\alpha$, split the Hamiltonian at a distance threshold $\ell$. Simulate the near part digitally and handle the far part via a separate (cheaper) method. The optimal $\ell$ balances costs and gives a smooth interpolation between the nearest-neighbour and all-to-all regimes.

5. **Matching lower bounds from LR light cones:** The Lieb-Robinson light cone also provides lower bounds on simulation complexity — you cannot simulate correlations faster than the LR velocity, so any algorithm must spend time proportional to the LR propagation time. Proving lower bounds from LR is a reusable template.

---

## References within this paper

- Lieb & Robinson (1972) — original Lieb-Robinson bound for short-range lattices
- Hastings & Koma (2006) — Lieb-Robinson bounds for power-law systems
- Nachtergaele, Ogata & Sims (2006) — further LR generalisations
- Foss-Rasmussen, Zhu, Hause, Gorshkov (2017) — earlier power-law simulation bounds using global norms
- [[Lieb-Robinson Bounds and the Generation of Correlations and Topological Quantum Order (Bravyi-Hastings-Verstraete 2006) — Paper Notes|Bravyi, Hastings & Verstraete (2006)]] — LR consequences for information propagation and topological order
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes|Childs, Su, Tran, Wiebe & Zhu (2019)]] — general commutator-based Trotter error theory; this paper is a precursor / companion
- Childs, Maslov, Nam, Ross & Su (2018) — empirical Trotter scaling study whose numerical observations motivated much of this work
- [[Quantum Algorithm for Simulating Real Time Evolution of Lattice Hamiltonians (Haah-Hastings-Kothari-Low 2021) — Paper Notes|Haah, Hastings, Kothari & Low (2021)]] — optimal $O(nT)$ simulation for nearest-neighbour lattices using LR; later work that this paper anticipates
- Stoudenmire & White (2010) — DMRG context for power-law Hamiltonians (trapped-ion motivation)
- Eldredge, Gong, Young, Moosavian, Foss-Rasmussen & Gorshkov (2017) — fastest spreading in long-range systems; closely related LR analysis

---

## Cross-links

### Paper notes
- [[Lieb-Robinson Bounds and the Generation of Correlations and Topological Quantum Order (Bravyi-Hastings-Verstraete 2006) — Paper Notes]] — the LR framework that this paper applies to simulation; the core bound being exploited
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]] — the general commutator theory for which this paper provides the power-law specialisation
- [[Destructive Error Interference in Product-Formula Lattice Simulation (Tran-Chu-Su-Childs-Gorshkov 2020) — Paper Notes]] — follow-on work by overlapping author set; destructive interference further tightens these bounds for NN
- [[Faster Digital Quantum Simulation by Symmetry Protection (Tran-Su-Childs-Wiebe 2021) — Paper Notes]] — further Tran-Childs-Gorshkov collaboration on Trotter error reduction
- [[Quantum Algorithm for Simulating Real Time Evolution of Lattice Hamiltonians (Haah-Hastings-Kothari-Low 2021) — Paper Notes]] — achieves optimal $O(nT\,\text{polylog})$ for NN lattices; can be seen as the $\alpha \to \infty$ endpoint of this paper's bounds
- [[Hamiltonian Simulation with Nearly Optimal Dependence on All Parameters (Berry-Childs-Kothari 2015) — Paper Notes]] — general simulation; the black-box baseline against which power-law savings are measured
- [[An Area Law for One-Dimensional Quantum Systems (Hastings 2007) — Paper Notes]] — ground-state structure for local systems; LR bounds are the common foundation

### Trick cards
- [[Lieb-Robinson Decomposition for Lattice Simulation]]
- [[Commutator-Sensitive Lieb-Robinson Bound]]
- [[Recursive Interval Decomposition for Power-Law Hamiltonians]]
- [[Product-Formula Time-Slicing for Local Hamiltonians]]
- [[Trotter Commutator-Scaling Bound]]
