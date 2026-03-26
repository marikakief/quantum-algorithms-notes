> **Source:** Scott Aaronson, *Shadow Tomography of Quantum States*, STOC 2018, arXiv:1711.01053
> **Links:** [arXiv](https://arxiv.org/abs/1711.01053) · [STOC 2018](https://dl.acm.org/doi/10.1145/3188745.3188802)
> **Tags:** #shadow-tomography #tomography #quantum-learning #sample-complexity #online-learning

---

## The computational problem

**Shadow tomography:** Given $k$ copies of an unknown $D$-dimensional mixed state $\rho$, and $M$ known two-outcome measurements $E_1, \ldots, E_M$ (each $0 \le E_i \le I$), estimate $\operatorname{Tr}(E_i \rho)$ to additive error $\varepsilon$ for *all* $i$ simultaneously, with success probability $\ge 1 - \delta$.

Goal: minimise $k$ as a function of $M$, $D$, $\varepsilon$, $\delta$.

Two baseline strategies:
- **Full tomography:** $\Theta(D^2/\varepsilon^2)$ copies — independent of $M$ but scales with dimension.
- **Naive per-measurement:** $\Theta(M/\varepsilon^2)$ copies — works for any $M$ but linear in the number of measurements.

Shadow tomography asks whether, when $M$ and $D$ are both large (exponential in $n$), we can do far better than either.

---

## What the paper does

Aaronson gives an explicit procedure using only $\tilde{O}(\varepsilon^{-4} \cdot \log^4 M \cdot \log D \cdot \log(1/\delta))$ copies — polylogarithmic in *both* $M$ and $D$. This is exponentially better than full tomography when $M$ is large, and exponentially better than naive per-measurement estimation when $D$ is large. The result is information-theoretic: the procedure may require exponential classical computation, but the sample complexity is what matters here.

The key insight is that you don't need to learn $\rho$ — you just need a hypothesis that approximately agrees with $\rho$ on the given measurements. A postselected-learning approach (combined with a gentle search primitive to find measurements where the hypothesis errs) achieves this.

---

## The algorithm

The algorithm combines two ideas: a multiplicative-weights hypothesis update (adapted from postselected quantum learning) and a [[Gentle Search via Quantum OR|gentle search]] that locates a failing measurement using few copies.

### Setup

Fix amplification parameter $q = \Theta(\varepsilon^{-2} \cdot \log\log D)$. Work with $\rho^{\otimes q}$, the $q$-fold tensor product. The algorithm maintains a classical description of a hypothesis state $\rho_t$ (initialised to $I/D$, the maximally mixed state).

### Main loop

At each iteration $t$:

1. **Amplified tests.** For each $i$ and current hypothesis $\rho_t$, define two $q$-register tests:
   - $E^*_{i,+}$: apply $E_i$ to each register; accept if count $\ge (\operatorname{Tr}(E_i \rho_t) + 3\varepsilon/4) \cdot q$
   - $E^*_{i,-}$: accept if count $\le (\operatorname{Tr}(E_i \rho_t) - 3\varepsilon/4) \cdot q$

   If $\operatorname{Tr}(E_i \rho) \ge \operatorname{Tr}(E_i \rho_t) + \varepsilon$ then $E^*_{i,+}$ accepts $\rho^{\otimes q}$ with probability $\ge 5/6$.

2. **[[Gentle Search via Quantum OR|Gentle search]].** Search among $i \in [M]$ for an index where either amplified test accepts $\rho^{\otimes q}$ with probability $\ge 2/3$, using $O(\log^4 M / \varepsilon^2)$ copies per invocation (via binary search + [[Quantum OR Subroutine|Quantum-OR]]).

3. **Termination.** If no such $i$ exists, output $b_i = \operatorname{Tr}(E_i \rho_t)$ for all $i$ — the current hypothesis satisfies $|b_i - \operatorname{Tr}(E_i \rho)| \le \varepsilon$ for all $i$.

4. **[[Postselected Hypothesis Update (Multiplicative Weights)|Hypothesis update]].** If index $j$ is found, update $\rho_t \to \rho_{t+1}$ by postselecting on the $q$-register filter consistent with the observed failure direction. This is a multiplicative-weights step: it increases weight on components consistent with observed acceptance behaviour.

### Resource accounting

- Each update step increases $\log(1/\operatorname{tr}(\rho_t^2))$ by a constant (relative entropy argument), so the number of iterations is bounded by $T = O(q \log D / \varepsilon) = O(\varepsilon^{-3} \log D \cdot \operatorname{polylog})$.
- Each iteration uses $O(\log^4 M / \varepsilon^2)$ copies via the gentle search.
- Total: $k = \tilde{O}(\varepsilon^{-5} \cdot \log^4 M \cdot \log D)$ in the original paper; subsequent analysis (Brandão et al. and improved online learning subroutines) brings this to $\tilde{O}(\varepsilon^{-4} \cdot \log^4 M \cdot \log D)$.

---

## Key results

**Main theorem:**
$$k = \tilde{O}\!\left(\frac{\log^4 M \cdot \log D}{\varepsilon^4} \cdot \log\frac{1}{\delta}\right)$$
copies of $\rho$ suffice to solve shadow tomography.

**Lower bound:** A classical reduction (diagonal states, computational basis measurements) gives $\Omega(\min\{D^2, \log M\}/\varepsilon^2)$, confirming that dependence on at least one of $D$ or $\log M$ is necessary.

**Corollary (circuits):** For any $n$-qubit state $\rho$, the acceptance probabilities of all polynomial-size quantum circuits on $\rho$ can be estimated to $\pm\varepsilon$ using only $n^{O(1)}$ copies (since $M = 2^{O(n \log n)}$, so $\log^4 M = n^{O(1)}$, and $\log D = n$).

---

## Key lemmas and techniques

1. **[[Gentle Measurement Lemma]]** (Winter): if $E$ accepts $\rho$ with probability $\ge 1 - \varepsilon$, then after measurement the post-measurement state has trace distance $O(\sqrt{\varepsilon})$ from $\rho$. Used to ensure the hypothesis update doesn't destroy the remaining copies of $\rho$.

2. **Quantum union bound** (Ambainis et al., Wilde): sequential gentle measurements preserve the state cumulatively. Underpins the stability of the multi-iteration procedure.

3. **[[Quantum OR Subroutine]]** (Harrow–Lin–Montanaro fix of Aaronson 2006): distinguishes "some $E_i$ accepts $\rho$ with prob $\ge c$" from "all accept with prob $\le c - \varepsilon$" using $O(\log M / \varepsilon^2)$ copies. This is the core primitive for the gentle search.

4. **Binary search over measurements:** reduces search to $O(\log M)$ calls to Quantum-OR, each with a slightly degraded gap. Total search cost: $O(\log^4 M / \varepsilon^2)$.

5. **Postselected learning / multiplicative weights for quantum states** (Aaronson 2004): the hypothesis convergence argument. The number of updates before the hypothesis satisfies all constraints is $O(\log D)$ (in the postselected setting); this paper makes it constructive by finding the failing measurement efficiently.

---

## Comparison with prior work

| Method | Sample complexity | Notes |
|---|---|---|
| Full tomography | $\Theta(D^2/\varepsilon^2)$ | Learns $\rho$ fully |
| Per-measurement | $\Theta(M/\varepsilon^2)$ | Independent measurements |
| **Shadow tomography** | $\tilde{O}(\log^4 M \cdot \log D / \varepsilon^4)$ | Joint measurements; poly-classical computation |
| Brandão et al. 2019 | Same sample complexity; $\operatorname{poly}(M, D)$ runtime | Efficient; uses Gibbs state methods |
| [[Classical shadows (Huang-Kueng-Preskill 2020)]] | $O(\log M / \varepsilon^2)$ for local obs. | Independent measurements; restricts to local/few-body observables |

Classical shadows (Huang–Kueng–Preskill) later achieved better sample complexity for *local* observables without needing joint measurements, but the shadow tomography bound applies to *arbitrary* measurements.

---

## Relation to the shadow frameworks

There are three distinct things called "shadow" in quantum computing:

| Framework | What it compresses | Joint measurements? |
|---|---|---|
| **Shadow tomography** (this paper) | Copies of $\rho$ needed to estimate many observables | Yes |
| [[Classical shadows (Huang-Kueng-Preskill 2020)]] | Same, for local observables | No (randomised single-copy) |
| [[Shadow Hamiltonian Simulation — Paper Notes\|Shadow Hamiltonian simulation]] | Dimension of the simulated state | Yes (full quantum computation) |

The shadow Hamiltonian simulation paper explicitly cites this work (in its related-work table) as occupying a different niche — it compresses the dynamics, not the tomographic sample complexity.

---

## Limits / caveats

- **Computationally expensive.** The procedure is information-theoretically optimal but the classical computation (maintaining and updating the hypothesis) may be exponential. Brandão et al. fix this at the cost of more sophisticated techniques.
- **Joint measurements required.** The algorithm performs entangled measurements across many copies of $\rho$ — not implementable with single-copy measurements. Classical shadows circumvent this for restricted observable classes.
- **Gaussian lower bound.** There are constructions (diagonal states + computational basis measurements) showing $\Omega(\log M / \varepsilon^2)$ copies are necessary, so the $\log M$ dependence is tight.
- **ε-dependence.** The $\varepsilon^{-4}$ (or $\varepsilon^{-5}$ in the original) is worse than the $\varepsilon^{-2}$ of single-observable estimation; the price of simultaneously handling exponentially many observables.

---

## Reusable ideas

1. **[[Gentle Search via Quantum OR]]** — using Quantum-OR + binary search to locate a "violating" measurement among exponentially many, using polylog copies, without destroying the state.
2. **[[Quantum OR Subroutine]]** — distinguishing "some measurement accepts ρ" from "none does" using $O(\log M / \varepsilon^2)$ copies; the fixed Harrow–Lin–Montanaro version.
3. **[[Postselected Hypothesis Update (Multiplicative Weights)]]** — running multiplicative weights in hypothesis space, converging in $O(\log D)$ steps by finding and postselecting on disagreeing measurements.

---

## References within this paper

- Aaronson (2004) — "Limitations of quantum advice and one-way communication"; postselected learning theorem; $O(\log D)$ iteration bound
- Harrow, Lin & Montanaro (2016/2017) — fix of the Quantum OR argument; central to the gentle search primitive
- Winter (1999) — [[Gentle Measurement Lemma]]
- O'Donnell & Wright (2016), Haah et al. (2017) — full tomography lower and upper bounds: $\Theta(D^2/\varepsilon^2)$
- Brandão, Harrow, Oppenheim & Strelchuk (2015) / Brandão et al. (2017–2019) — follow-up work on computationally efficient versions
- [[Classical shadows (Huang-Kueng-Preskill 2020)]] — later work circumventing joint measurements for local observables

---

## Cross-links

### Paper notes
- [[The Learnability of Quantum States (Aaronson 2006) — Paper Notes]] — the offline precursor: same goal, $O(n)$ sample complexity, no gentleness requirement
- [[Shadow Hamiltonian Simulation — Paper Notes]]
- [[Triply Efficient Shadow Tomography (King, Gosset, Kothari, Babbush 2024) — Paper Notes]] — achieves triply efficient (sample + computation + few-copy) shadow tomography using two-copy measurements; closes the gap between sample-efficient and computationally efficient protocols left open here
- [[Power of Data in Quantum Machine Learning (Huang, Babbush, McClean et al 2021) — Paper Notes]] — uses the classical shadow formalism for projected quantum kernels; an application of shadow-type ideas to QML advantage

### Trick cards
- [[Gentle Search via Quantum OR]]
- [[Quantum OR Subroutine]]
- [[Postselected Hypothesis Update (Multiplicative Weights)]]
