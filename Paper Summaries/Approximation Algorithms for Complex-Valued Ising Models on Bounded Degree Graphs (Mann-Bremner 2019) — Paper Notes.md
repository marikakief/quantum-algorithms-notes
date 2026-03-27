> **Source:** Ryan L. Mann and Michael J. Bremner, *Approximation Algorithms for Complex-Valued Ising Models on Bounded Degree Graphs*, Quantum **3**, 162 (2019)
> **Links:** [arXiv:1806.11282](https://arxiv.org/abs/1806.11282) · [Journal](https://doi.org/10.22331/q-2019-07-11-162)
> **Tags:** #Ising-model #partition-function #FPTAS #complexity #quantum-supremacy #IQP #classical-simulation

---

## The computational problem

Approximate the Ising model partition function

$$Z_{\text{Ising}}(G; \Omega, \Upsilon) = \sum_{\sigma \in \{-1,+1\}^V} \exp\left(\sum_{\{u,v\} \in E} \omega_{\{u,v\}} \sigma_u \sigma_v + \sum_{v \in V} \upsilon_v \sigma_v\right)$$

with **complex-valued** interactions $\omega_e$ and external fields $\upsilon_v$ on graphs of bounded maximum degree $\Delta$, to multiplicative accuracy $\varepsilon$ in deterministic polynomial time.

This is directly connected to quantum computing: output amplitudes of IQP circuits are proportional to Ising partition functions with imaginary parameters.

---

## What the paper does

Establishes a deterministic polynomial-time approximation scheme (FPTAS) for the complex-valued Ising partition function on bounded-degree graphs when $|\omega_e|$ and $|\upsilon_v|$ are small enough — specifically when $|1 - e^{\pm \omega_e}| \leq \delta_{\Delta+1}$ and $|1 - e^{\pm \upsilon_v}| \leq \delta_{\Delta+1}$, where $\delta_\Delta = \Omega(1/\Delta)$ is an absolute constant.

The approach combines Barvinok's interpolation method (zero-free regions → truncated Taylor expansion of $\log Z$) with Patel-Regts efficient coefficient computation via induced subgraph counting. The paper also proves $Z_{\text{Ising}} \neq 0$ in this regime, which is of independent interest for understanding phase transitions via Lee-Yang zeros.

As a corollary, certain IQP circuit output amplitudes can be approximated in polynomial time when the gate angles are small.

---

## The algorithm / construction

### Step 1: Reduce to graph homomorphism partition function

The Ising partition function is reduced (approximation-preserving, polynomial-time) to a restricted multivariate graph homomorphism partition function $\mathrm{Hom}_M(G, S, k; \mathbf{A})$ — a sum over vertex colourings weighted by edge matrices.

### Step 2: Zero-freeness (Barvinok's framework)

Parameterise the edge matrices as $\mathbf{A}(z) = \{(1 + z(a^e_{ij} - 1))\}$ so that $\mathbf{A}(0) = \mathbf{J}$ (all-ones matrix, easy to evaluate) and $\mathbf{A}(1) = \mathbf{A}$ (target). By Barvinok's result, $\mathrm{Hom}_M(G, S, k; \mathbf{A}(z)) \neq 0$ for $|z| \leq \delta_\Delta / \delta$, so the polynomial has no zeros in a disc around the origin.

### Step 3: Efficient coefficient computation (Patel-Regts)

The leading coefficients of $P(G, S, k; z) = \mathrm{Hom}_M(G, S, k; \mathbf{A}(z))$ can be expressed as linear combinations of **connected induced subgraph counts** of size $O(\log(|V|/\varepsilon))$. On bounded-degree graphs, all such subgraphs can be enumerated in polynomial time (Borgs et al.).

### Step 4: Interpolation

With the first $m = O(\log(|V|/\varepsilon))$ inverse power sums of the roots computed, Barvinok's interpolation lemma gives a multiplicative $\varepsilon$-approximation to $P(G, S, k; 1) = Z_{\text{Ising}}$.

---

## Key results

**Corollary 6.** For fixed $\Delta$ and $0 < \delta < \delta_{\Delta+1}$, there is a deterministic FPTAS for $Z_{\text{Ising}}(G; \Omega, \Upsilon)$ on graphs of max degree $\leq \Delta$ when $(\Omega, \Upsilon)$ lies in the closed polyregion $\mathcal{R}_G(\delta)$.

**Corollary 7.** Under the same conditions, $Z_{\text{Ising}}(G; \Omega, \Upsilon) \neq 0$ — no phase transitions in this regime.

**Corollary 9.** There is a deterministic FPTAS for IQP circuit amplitudes $\psi_G = \langle 0^{|V|} | e^{-iH_{X_G}} | 0^{|V|} \rangle$ on bounded-degree graphs when $|\omega_e|, |\upsilon_v| \leq 2\arcsin(\delta/2)$.

**Relationship:** $\psi_G = 2^{-|V|} Z_{\text{Ising}}(G; i\Omega, i\Upsilon)$.

---

## Comparison with prior work

| Paper | Setting | Result |
|-------|---------|--------|
| Jerrum-Sinclair (1993) | Ferromagnetic, real fields | FPRAS (randomised) |
| Sinclair-Srivastava-Thurley (2012) | Anti-ferromagnetic, uniqueness region | Det. FPTAS |
| Sly-Sun (2012) | Anti-ferromagnetic, outside uniqueness | No FPRAS unless RP = NP |
| Liu-Sinclair-Srivastava (2017) | Ferromagnetic, non-imaginary complex fields | Det. FPTAS (via Lee-Yang) |
| **This paper** | Complex interactions + fields, $\|\cdot\| = O(1/\Delta)$ | Det. FPTAS (via Barvinok + Patel-Regts) |

The convergence radius $\delta_\Delta = \Omega(1/\Delta)$ is not tight (the paper notes the anti-ferromagnetic bounds of Sinclair et al. are sharper in the real case), but it's the first result for general complex parameters.

---

## Limits / caveats

1. **Small coupling only.** The FPTAS requires $|\omega_e|, |\upsilon_v| = O(1/\Delta)$ — essentially perturbatively close to the trivial model. For general coupling strengths, the problem is #P-hard.

2. **Bounds not sharp.** The paper explicitly notes the convergence radius is suboptimal compared to the real anti-ferromagnetic case. Sharpening the zero-free disc is an open problem.

3. **IQP implication is limited.** The classical simulation of IQP amplitudes only works for small gate angles, which corresponds to limited interference / short evolution time. Full IQP simulation would collapse the polynomial hierarchy.

4. **Bounded degree required.** The polynomial-time coefficient computation relies on bounded-degree subgraph enumeration. The approach doesn't extend to dense graphs.

---

## Reusable ideas

1. **[[Barvinok Interpolation for Partition Function Approximation]]** — Zero-freeness of a polynomial in a disc → truncated Taylor expansion of the log gives a multiplicative approximation. Combined with efficient coefficient computation, this yields FPTAS for combinatorial partition functions.

2. **[[IQP Amplitudes as Ising Partition Functions]]** — The output amplitude $\langle 0^n | e^{-iH} | 0^n \rangle$ for commuting Hamiltonians diagonal in the $X$ basis equals $2^{-n} Z_{\text{Ising}}(G; i\Omega, i\Upsilon)$. Classical simulability of the partition function in the small-coupling regime directly implies classical simulation of the corresponding quantum circuits.

---

## References within this paper

- **Barvinok (2016)** — monograph on combinatorics and partition function complexity; the interpolation framework
- **Patel and Regts (2017)** — efficient computation of graph polynomial coefficients via induced subgraph counting
- **Borgs, Chayes, Kahn, Lovász (2013)** — enumeration of connected induced subgraphs on bounded-degree graphs
- [[Classical Simulation of Commuting Quantum Computations Implies Collapse of the Polynomial Hierarchy (Bremner-Jozsa-Shepherd 2011) — Paper Notes|Bremner-Jozsa-Shepherd (2011)]] — IQP hardness; post-IQP = PP
- **Jerrum and Sinclair (1993)** — FPRAS for ferromagnetic Ising partition function
- **Sly and Sun (2012)** — hardness of anti-ferromagnetic Ising approximation outside the uniqueness region
- **Jaeger, Vertigan, Welsh (1990)** — exact computation of Tutte polynomial (and hence Ising partition function) is #P-hard

---

## Cross-links

### Paper notes
- [[Efficient Algorithms for Approximating Quantum Partition Functions (Mann-Helmuth 2021) — Paper Notes|Mann-Helmuth (2021)]] — same first author; quantum (not Ising) partition functions via cluster expansion
- [[Classical Simulation of Commuting Quantum Computations Implies Collapse of the Polynomial Hierarchy (Bremner-Jozsa-Shepherd 2011) — Paper Notes|Bremner-Jozsa-Shepherd (2011)]] — IQP hardness results that motivate understanding the easy/hard boundary
- [[Simulating Quantum Computations with Tutte Polynomials (Mann 2021) — Paper Notes|Mann (2021)]] — related connection between quantum circuits and graph polynomials
- [[On the Complexity of Random Quantum Computations and the Jones Polynomial (Mann-Bremner 2017) — Paper Notes|Mann-Bremner (2017)]] — companion paper on Jones polynomial and quantum circuit complexity

### Trick cards
- [[Barvinok Interpolation for Partition Function Approximation]]
- [[IQP Amplitudes as Ising Partition Functions]]
- [[Truncated Cluster Expansion as FPTAS]] — alternative approach (cluster expansion vs Barvinok interpolation) for similar problems
- [[Quantum Cluster Expansion for Partition Functions]] — quantum generalisation
