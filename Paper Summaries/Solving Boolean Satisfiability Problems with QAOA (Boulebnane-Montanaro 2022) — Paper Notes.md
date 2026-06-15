> **Source:** Sami Boulebnane and Ashley Montanaro, *Solving boolean satisfiability problems with the quantum approximate optimization algorithm*, arXiv:2208.06909 (2022)
> **Links:** [arXiv](https://arxiv.org/abs/2208.06909) · [PDF](https://arxiv.org/pdf/2208.06909)
> **Tags:** #QAOA #SAT #random-k-SAT #constraint-satisfaction #average-case #saddle-point #near-term

---

## The computational problem

**Input:** a random $k$-SAT instance on $n$ Boolean variables, generated as follows. Sample

$$
m \sim \mathrm{Poisson}(rn),
$$

then draw $m$ clauses independently, each clause containing $k$ literals chosen uniformly with replacement from

$$
\{x_0,\bar{x}_0,\ldots,x_{n-1},\bar{x}_{n-1}\}.
$$

The paper focuses on $r$ near the random-$k$-SAT satisfiability threshold. For example, their numerical benchmarks use

$$
r_8 \approx 176.54.
$$

**Hamiltonian:** for an instance $\sigma$, define the diagonal cost Hamiltonian

$$
H[\sigma]=\sum_{y\in\{0,1\}^n} |\{j: y\not\models \sigma_j\}|\,|y\rangle\langle y|,
$$

so satisfying assignments are exactly the zero-energy states. The success projector is

$$
\{H[\sigma]=0\}.
$$

**QAOA state:**

$$
|\Psi(\sigma,\beta,\gamma)\rangle
= e^{-i\beta_{p-1}H_B/2}e^{-i\gamma_{p-1}H[\sigma]/2}\cdots
  e^{-i\beta_0H_B/2}e^{-i\gamma_0H[\sigma]/2}|+\rangle^{\otimes n},
$$

where

$$
H_B=\sum_{j=0}^{n-1}X_j.
$$

**Output goal:** maximise the probability of sampling a satisfying assignment,

$$
p_{\mathrm{succ}}(\sigma)=
\langle\Psi(\sigma,\beta,\gamma)|\{H[\sigma]=0\}|\Psi(\sigma,\beta,\gamma)\rangle.
$$

Repeated sampling gives expected runtime $1/p_{\mathrm{succ}}(\sigma)$ on a satisfiable instance.

---

## What the paper does

This is QAOA treated as an **exact-solution sampler** for hard random SAT, not as an approximate optimiser. That shift matters: the objective is not expected energy, but the probability mass on the zero-energy subspace.

The main theoretical result is an analytic method for the average success probability

$$
\mathbb{E}_{\sigma\sim \mathrm{CNF}(n,k,r)}[p_{\mathrm{succ}}(\sigma)]
$$

in the large-$n$ limit, for fixed QAOA depth and fixed angles, when $k$ is a power of two. Technically, the paper rewrites the average success probability as a generalized multinomial sum and estimates its exponential scaling by a saddle-point calculation.

The empirical claim is more provocative: for random 8-SAT near the threshold, fixed-angle QAOA with roughly $p\approx 14$ layers matches the scaling exponent of WalkSATlm, the best classical solver they tested. Larger $p$ looks better in their simulations, but the evidence is not yet a clean quantum advantage claim. The gap between average success probability and median runtime becomes visible at large $p$, and the numerical experiments only reach small $n$.

My assessment: this is one of the more serious QAOA-for-CSP papers because it compares against strong classical SAT heuristics and tracks exponential scaling exponents, not just toy instance counts. But it is still evidence, not a theorem of advantage. The theory covers average success probability under small-$\gamma$ assumptions; the strongest performance claims rely on extrapolating small-$n$ simulations.

---

## The algorithm / construction

### 1. Use QAOA as a SAT sampler

For fixed angles $(\beta,\gamma)$, run the QAOA circuit and measure in the computational basis. If the sampled bitstring satisfies all clauses, stop; otherwise repeat.

The expected number of samples for a fixed satisfiable instance is

$$
T(\sigma)=\frac{1}{p_{\mathrm{succ}}(\sigma)}.
$$

This differs from standard QAOA analysis, which usually optimises

$$
\langle H[\sigma]\rangle
$$

or an approximation ratio. Here the low-energy tail is what matters.

### 2. Poissonise the random formula

The number of clauses is Poisson with mean $rn$, not fixed to $\lfloor rn\rfloor$. This is not cosmetic: it makes the clause average factor cleanly and turns the average success probability into a form amenable to a generalized multinomial expansion.

### 3. Average over random clauses

For bitstrings $y^{(0)},\ldots,y^{(J-1)}$, the probability that a random $k$-clause is simultaneously unsatisfied by all of them is

$$
\left(\frac{|y^{(0)}\cap\cdots\cap y^{(J-1)}|}{2n}\right)^k,
$$

where the intersection denotes coordinates on which the bitstrings all agree, with the literal polarity forced accordingly.

This converts clause averaging into a function only of the empirical type counts

$$
n_s = \left|\{i\in[n] : (y_i^{(0)},\ldots,y_i^{(2p)})=s\}\right|,
\qquad s\in\{0,1\}^{2p+1}.
$$

### 4. Express the success probability as a generalized multinomial sum

The QAOA path expansion involves $2p+1$ computational-basis strings: one central string for the success projector and $p$ strings on each side from the forward/backward phase unitaries. After grouping coordinates by type $s\in\{0,1\}^{2p+1}$, the expected success probability becomes a multinomial sum over the counts $n_s$:

$$
\mathbb{E}_{\sigma}[p_{\mathrm{succ}}]
=2^{-n}e^{-2^{-k}rn(1+4\sum_j\sin^2(\gamma_j/4))}
\sum_{\sum_s n_s=n}
\binom{n}{(n_s)_s}
\prod_s B_{\beta,s}^{n_s}
\exp\!
\left(
 rn \sum_J c_J
 \left(\frac{1}{2n}\sum_{s:\,s_j\text{ equal on }J} n_s\right)^k
\right).
$$

The notation in the paper is heavier, but the idea is simple: QAOA amplitudes give the $B_{\beta,s}$ factors, and the random-SAT clause average gives the polynomial in the type frequencies.

### 5. Saddle-point estimate

For $k=2^q$ and sufficiently small phase angles $\gamma$, the generalized multinomial sum has a unique saddle point $z^*$, and the large-$n$ exponent is computable from that saddle point. Proposition 3 gives

$$
\lim_{n\to\infty}\frac{1}{n}\log \mathbb{E}_{\sigma}[p_{\mathrm{succ}}]
= F(z^*)+(2^q-1)\sum_{\alpha\subseteq[2p+1]}
\left(\frac{\partial F}{\partial z_\alpha}(z^*)\right)^{2^q}.
$$

The formula is not the part to memorise. The transferable idea is the reduction:

$$
\text{random-CSP QAOA success probability}
\to \text{type-count multinomial sum}
\to \text{saddle point exponent}.
$$

---

## Key results

### Analytic scaling of average success probability

For fixed $p$, fixed angles, $k=2^q$, and sufficiently small $\gamma$, the average success probability has a well-defined exponential rate:

$$
\mathbb{E}_{\sigma}[p_{\mathrm{succ}}] \approx 2^{-c(p,k)n}
$$

up to subexponential factors, with $c(p,k)$ computed by the saddle-point algorithm.

### Numerical validation

The predicted limiting exponent agrees well with direct state-vector simulations on small random instances. They test $n\in\{12,\ldots,20\}$ and compare against analytic predictions; for $p=1$ they can also compute finite-size expectations up to around $n=70$ in $O(n^3)$.

### Scaling with QAOA depth

The fitted exponents decrease with $p$. Figure 2 reports fits of the form

$$
c(p)\approx 0.13p^{-1.12}\quad(k=2),
$$

$$
c(p)\approx 0.57p^{-0.51}\quad(k=4),
$$

$$
c(p)\approx 0.69p^{-0.32}\quad(k=8).
$$

The $k=8$ exponent improves only slowly with $p$, but enough to make the classical comparison interesting.

### Comparison with SAT solvers

For random 8-SAT near threshold, their fitted median runtime exponents include:

- WalkSATlm: roughly $2^{0.325n}$
- QAOA, $p=14$: roughly $2^{0.326n}$
- QAOA, $p=60$: median-runtime fit roughly $2^{0.302n}$
- QAOA, $p=60$: average-success-probability extrapolation could be as low as about $2^{0.19n}$

The last two numbers should not be conflated. The average success probability can be dominated by easier instances, whereas median runtime is closer to the SAT-solver benchmarking norm.

### Amplitude amplification option

If the QAOA circuit is run fault-tolerantly and the satisfying-assignment check is coherent, amplitude amplification can reduce the sample exponent by a factor of two:

$$
T \sim 2^{cn}\quad \leadsto\quad \sqrt{T}\sim 2^{cn/2}.
$$

That is a different regime from near-term QAOA, but it is the cleanest way to convert their sampler into a search algorithm with a standard quantum speedup.

---

## Why this matters

The paper gives a route to analyse QAOA on **solution-finding** CSPs where success probabilities are exponentially small but the exponent is the whole story. This is a better match to SAT than expected-energy optimisation.

It also shifts QAOA benchmarking toward the right question: not “does QAOA improve an approximation ratio on tiny instances?”, but “what exponential TTS exponent does it achieve against tuned classical heuristics on the same random ensemble?”

For Marika's interests, the useful part is the analytic method: a typical-case, fixed-parameter, large-$n$ calculation for a quantum heuristic, tied to classical algorithmic thresholds. It is not a clean complexity separation, but it is the kind of evidence that can sharpen conjectures about where QAOA might beat local classical heuristics.

---

## Limits / caveats

- **Small-angle theorem, larger-angle numerics.** The saddle-point proof requires sufficiently small $\gamma$. The method seems accurate near empirically good angles, but that is not fully proved.
- **$k$ must be a power of two for the main analytic formula.** The numerical methodology is broader, especially at $p=1$, but Proposition 3 is stated for random-$2^q$-SAT.
- **Fixed angles, average instance.** The angles are not retrained per instance. That is a strength for deployment, but it may miss hard-instance performance.
- **Average success probability is not median runtime.** At large $p$, the two estimates diverge. The average can be too optimistic if easy instances dominate.
- **Small-$n$ extrapolation.** The numerical comparison reaches $n\le 20$ for full simulations. Random 8-SAT near threshold has many clauses, so going much larger is expensive.
- **Classical baseline is empirical.** WalkSATlm is strong here, but “best tested” is not “best possible”. Survey propagation was not included for $k=8$ because their implementation was impractical in this regime.
- **Near-term hardware overhead ignored.** Runtime is measured in QAOA samples, not noisy circuit time, compilation depth, or error mitigation cost.

---

## Reusable ideas

1. [[Exact-Solution Objective for QAOA]] — optimise probability mass on the satisfying subspace rather than expected energy.

2. [[Poissonized Random-CSP Averaging]] — sample the number of clauses from a Poisson distribution so the random-clause average exponentiates cleanly.

3. [[Type-Count Expansion for QAOA Paths]] — group the $2p+1$ QAOA path strings coordinate-wise to reduce the average success probability to a multinomial sum over empirical types.

4. [[Saddle-Point Exponents for Generalized Multinomial Sums]] — estimate the large-$n$ scaling exponent of the QAOA success probability from a fixed point / saddle point.

5. [[Median TTS Benchmark for Quantum Heuristics]] — compare quantum samplers to classical solvers using median time-to-solution exponents, not only average success probability.

---

## References within this paper

- [[A Quantum Approximate Optimization Algorithm (Farhi-Goldstone-Gutmann 2014) — Paper Notes|Farhi-Goldstone-Gutmann (2014)]] — QAOA framework.
- Hogg (2000) — early quantum heuristic for random SAT, essentially QAOA with prescribed linear-schedule angles.
- [[Compilation of Fault-Tolerant Quantum Heuristics for Combinatorial Optimization (Sanders, Berry, Babbush et al 2020) — Paper Notes|Babbush et al. (2021)]] — warning that quadratic speedups need large enough structure to matter fault-tolerantly.
- Barak et al. (2015) — classical algorithm beating early QAOA claims for bounded-occurrence Max-E3Lin2.
- Basso-Gamarnik-Mei-Zhou (2022) — related generalized-multinomial / spin-glass QAOA analysis with different assumptions.
- Cai-Luo-Su (2014) — WalkSATlm, the strongest classical solver in their random 8-SAT tests.
- Mezard-Zecchina (2002) — survey propagation for random SAT; relevant classical competitor, though not practical in their $k=8$ implementation.
- Brandao et al. (2018) — fixed-angle QAOA concentration on typical instances.

---

## Cross-links

### Paper notes

- [[A Quantum Approximate Optimization Algorithm (Farhi-Goldstone-Gutmann 2014) — Paper Notes]]
- [[The Theory of Variational Hybrid Quantum-Classical Algorithms (McClean-Romero-Babbush-Aspuru-Guzik 2015) — Paper Notes]]
- [[Optimizing Quantum Optimization Algorithms via Faster Quantum Gradient Computation (Gilyén-Arunachalam-Wiebe 2019) — Paper Notes]]
- [[Compilation of Fault-Tolerant Quantum Heuristics for Combinatorial Optimization (Sanders, Berry, Babbush et al 2020) — Paper Notes]]
- [[Large Time-Step Discretisation of Adiabatic Quantum Dynamics (An-Costa-Berry 2025) — Paper Notes]]
- [[Entanglement-Induced Barren Plateaus (Ortiz Marrero-Kieferová-Wiebe 2021) — Paper Notes]]

### Trick cards

- [[Exact-Solution Objective for QAOA]]
- [[Poissonized Random-CSP Averaging]]
- [[Type-Count Expansion for QAOA Paths]]
- [[Saddle-Point Exponents for Generalized Multinomial Sums]]
- [[Median TTS Benchmark for Quantum Heuristics]]
