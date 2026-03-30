> **Source:** F. Magniez, A. Nayak, J. Roland, M. Santha, *Search via Quantum Walk*, SIAM J. Comput. **40**(1), 142–164 (2011); preliminary version in STOC 2007
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/0608026) · [SIAM](https://doi.org/10.1137/090745854)
> **Tags:** #quantum-walk #search #amplitude-amplification #Markov-chain #phase-estimation #Szegedy-walk

---

## The problem

Given a classical Markov chain $P$ on state space $X$ with stationary distribution $\pi$, and a set of "marked" elements $M \subseteq X$, find a marked element (or determine $M = \emptyset$). The classical cost depends on three primitives:
- **Setup cost $S$:** sampling from $\pi$ and building a data structure
- **Update cost $U$:** simulating one step of $P$ and updating the data structure
- **Checking cost $C$:** determining if a state is marked

Classically, the random walk approach costs $S + \frac{1}{\delta\varepsilon}(U + C)$ where $\delta$ is the spectral gap of $P$ and $\varepsilon = |M|/|X|$ is the marked fraction. The question: what's the best quantum algorithm?

## What the paper does

Unifies and improves the two prior quantum walk search frameworks — [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes|Ambainis (2004)]] and [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes|Szegedy (2004)]] — into a single algorithm that gets the best of both:

$$\text{Cost} = S + \frac{1}{\sqrt{\varepsilon}}\left(\frac{1}{\sqrt{\delta}} U + C\right)$$

This is quadratically better than classical in both the mixing time ($1/\delta \to 1/\sqrt{\delta}$) and the hitting time ($1/\varepsilon \to 1/\sqrt{\varepsilon}$), and the costs separate: checking happens $O(1/\sqrt{\varepsilon})$ times, while updates happen $O(1/\sqrt{\delta\varepsilon})$ times. When $C \gg U$ (e.g., Triangle Finding), this separation matters.

Prior to this paper:
- Ambainis achieved the separated cost $S + \frac{1}{\sqrt{\varepsilon}}(\frac{1}{\sqrt{\delta}} U + C)$ but only for specific Markov chains (Johnson graphs) with specific marked sets
- Szegedy achieved the combined cost $S + \frac{1}{\sqrt{\delta\varepsilon}}(U + C)$ for any symmetric ergodic chain, but only *detected* marked elements (couldn't find them), and the checking cost $C$ was multiplied by the walk cost

This paper: works for any ergodic reversible chain, finds marked elements, and achieves the separated cost.

---

## The algorithm

### Core idea: approximate reflection via phase estimation

The algorithm is a Grover-style rotation in the 2D subspace $\mathcal{S} = \text{Span}(|\pi\rangle, |\mu\rangle)$, where:
- $|\pi\rangle = \sum_x \sqrt{\pi_x}|x\rangle|p_x\rangle$ encodes the stationary distribution (lives in $A + B$ of the [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes|Szegedy quantum walk]])
- $|\mu\rangle$ is the projection of $|\pi\rangle$ onto the marked subspace, normalised

The Grover rotation needs two reflections:
1. $\text{ref}(\mu^\perp) = -\text{ref}(M)$: flip the phase of marked states. Cost: $C$.
2. $\text{ref}(\pi)$: reflect about $|\pi\rangle$. This is the hard one.

The naive approach to $\text{ref}(\pi)$ would undo the state preparation ($\text{cost } S + U$), reflect about $|0\rangle$, and redo the preparation. But $S$ may be much larger than $U$.

**The key observation:** the [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes|Szegedy walk]] $W(P) = \text{ref}(B) \cdot \text{ref}(A)$ has $|\pi\rangle$ as its unique eigenvalue-1 eigenvector in $A + B$, with all other eigenvalues bounded away from 1 by the [[Discriminant Matrix Spectral Theorem|phase gap]] $\Delta(P) \geq 2\sqrt{\delta}$. So [[Phase Estimation as Eigenvalue Filter for Walk-Based Search|phase estimation on $W(P)$]] can distinguish $|\pi\rangle$ from everything else.

### The approximate reflection $R(P)$

Run phase estimation on $W(P)$ with $s = \lceil\log_2(2\pi/\Delta(P))\rceil$ bits of precision, repeated $k$ times independently. Then:
- If all $k$ phase estimates are zero → don't flip the phase (this is $|\pi\rangle$ or very close)
- If any phase estimate is nonzero → flip the phase

This implements an approximate $\text{ref}(\pi)$ with error $\|R(P) + \text{Id}\| \leq 2^{1-k}$ on the orthogonal complement of $|\pi\rangle$. Cost per application: $O(k \cdot \frac{1}{\sqrt{\delta}})$ calls to $W(P)$, so $O(\frac{k}{\sqrt{\delta}})$ updates.

### Handling the approximation error

A naive approach would set $k = O(\log(1/\sqrt{\varepsilon}))$, yielding the extra log factor in the cost:

$$S + \frac{1}{\sqrt{\varepsilon}}\left(\frac{\log(1/\sqrt{\varepsilon})}{\sqrt{\delta}} U + C\right)$$

To remove the log, the paper adapts [[Recursive Amplitude Amplification (Høyer-Mosca-de Wolf)|recursive amplitude amplification (RAA)]] from Høyer, Mosca, and de Wolf. Instead of iterating Grover directly, define procedures recursively:
$$A_0 = \text{Id}, \quad A_i = A_{i-1} \cdot \text{ref}(\pi) \cdot A_{i-1}^\dagger \cdot \text{ref}(\mu^\perp) \cdot A_{i-1}$$

Each $A_i$ rotates by $3^i \varphi$ (where $\sin\varphi = \sqrt{p_M}$), so $t = \log_3(1/\varphi)$ levels suffice. At level $i$, use an approximate reflection $R(\beta_i)$ with precision $\beta_i = O(\gamma/i^2)$. The errors form a convergent series, and the total cost is $O(3^t \cdot (c_1 \log(1/\gamma) + c_2)) = O(\frac{1}{\sqrt{\varepsilon}}(\frac{1}{\sqrt{\delta}} U + C))$ — no extra log factor.

### Extension to non-reversible chains

For non-reversible ergodic chains, the discriminant matrix $D(P) = (\sqrt{p_{xy} p^*_{yx}})$ may not be symmetric. The algorithm still works, replacing the spectral gap $\delta(P)$ with the singular value gap of $D(P)$. The phase gap of $W(P)$ is still $\geq 2\sqrt{\delta_{\text{sv}}}$, where $\delta_{\text{sv}}$ is this singular value gap.

---

## Key results

| Result | Statement |
|---|---|
| Main theorem (reversible) | Cost $S + \frac{1}{\sqrt{\varepsilon}}\left(\frac{1}{\sqrt{\delta}} U + C\right)$, finds marked element w.h.p. |
| Main theorem (non-reversible) | Same, with $\delta$ replaced by singular value gap of $D(P)$ |
| Phase gap quadratic amplification | $\Delta(P) \geq 2\sqrt{\delta}$ — the quantum walk quadratically amplifies the classical gap |
| Approximate reflection | $R(P)$ approximates $\text{ref}(\pi)$ to error $\leq 2^{1-k}$ using $O(k/\sqrt{\delta})$ walk steps |
| RAA removes log | Recursive amplitude amplification eliminates the $\log(1/\sqrt{\varepsilon})$ overhead |

---

## Comparison with prior work

| | Ambainis (2004) | [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes|Szegedy (2004)]] | **MNRS (2007)** |
|---|---|---|---|
| Cost | $S + \frac{1}{\sqrt{\varepsilon}}(\frac{1}{\sqrt{\delta}} U + C)$ | $S + \frac{1}{\sqrt{\delta\varepsilon}}(U + C)$ | $S + \frac{1}{\sqrt{\varepsilon}}(\frac{1}{\sqrt{\delta}} U + C)$ |
| Finds element? | ✓ | ✗ (detection only) | ✓ |
| Chain class | Johnson graphs + structured $M$ | Symmetric ergodic | **Any ergodic** (reversible or non-reversible) |
| Checking cost | Separated | Not separated | **Separated** |
| Analysis | Ad hoc, hard to extend | General | **General + simple** |

When $C \ll U/\sqrt{\delta}$, all three are comparable. When $C \gg U/\sqrt{\delta}$ (e.g., Triangle Finding where checking involves a Grover search), the Ambainis/MNRS cost structure wins. Szegedy detects but can't find; MNRS does both.

---

## Why the quadratic speedup?

The origin is clean: the Szegedy walk $W(P)$ has phase gap $\Delta(P) \geq 2\sqrt{\delta}$, a quadratic amplification of the classical spectral gap $\delta$. This comes from the relationship $\Delta(P) \geq |1 - e^{2i\theta_1}| = 2\sqrt{1 - |\lambda_1|^2} \geq 2\sqrt{\delta}$, where $\cos\theta_1 = |\lambda_1| = 1 - \delta$.

Phase estimation on the walk then discriminates $|\pi\rangle$ (eigenvalue 1) from everything else (eigenvalues at angular distance $\geq \Delta(P)$) using $O(1/\Delta(P)) = O(1/\sqrt{\delta})$ walk steps. This is the walk-based analogue of the $O(1/\sqrt{N})$ Grover speedup: the walk mixes quadratically faster in the quantum setting.

---

## Applications improved

| Problem | Previous best | MNRS improvement |
|---|---|---|
| Element Distinctness | $O(N^{2/3})$ queries (Ambainis) — single solution only | Same complexity, arbitrary number of solutions |
| Triangle Finding | $O(N^{13/10})$ (Magniez-Santha-Szegedy) | $O(N^{13/10}/\text{polylog})$ — removes polylog |
| Group Commutativity | $O(N^{2/3} \log N)$ (Magniez-Nayak) — detection only | Same complexity, now finds non-commuting pair |
| Matrix Product Verification | Szegedy detection | Now finds witness |

---

## Limits / caveats

- **Mixing time assumption.** The cost depends on $1/\sqrt{\delta}$, which can be exponential for badly connected chains. The algorithm doesn't help you choose a good Markov chain.
- **Setup cost.** The $S$ term is paid once and can dominate — for Johnson graph applications, $S$ involves loading $r$-element subsets.
- **Reversibility assumption** for the clean result. The non-reversible extension works but requires the singular value gap of the discriminant, which is less well-studied than spectral gaps.
- **Finding vs. detection gap remains open** in general. For state-transitive chains with a unique marked state, later work (Krovi-Magniez-Ozols-Roland 2015) closes this, but the general relationship between detection and finding quantum hitting times is still not fully resolved.
- **No speedup of hitting time** in general — the paper gives a speedup of $1/\delta\varepsilon$ (the "extended" hitting time), not the potentially smaller actual hitting time. Szegedy's result uses the actual hitting time for detection, but it's unclear if this extends to finding.

---

## Reusable ideas

1. [[Phase Estimation as Eigenvalue Filter for Walk-Based Search]] — use phase estimation on a quantum walk to implement an approximate reflection about the stationary state, discriminating the eigenvalue-1 component from the rest at cost $O(1/\sqrt{\delta})$ walk steps
2. [[Recursive Amplitude Amplification with Approximate Reflections]] — adapt RAA to tolerate imperfect reflections by using increasingly precise approximations at each recursion level, with errors forming a convergent series; eliminates log factors

---

## References within this paper

- [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes|Ambainis (2004/2007)]] — the first quantum walk search algorithm; specific to Johnson graphs
- [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes|Szegedy (2004)]] — the quantum walk framework ($W(P) = \text{ref}(B) \cdot \text{ref}(A)$, discriminant matrix, detection)
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|Brassard-Høyer-Mosca-Tapp (2002)]] — amplitude amplification framework
- Høyer, Mosca, de Wolf (2003) — recursive amplitude amplification; basis for RAA technique
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|Kitaev (1995)]], Cleve-Ekert-Macchiavello-Mosca (1998) — phase estimation
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover (1996)]] — the original $O(\sqrt{N})$ search
- [[Quantum Algorithms for the Triangle Problem (Magniez-Santha-Szegedy 2007) — Paper Notes|Magniez-Santha-Szegedy (2005)]] — Triangle Finding via recursive quantum walk
- Magniez-Nayak (2005) — Group Commutativity via quantum walk

---

## Cross-links

### Paper notes
- [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes]] — this paper directly builds on Szegedy's walk framework
- [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes]] — the other predecessor; MNRS unifies the two
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes]] — amplitude amplification used in the outer loop
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]] — MNRS generalises Grover to walk-based settings
- [[Exponential Algorithmic Speedup by Quantum Walk (Childs-Cleve-Deotto-Farhi-Gutmann-Spielman 2003) — Paper Notes]] — continuous-time walk-based speedup; different walk paradigm
- [[Any AND-OR Formula Can Be Evaluated in O(N^{1⁄2+o(1)}) Queries (Ambainis-Childs-Reichardt-Špalek-Zhang 2007) — Paper Notes]] — uses Szegedy walk for formula evaluation
- [[Quantum Algorithms for the Triangle Problem (Magniez-Santha-Szegedy 2007) — Paper Notes]] — applies the MNRS framework to triangle finding; Magniez and Santha are co-authors on both
- [[Quantum Walks Can Find a Marked Element on Any Graph (Krovi-Magniez-Ozols-Roland 2016) — Paper Notes]] — supersedes for finding: achieves $O(\sqrt{\mathrm{HT}})$ on any reversible chain via interpolated walk; removes state-transitivity requirement

### Trick cards
- [[Phase Estimation as Eigenvalue Filter for Walk-Based Search]]
- [[Recursive Amplitude Amplification with Approximate Reflections]]
- [[Discriminant Matrix Spectral Theorem]] — the spectral decomposition of $W(P)$ that makes the phase estimation approach work
- [[Szegedy Walk Quantization of Hamiltonians]] — the walk construction
- [[Quantized Bipartite Walk Construction]] — the $\text{ref}(B) \cdot \text{ref}(A)$ structure
- [[Walk on the Johnson Graph for Subset Search]] — the specific walk used in Element Distinctness
