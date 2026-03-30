> **Source:** Hari Krovi, Frédéric Magniez, Maris Ozols, Jérémie Roland, *Quantum walks can find a marked element on any graph*, Algorithmica **74**(2):851–907, 2016; arXiv:1002.2419
> **Links:** [arXiv](https://arxiv.org/abs/1002.2419) · [Algorithmica](https://doi.org/10.1007/s00453-015-9979-8)
> **Tags:** #quantum-walk #search #spatial-search #hitting-time #Markov-chain #interpolated-walk #eigenvalue-estimation

---

## The computational problem

**Spatial search on graphs (Find):** Given an undirected graph $G = (X, E)$ with $|X| = n$ vertices, a reversible ergodic Markov chain $P$ on $G$, and a set of marked vertices $M \subseteq X$ (promised non-empty), find any marked vertex. Operations allowed: Setup($P$) (sample from stationary distribution $\pi$), Update($P$) (one step of $P$), Check($M$) (query whether a vertex is marked).

**Classical cost:** $S + \mathrm{HT}(P, M) \cdot (U + C)$, where $\mathrm{HT}(P, M)$ is the hitting time — expected steps to reach $M$ starting from $\pi$ conditioned on starting unmarked.

**Open question answered:** Does a quantum walk achieve quadratic speedup $O(\sqrt{\mathrm{HT}(P,M)})$ for *finding* (not just detecting) a marked vertex on *any* reversible Markov chain?

## What the paper does

Resolves the open question affirmatively for single marked elements: a quantum walk finds a marked vertex on **any** graph in $O(\sqrt{\mathrm{HT}(P,M)})$ steps, a full quadratic speedup over the classical hitting time. For multiple marked elements, the cost is $O(\sqrt{\mathrm{HT}^+(P,M)})$ where $\mathrm{HT}^+$ is a new quantity called the *extended hitting time*, which equals $\mathrm{HT}$ when $|M| = 1$ but can be larger in general.

The approach is conceptually cleaner than all prior work. Instead of bolting Grover-style reflections onto a walk operator (as in [[Search via Quantum Walk (Magniez-Nayak-Roland-Santha 2007) — Paper Notes|MNRS (2007)]]) or requiring state-transitivity (as in Tulsi (2008) / [[Search via Quantum Walk (Magniez-Nayak-Roland-Santha 2007) — Paper Notes|MNRS (2012)]]), it directly quantizes an *interpolated* Markov chain $P(s) = (1-s)P + sP'$ between the original walk $P$ and the absorbing walk $P'$. The quantum algorithm is then just [[Phase Estimation as Eigenvalue Filter for Walk-Based Search|eigenvalue estimation]] on the Szegedy walk operator $W(s)$ of this interpolated chain.

## The algorithm

### The interpolated walk

The key idea is the interpolated Markov chain:

$$P(s) = (1-s)P + sP', \quad s \in [0,1]$$

where $P'$ is the **absorbing walk** — identical to $P$ except that outgoing transitions from marked vertices are replaced by self-loops. As $s$ increases from 0 to 1, the chain smoothly transitions from $P$ (uniform exploration) to $P'$ (absorption at marked vertices).

The stationary distribution $\pi(s)$ interpolates between $\pi$ (stationary distribution of $P$) and its restriction to $M$.

### The quantum walk operator $W(s)$

Following [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes|Szegedy's construction]], the quantum analogue of $P(s)$ is:

$$W(s) = V(P(s))^\dagger \cdot \mathrm{Shift} \cdot V(P(s)) \cdot \mathrm{ref}_X$$

where $V(P(s))|x\rangle|\bar{0}\rangle = |x\rangle|p_x(s)\rangle = |x\rangle\sum_y \sqrt{P(s)_{xy}}|y\rangle$ prepares the transition superposition.

The eigenvalues of $W(s)$ are $e^{\pm i\varphi_k(s)}$ where $\cos\varphi_k(s) = \lambda_k(s)$ are the eigenvalues of the discriminant matrix $D(s) = D(P(s))$. The unique $+1$ eigenvector is $|\Psi_n(s)\rangle = |v_n(s)\rangle|\bar{0}\rangle$.

### The geometric picture

The principal eigenvector $|v_n(s)\rangle$ lives in the 2D subspace spanned by $|U\rangle$ (normalised uniform superposition over unmarked vertices) and $|M\rangle$ (normalised uniform superposition over marked vertices, weighted by $\pi$):

$$|v_n(s)\rangle = \cos\theta(s)\,|U\rangle + \sin\theta(s)\,|M\rangle$$

where

$$\cos^2\theta(s) = \frac{(1-s)(1-p_M)}{1-s(1-p_M)}, \quad \sin^2\theta(s) = \frac{p_M}{1-s(1-p_M)}$$

and $p_M = \sum_{x \in M} \pi_x$ is the probability of sampling a marked vertex from $\pi$.

The interpolation parameter $s$ tunes the overlap: at $s = 0$, $|v_n\rangle \approx |U\rangle$; at $s \to 1$, $|v_n\rangle \approx |M\rangle$. The algorithm chooses $s$ to balance both overlaps.

### The search algorithm

**Search($P, M, s, t$):**

1. Prepare $|\pi\rangle|\bar{0}\rangle$ using Setup($P$).
2. Check if the current vertex is marked. If yes, output and stop.
3. Otherwise, the state is $|U\rangle|\bar{0}\rangle$. Run Eigenvalue Estimation($W(s)$, $t$) with $t = \lceil\log T\rceil$ bits of precision.
4. The eigenvalue estimation projects $|U\rangle|\bar{0}\rangle$ onto the $+1$ eigenspace of $W(s)$, yielding (approximately) $|v_n(s)\rangle$. Measure the vertex register. Output if marked.

**Why it works:** After eigenvalue estimation with precision $T$, the probability of finding a marked vertex is at least:

$$q \geq (\cos\theta(s)\sin\theta(s) - \varepsilon_2)^2$$

where $\varepsilon_2 = \frac{\pi}{\sqrt{2}} \cdot \frac{\sqrt{\mathrm{HT}(s)}}{T}$ is the phase estimation error. Choosing $T = O(\sqrt{\mathrm{HT}(s)})$ makes this a constant.

### The optimal interpolation parameter

The optimal $s$ that maximises $\cos\theta(s)\sin\theta(s)$ is:

$$s^* = 1 - \sqrt{\frac{p_M}{1-p_M}}$$

giving $\cos\theta(s^*) = \sin\theta(s^*) = 1/\sqrt{2}$ (equal overlap on marked and unmarked). An approximation $p^*$ of $p_M$ with $|p^* - p_M| \leq p_M/3$ suffices.

### Extended hitting time

The *interpolated hitting time* is:

$$\mathrm{HT}(s) = \sum_{k=1}^{n-1} \frac{|\langle v_k(s)|U\rangle|^2}{1 - \lambda_k(s)}$$

The extended hitting time is $\mathrm{HT}^+(P,M) = \lim_{s \to 1} \mathrm{HT}(s)$, and satisfies the relation:

$$\mathrm{HT}(s) = \frac{p_M^2}{(1 - s(1-p_M))^2}\,\mathrm{HT}^+(P,M)$$

For $|M| = 1$: $\mathrm{HT}^+(P,M) = \mathrm{HT}(P,M)$.

### Handling unknown parameters

The paper provides several variants:

| Variant | Known | Complexity |
|---|---|---|
| Theorem 20 | $p_M$, $\mathrm{HT}^+$ exactly | $S + O(\sqrt{\mathrm{HT}^+}) \cdot (U + C)$ |
| Theorem 23 | $p_M$ approximately, $\mathrm{HT}^+$ | $S + O(\sqrt{\mathrm{HT}^+}) \cdot (U + C)$ |
| Theorem 24 | $p_M$ approximately | $O(\log T) \cdot S + T \cdot (U + C)$, $T = \sqrt{\mathrm{HT}^+}$ |
| Theorem 25 | lower bound $p_{\min} \leq p_M$ | $\sqrt{\log(1/p_{\min})} \cdot (O(\log T) \cdot S + T \cdot (U+C))$ |
| Theorem 26 | upper bound $\mathrm{HT}_{\max} \geq \mathrm{HT}^+$ | $O(\log(1/p_M)) \cdot (S + \sqrt{\mathrm{HT}_{\max}} \cdot (U+C))$ |

## Key results

**Main theorem (single marked element):** For any reversible ergodic Markov chain $P$ on graph $G$ with a single marked vertex, a marked vertex can be found with high probability in quantum complexity:

$$S + O\!\left(\sqrt{\mathrm{HT}(P,M)}\right) \cdot (U + C)$$

This is a full quadratic speedup over the classical hitting time, with no symmetry assumptions on $P$.

**Multiple marked elements:** The complexity becomes $O(\sqrt{\mathrm{HT}^+(P,M)})$, where $\mathrm{HT}^+ \geq \mathrm{HT}$ in general. Whether $\mathrm{HT}^+ = \Theta(\mathrm{HT})$ for all $P, M$ remains open.

## Comparison with prior work

| Paper | Finds? | Any $P$? | Multiple $M$? | Cost |
|---|---|---|---|---|
| [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes\|Szegedy (2004)]] | Detect only | Symmetric only | Yes | $O(\sqrt{\mathrm{HT}})$ |
| [[Search via Quantum Walk (Magniez-Nayak-Roland-Santha 2007) — Paper Notes\|MNRS (2007)]] | Yes | Any ergodic | Yes | $S + \frac{1}{\sqrt{\varepsilon}}(\frac{1}{\sqrt{\delta}}U + C)$ — not always $\sqrt{\mathrm{HT}}$ |
| Tulsi (2008) | Yes | State-transitive | $\|M\|=1$ only | $O(\sqrt{\mathrm{HT}})$ |
| MNRS (2012) | Yes | State-transitive | $\|M\|=1$ only | $O(\sqrt{\mathrm{HT}})$ |
| [[Spatial Search by Quantum Walk (Childs-Goldstone 2004) — Paper Notes\|Childs-Goldstone (2004)]] | Yes | Specific lattices | $\|M\|=1$ only | $O(\sqrt{N})$ for $d > 4$ |
| **This paper** | **Yes** | **Any reversible** | **Yes** ($\mathrm{HT}^+$) | $O(\sqrt{\mathrm{HT}})$ for $\|M\|=1$ |

The 2D grid illustrates the gap between prior approaches: [[Search via Quantum Walk (Magniez-Nayak-Roland-Santha 2007) — Paper Notes|MNRS (2007)]] gives $\Theta(n)$ (no speedup over classical $\Theta(n \log n)$), while this paper gives $O(\sqrt{n \log n})$ — a genuine quadratic improvement.

## Limits / caveats

- For **multiple marked elements**, the cost is $O(\sqrt{\mathrm{HT}^+})$ rather than $O(\sqrt{\mathrm{HT}})$. The extended hitting time $\mathrm{HT}^+$ can exceed $\mathrm{HT}$ (the paper gives an explicit example in Appendix C). Whether this gap is inherent or an artefact of the analysis is open.
- Requires an approximation of $p_M$ or bounds on $p_M$ / $\mathrm{HT}^+$. When neither is known, there's a $\sqrt{\log(1/p_{\min})}$ or $\log(1/p_M)$ overhead from binary search.
- The interpolation idea is inspired by the adiabatic algorithm of Krovi, Ozols, Roland (2010), but the circuit-based version here avoids the adiabatic algorithm's drawbacks (gap estimation, scheduling).
- The walk must be **reversible**. Extensions to non-reversible chains would require different techniques (see [[Search via Quantum Walk (Magniez-Nayak-Roland-Santha 2007) — Paper Notes|MNRS (2007)]] for the non-reversible detection case).

## Reusable ideas

1. [[Interpolated Walk for Quantum Search]] — Interpolating between a Markov chain and its absorbing version, then applying the Szegedy walk of the interpolated chain. The interpolation parameter $s$ controls the overlap of the principal eigenvector on marked vs unmarked vertices, enabling a smooth transition that eigenvalue estimation can exploit.

2. [[Phase Estimation as Eigenvalue Filter for Walk-Based Search]] — Using phase/eigenvalue estimation not to learn eigenvalues, but as a **filter** that projects onto the $+1$ eigenspace of a walk operator. The projection maps $|U\rangle$ to $|v_n(s)\rangle$, which has controlled overlap on $|M\rangle$. This is the same idea underlying [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes|Szegedy's detection algorithm]], but here applied to the interpolated walk.

3. [[Incremental Eigenvalue Estimation]] — When the required precision $T$ is unknown, doubling the precision parameter $t$ iteratively (with constant repetitions at each level) finds a marked element with the same expected complexity $O(\sqrt{\mathrm{HT}^+})$, up to logarithmic factors in the setup cost.

## References within this paper

- [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes|Szegedy (2004)]] — The Szegedy walk construction that this paper builds on. Szegedy could detect but not find.
- [[Search via Quantum Walk (Magniez-Nayak-Roland-Santha 2007) — Paper Notes|MNRS (2007)]] — The previous best general quantum walk search framework. Their cost $S + \frac{1}{\sqrt{\varepsilon}}(\frac{1}{\sqrt{\delta}}U + C)$ doesn't always match $\sqrt{\mathrm{HT}}$.
- Tulsi (2008), MNRS (2012) — Extended Szegedy's detection to finding, but only for state-transitive chains with $|M| = 1$.
- [[Spatial Search by Quantum Walk (Childs-Goldstone 2004) — Paper Notes|Childs-Goldstone (2004)]] — Continuous-time spatial search achieving $\sqrt{N}$ on lattices $d > 4$. This paper's discrete-time approach works on any graph.
- [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes|Ambainis (2007)]] — First application of quantum walk to a natural search problem (element distinctness).
- Krovi, Ozols, Roland (2010) — Adiabatic approach to the same problem (preliminary conference version). The interpolated walk idea originates there, but the adiabatic version has practical drawbacks.
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover (1996)]] — Special case: unstructured search is quantum walk on the complete graph.

## Cross-links

### Paper notes
- [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes]]
- [[Search via Quantum Walk (Magniez-Nayak-Roland-Santha 2007) — Paper Notes]]
- [[Spatial Search by Quantum Walk (Childs-Goldstone 2004) — Paper Notes]]
- [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes]]
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]]
- [[Quantum Walks and Their Algorithmic Applications (Ambainis 2003) — Paper Notes]]
- [[Quantum Walk Speedup of Backtracking Algorithms (Montanaro 2015) — Paper Notes]]

### Trick cards
- [[Interpolated Walk for Quantum Search]]
- [[Phase Estimation as Eigenvalue Filter for Walk-Based Search]]
- [[Incremental Eigenvalue Estimation]]
- [[Discriminant Matrix Spectral Theorem]]
- [[Amplitude-Amplified State Preparation inside Walk Operators]]
