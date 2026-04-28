# Gibbs Distribution Sampling via Quantum Walk Speedup

> **Source:** Chakrabarti, Childs, Li & Wu, arXiv:1809.01731 (2020); quantum walk speedup for Markov chains from [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes|Szegedy (2004)]]
> **Tags:** #trick #convex-optimization #gibbs-sampling #quantum-walk #Markov-chain #simplex #mixing-time

---

## What it does

Samples from the Gibbs (Boltzmann) distribution $\pi_\beta(x) \propto e^{-\beta f(x)}$ over the simplex $\Delta_n$ quadratically faster than classical Markov chain Monte Carlo, and uses this to minimize a convex function over the simplex.

---

## The trick

**The optimization-via-sampling reduction.** To minimize a convex function $f$ over the $n$-simplex $\Delta_n$, sample from the Gibbs distribution at inverse temperature $\beta$:
$$\pi_\beta(x) \propto e^{-\beta f(x)}, \quad x \in \Delta_n$$
For $\beta = O(\log n / \varepsilon)$, the Gibbs distribution concentrates within $\varepsilon$ of the optimal value in expectation:
$$\mathbb{E}_{x \sim \pi_\beta}[f(x)] \leq \min_{x \in \Delta_n} f(x) + \varepsilon$$
So an approximate sample from $\pi_\beta$ gives an $\varepsilon$-approximate optimizer.

**Classical Gibbs sampler.** Design a Markov chain on (a discretization of) $\Delta_n$ with stationary distribution $\pi_\beta$. The standard choice is a Langevin dynamics / Metropolis-Hastings chain. The mixing time $T_{\text{mix}}$ governs how many steps are needed to reach stationarity; for log-concave distributions on the simplex with $\beta = O(\log n / \varepsilon)$, classical MCMC achieves $T_{\text{mix}} = O(n^2 \log(1/\varepsilon))$ steps.

**Quantum speedup via Szegedy quantization.** Given a classical Markov chain $P$ on state space $\Omega$ with transition matrix $P_{xy}$, the [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes|Szegedy quantum walk]] defines a unitary $W(P)$ on $\mathbb{C}^\Omega \otimes \mathbb{C}^\Omega$ whose phase gap $\Delta(W) = \Omega(\sqrt{\delta(P)})$, where $\delta(P)$ is the classical spectral gap. Since the quantum walk **mixes** (in the sense of producing samples from $\pi_\beta$) in $O(1/\Delta(W)) = O(1/\sqrt{\delta(P)})$ steps rather than the classical $O(1/\delta(P))$, the mixing time is quadratically reduced:
$$T_{\text{mix}}^{\text{quantum}} = O\!\left(\sqrt{T_{\text{mix}}^{\text{classical}}}\right) = O\!\left(n \sqrt{\log(1/\varepsilon)}\right)$$

**Connecting to optimization.** The full algorithm is:
1. Discretize $\Delta_n$ into a grid with $N = n^{O(1)}$ points.
2. Define the Gibbs-Metropolis chain on the grid with transition probabilities $P_{xy} \propto \min(1, e^{-\beta(f(y)-f(x))})$ for adjacent grid points.
3. Build the Szegedy quantum walk $W(P)$; each step of $W(P)$ requires one quantum query to $f$ (to compute transition amplitudes).
4. Run the quantum walk for $O(\sqrt{T_{\text{mix}}^{\text{classical}}})$ steps.
5. Measure to obtain a sample $x$; output $x$ as the approximate optimizer.

Total quantum queries to $f$: $O(n^{3/2} \log(1/\varepsilon))$ — a $\sqrt{n}$ improvement over classical.

**Key ingredients from Szegedy's framework.** The quantum walk is defined on the "edge Hilbert space" $\mathcal{H} = \text{span}\{|x,y\rangle : P_{xy} > 0\}$. The discriminant matrix $D(P)$ has $(x,y)$-entry $\sqrt{P_{xy} P_{yx}}$, and the quantum walk unitary is $W(P) = \text{ref}(B) \cdot \text{ref}(A)$ where $A$ and $B$ are superpositions over outgoing and incoming edges. The phase gap of $W(P)$ is $\Omega(\sqrt{\delta(P)})$ by Szegedy's theorem, giving the quadratic speedup.

---

## When to reach for it

- You want to sample from a Gibbs distribution or minimize a function over a discrete/simplex domain where the objective can be evaluated quantumly.
- The classical Markov chain for Gibbs sampling has a known (polynomial in $n$) mixing time — the quantum speedup is quadratic, so it helps whenever the classical mixing time is not already fast.
- The function $f$ is available as a quantum oracle (coherent evaluation).
- Well-suited for simplex optimization with convex $f$; extends to other domains where Gibbs sampling is the classical algorithm of choice (e.g., low-temperature physics, discrete optimization via annealing).

---

## Complexity

$$O\!\left(n^{3/2} \log(1/\varepsilon)\right) \text{ quantum queries to } f$$

vs. $O(n^2 \log(1/\varepsilon))$ classical Gibbs sampling. Speedup factor: $O(\sqrt{n})$.

---

## Caveat

- **Mixing time dependence.** The speedup is quadratic in $T_{\text{mix}}^{\text{classical}}$, not absolute. If the classical chain already has $T_{\text{mix}} = O(1)$ (e.g., very simple geometry), there is nothing to speed up.
- **Log-concavity assumption.** The mixing time bound $T_{\text{mix}} = O(n^2)$ relies on the Gibbs distribution being log-concave (equivalently, $f$ convex and $\beta$ not too large). For non-convex $f$ or very low temperature ($\beta \gg \log n / \varepsilon$), the chain may mix exponentially slowly — the quantum walk gives a quadratic speedup in mixing time, not a speedup over an exponential.
- **Discretization overhead.** The simplex must be discretized with grid spacing $\delta \ll \varepsilon / \|\nabla f\|$; this discretization adds a factor of $(1/\delta)^n$ to the state space. For moderate $n$, the state space is exponentially large, and the walk must be on a sparse subgraph (nearest-neighbor) to remain tractable.
- **Readout.** The quantum walk outputs a quantum state; measuring gives a classical sample. If multiple samples are needed, the walk must be rerun each time.

---

## Related notes

- [[Quantum Algorithms and Lower Bounds for Convex Optimization (Chakrabarti-Childs-Li-Wu 2020) — Paper Notes]]
- [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes]]
- [[An Example of the Difference Between Quantum and Classical Random Walks (Childs-Farhi-Gutmann 2002) — Paper Notes]]
- [[Szegedy Walk Quantization of Hamiltonians]]
- [[Quantum Subgradient Method via Amplitude-Estimated Gradient Averaging]]
