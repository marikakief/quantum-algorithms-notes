> **Source:** Ashley Montanaro, *Quantum speedup of Monte Carlo methods*, arXiv:1504.06987, Theory of Computing **11**, 1–36 (2015)
> **Links:** [arXiv](https://arxiv.org/abs/1504.06987) · [Journal](https://doi.org/10.4086/toc.2015.v011a001)
> **Tags:** #monte-carlo #amplitude-estimation #partition-function #quantum-walk #sampling #statistical-physics

---

## The computational problem

**Input:** A randomised (or quantum) algorithm $A$ that outputs a real-valued random variable $v(A)$ with bounded variance $\text{Var}(v(A)) \leq \sigma^2$.

**Output:** An estimate $\tilde{\mu}$ of $\mu = \mathbb{E}[v(A)]$ such that $|\tilde{\mu} - \mu| \leq \epsilon$ with probability $\geq 2/3$.

**Classical cost:** $O(\sigma^2/\epsilon^2)$ independent runs of $A$ (tight by Chebyshev).

**Quantum cost (this paper):** $\tilde{O}(\sigma/\epsilon)$ — a near-quadratic improvement.

---

## What the paper does

Gives a general-purpose quantum algorithm that near-quadratically speeds up the estimation of expected values of any bounded-variance randomised or quantum subroutine. This is the quantum analogue of classical Monte Carlo sampling, and it's optimal (up to log factors) by the Nayak-Wu lower bound.

The real power comes from composition: the subroutine $A$ can itself be a quantum algorithm (e.g., a quantum walk sampler), so quantum speedups stack. The paper demonstrates this by combining quantum mean estimation with [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes|Szegedy quantum walks]] to speed up partition function computation — the workhorse of computational statistical physics.

---

## The algorithms

### Algorithm 1: Bounded output ($v(A) \in [0,1]$)

Encode the output value into the amplitude of an ancilla qubit:

$$W|x\rangle|0\rangle = |x\rangle\left(\sqrt{1-\phi(x)}|0\rangle + \sqrt{\phi(x)}|1\rangle\right)$$

where $\phi(x)$ is the output value when measurement yields $x$. Then $\langle\psi|P|\psi\rangle = \mathbb{E}[v(A)]$ where $P = I \otimes |1\rangle\langle 1|$.

Apply [[Amplitude Estimation via Phase Estimation on Q|amplitude estimation]] with $t$ iterations: get $|\tilde{\mu} - \mu| \leq O(\sqrt{\mu}/t + 1/t^2)$. For additive error $\epsilon$: $t = O(1/\epsilon)$.

### Algorithm 3: Bounded variance (the main result)

For general $A$ with $\text{Var}(v(A)) \leq \sigma^2$:

1. **Normalise.** Set $A' = A/\sigma$ so variance $\leq 1$.
2. **Rough estimate.** Run $A'$ once to get $\tilde{m}$ (a crude estimate of $\mu' = \mathbb{E}[v(A')]$). By Chebyshev, $|\tilde{m} - \mu'| \leq 3$ with probability $\geq 8/9$.
3. **Centre and split.** Define $B = A' - \tilde{m}$. Split into positive and negative parts: $B_{\geq 0}$ and $-B_{<0}$.
4. **Estimate each part.** Apply Algorithm 2 (the bounded-$\ell_2$-norm estimator, which uses Algorithm 1 as subroutine) to $B_{\geq 0}/4$ and $-B_{<0}/4$.
5. **Recombine.** $\tilde{\mu} = \sigma(\tilde{m} - 4\tilde{\mu}_- + 4\tilde{\mu}_+)$.

**Key intermediate step (Algorithm 2):** For non-negative outputs with bounded $|v(A)|_2$, split the output range into dyadic intervals $[2^{\ell-1}, 2^\ell)$, estimate each interval's contribution separately via Algorithm 1, and recombine. This is Heinrich's idea generalised from uniform distributions to arbitrary algorithms. Uses $\tilde{O}(1/\epsilon)$ calls.

**Total cost of Algorithm 3:** $\tilde{O}(\sigma/\epsilon)$ uses of $A$ and $A^{-1}$.

### Algorithm 4: Bounded relative variance

When $\text{Var}(v(A))/(\mathbb{E}[v(A)])^2 \leq B$ and we want relative error $\epsilon$:

1. Run $A$ $O(B)$ times for a rough estimate $\tilde{m}$.
2. Apply Algorithm 2 to $A/\tilde{m}$.

**Cost:** $\tilde{O}(B/\epsilon)$ — again near-quadratic over the classical $O(B/\epsilon^2)$.

---

## Application: partition functions

The partition function $Z(\beta) = \sum_{x \in \Omega} e^{-\beta H(x)}$ is approximated via telescoping products:

$$Z(\beta_\ell) = Z(\beta_0) \prod_{i=0}^{\ell-1} \frac{Z(\beta_{i+1})}{Z(\beta_i)}$$

Each ratio $\alpha_i = Z(\beta_{i+1})/Z(\beta_i)$ is the expected value of $Y_i(x) = e^{-(\beta_{i+1}-\beta_i)H(x)}$ under the Gibbs distribution $\pi_i$.

**Chebyshev cooling schedule:** Choose inverse temperatures $\beta_i$ such that $\mathbb{E}[Y_i^2]/\mathbb{E}[Y_i]^2 \leq B = O(1)$ for all $i$. Such a schedule of length $\ell = \tilde{O}(\sqrt{\log A})$ always exists (Štefankovič-Vempala-Vigoda), where $A = |\Omega|$.

### Classical cost

$\tilde{O}((\log A) \cdot \tau / \epsilon^2)$ Markov chain steps, where $\tau$ is the relaxation time.

### Quantum cost

$\tilde{O}((\log A) \cdot \sqrt{\tau} \cdot (1/\epsilon + \sqrt{\tau}))$ quantum walk steps.

The speedup is near-quadratic in both $\tau$ (from quantum walks) and $\epsilon$ (from quantum mean estimation). These compose because Algorithm 4 can use quantum walk sampling as its subroutine.

### Concrete examples

| Problem | Quantum | Classical |
|---|---|---|
| Ising model ($n$ vertices, $(d-1)\tanh\beta < 1$) | $\tilde{O}(n^{3/2}/\epsilon + n^2)$ | $\tilde{O}(n^2/\epsilon^2)$ |
| $k$-colourings ($k > 2d$) | $\tilde{O}(n^{3/2}/\epsilon + n^2)$ | $\tilde{O}(n^2/\epsilon^2)$ |
| Matchings ($n$ vertices, $m$ edges) | $\tilde{O}(n^{3/2}m^{1/2}/\epsilon + n^2 m)$ | $\tilde{O}(n^2 m/\epsilon^2)$ |

---

## How quantum walks fit in

The algorithm needs samples from Gibbs distributions $\pi_i$. Classically: run a Markov chain for $O(\tau)$ steps. Quantumly: use [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes|Szegedy walks]] for $O(\sqrt{\tau})$ steps.

The catch: Szegedy walks don't directly give classical samples — they produce coherent superposition states $|\pi\rangle = \sum_x \sqrt{\pi(x)}|x\rangle$. But that's fine here, because Algorithm 4 works with quantum subroutines. It needs $|\pi_i\rangle$ states and approximate reflections about them, not classical samples.

For producing $|\pi_i\rangle$ from the previous $|\pi_{i-1}\rangle$, the paper uses Wocjan-Abeyesinghe's result: if consecutive distributions have high overlap ($|\langle\pi_i|\pi_{i+1}\rangle|^2 \geq 1/B$, guaranteed by the Chebyshev schedule), then $|\pi_r\rangle$ can be prepared in $\tilde{O}(r\sqrt{\tau})$ quantum walk steps, and approximate reflections cost $\tilde{O}(\sqrt{\tau})$ each.

---

## Key results

| Algorithm | Precondition | Error | Uses of $A$ |
|---|---|---|---|
| Algorithm 1 | $v(A) \in [0,1]$ | Additive $\epsilon$ | $O(1/\epsilon)$ |
| Algorithm 3 | $\text{Var}(v(A)) \leq \sigma^2$ | Additive $\epsilon$ | $\tilde{O}(\sigma/\epsilon)$ |
| Algorithm 4 | $\text{Var}/\mu^2 \leq B$ | Relative $\epsilon$ | $\tilde{O}(B/\epsilon)$ |
| Partition functions | Chebyshev schedule, relaxation time $\tau$ | Relative $\epsilon$ | $\tilde{O}(\log A \cdot \sqrt{\tau}/\epsilon)$ |

All bounds are optimal up to polylog factors.

---

## Limits / caveats

- **Need $A^{-1}$.** The algorithm requires running the subroutine in reverse (uncomputation). For classical randomised algorithms this is easy (reversible computation). For inherently irreversible processes, you'd need to work around this.
- **Quadratic, not exponential.** This is a polynomial speedup, not an exponential one. For problems where $\sigma/\epsilon$ is already small, the improvement may not justify quantum overhead.
- **Mixing time assumption.** The partition function results assume known upper bounds on the relaxation time $\tau$ of the Markov chains. Getting tight bounds on $\tau$ is often the hard part in practice.
- **Chebyshev schedule computation is classical.** The cooling schedule is computed classically using $\tilde{O}(\log A \cdot \tau)$ Markov chain steps. This can dominate when $\tau$ is large — the quantum speedup applies to the estimation phase, not the schedule construction.
- **No-cloning bottleneck.** Classically, samples from $\pi_i$ can be reused as warm starts for $\pi_{i+1}$. Quantumly, the states $|\pi_i\rangle$ can't be cloned, so each must be prepared fresh. This is why the schedule computation isn't sped up.
- **State preparation cost can kill the speedup.** Herbert (2021, [arXiv:2101.02240](https://arxiv.org/abs/2101.02240)) showed that the quadratic advantage of quantum Monte Carlo vanishes when [[Creating Superpositions That Correspond to Efficiently Integrable Probability Distributions (Grover-Rudolph 2002) — Paper Notes|Grover-Rudolph]] state preparation is used with numerical (sampling-based) integration. The state-preparation error propagates into the mean estimate at rate $\tilde{\Omega}(1/\hat{\epsilon}^2)$, matching classical MC. Montanaro's framework itself is fine — the issue is the cost of the oracle $A$ that prepares the distribution. The speedup holds if the oracle cost doesn't grow with precision, or if the function applied to the samples is sufficiently complex relative to the distribution preparation.

---

## Reusable ideas

1. **[[Quantum Mean Estimation via Amplitude Estimation]]** — Encode the output of a randomised algorithm into an amplitude, then use [[Amplitude Estimation via Phase Estimation on Q|amplitude estimation]] to estimate the mean. Near-quadratic speedup over classical sampling. The key insight: split the output range into dyadic intervals, estimate each separately, recombine. Works for any bounded-variance subroutine.

2. **[[Telescoping Product Estimation for Partition Functions]]** — Express $Z(\beta)$ as a product of ratios $Z(\beta_{i+1})/Z(\beta_i)$, each of which is the expected value of a bounded-relative-variance random variable under the Gibbs distribution. Combine with Chebyshev cooling schedules to keep the number of ratios at $\tilde{O}(\sqrt{\log |\Omega|})$.

---

## References within this paper

- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|Brassard-Høyer-Mosca-Tapp (2002)]] — amplitude estimation (Theorem 2 in this paper)
- [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes|Szegedy (2004)]] — quantum walk framework for Markov chain speedup
- Wocjan & Abeyesinghe (2008) — quantum walk sampling from slowly-varying Markov chains; preparing coherent Gibbs states
- Somma, Boixo, Barnum & Knill (2008) — quantum simulated annealing; predecessor for walk-based speedups
- [[Spectral Gap Amplification (Somma-Boixo 2013) — Paper Notes|Somma-Boixo (2013)]] — related gap amplification for frustration-free Hamiltonians
- Heinrich (2002) — first optimal quantum algorithm for mean estimation under uniform distribution
- Nayak & Wu (2002) — lower bound $\Omega(1/\epsilon)$ for quantum mean estimation
- Štefankovič, Vempala & Vigoda (2009) — Chebyshev cooling schedules, fastest classical partition function algorithm
- Bravyi, Harrow & Hassidim (2011) — quantum algorithm for total variation distance
- Jerrum & Sinclair (1993) — Markov chain Monte Carlo for Ising model

---

## Cross-links

### Paper notes
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes]] — amplitude estimation is the core subroutine
- [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes]] — quantum walks provide the sampling primitive
- [[Spectral Gap Amplification (Somma-Boixo 2013) — Paper Notes]] — related: gap amplification speeds up adiabatic state preparation for related problems
- [[Search via Quantum Walk (Magniez-Nayak-Roland-Santha 2007) — Paper Notes]] — related walk-based algorithms
- [[Quantum Computational Finance — Monte Carlo Pricing of Financial Derivatives (Rebentrost-Gupt-Bromley 2018) — Paper Notes]] — applies this paper's quantum mean estimation to derivative pricing
- [[Quantum Walk Speedup of Backtracking Algorithms (Montanaro 2015) — Paper Notes]] — same author's companion paper; quadratic speedup for deterministic backtracking via quantum walk on trees
- [[Applying Quantum Algorithms to Constraint Satisfaction Problems (Campbell-Khurana-Montanaro 2019) — Paper Notes]] — practical follow-up combining backtracking walk with Grover and Monte Carlo speedup

### Trick cards
- [[Quantum Mean Estimation via Amplitude Estimation]]
- [[Telescoping Product Estimation for Partition Functions]]
- [[Amplitude Estimation via Phase Estimation on Q]]
- [[Standard Amplitude Amplification]]
- [[Quantized Bipartite Walk Construction]] — Szegedy walk used for sampling
