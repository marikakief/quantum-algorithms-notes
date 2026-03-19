> **Source:** Lov Grover and Terry Rudolph, *Creating superpositions that correspond to efficiently integrable probability distributions*, arXiv:quant-ph/0208112 (2002)
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/0208112)
> **Tags:** #state-preparation #amplitude-loading #log-concave #sampling #quantum-search

---

## The computational problem

**Input:** A probability distribution $p(x)$ over a continuous domain, discretised into $N = 2^n$ bins, such that the cumulative distribution function $\int_a^b p(x)\,dx$ can be efficiently computed classically (e.g., log-concave distributions: Gaussian, exponential, Poisson, etc.).

**Output:** A quantum state $|\psi\rangle = \sum_{i=0}^{N-1} \sqrt{p_i}\, |i\rangle$, where $p_i$ is the probability mass in bin $i$.

**Classical analogue:** Sampling from $p(x)$ is already efficient classically. The point is to prepare the distribution *coherently* — in superposition — so that subsequent quantum operations (amplitude estimation, Fourier transform, search) can exploit interference.

---

## What the paper does

A 2-page note that gives a clean, recursive procedure for loading a discretised probability distribution into quantum amplitudes. The key constraint: the CDF $\int_a^b p(x)\,dx$ must be efficiently computable. For log-concave distributions (which include most important statistical families), this is satisfied.

The paper doesn't introduce heavy machinery — the idea is simple and self-contained. But it's cited everywhere that quantum algorithms need to prepare non-uniform distributions, from [[Quantum Computational Finance — Monte Carlo Pricing of Financial Derivatives (Rebentrost-Gupt-Bromley 2018) — Paper Notes|quantum finance]] to [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|HHL]] state preparation discussions.

---

## The algorithm

### Recursive bisection

Start with a single qubit encoding the coarsest split of the distribution (left half vs. right half). Then refine, one qubit at a time.

Suppose we already have an $m$-qubit state:

$$|\psi_m\rangle = \sum_{i=0}^{2^m - 1} \sqrt{p_i^{(m)}}\, |i\rangle$$

where $p_i^{(m)}$ is the probability mass in region $i$ of the $2^m$-region discretisation. We want to add one more qubit to get the $2^{m+1}$-region discretisation.

For each region $i$, define $f(i) = \alpha_i / p_i^{(m)}$, where $\alpha_i$ is the probability in the left sub-half and $\beta_i = p_i^{(m)} - \alpha_i$ is the right sub-half. The evolution we need is:

$$\sqrt{p_i^{(m)}}\, |i\rangle \;\longrightarrow\; \sqrt{\alpha_i}\, |i\rangle|0\rangle + \sqrt{\beta_i}\, |i\rangle|1\rangle$$

This is a controlled rotation by angle $\theta_i = \arccos\sqrt{f(i)}$ on the new qubit, conditioned on register $|i\rangle$.

### Steps per refinement round

1. **Compute the rotation angle:** Use the efficient classical integration algorithm (run coherently on a quantum computer) to compute $\theta_i = \arccos\sqrt{f(i)}$ into an ancilla register:

$$\sqrt{p_i^{(m)}}\, |i\rangle|0\ldots 0\rangle \;\longrightarrow\; \sqrt{p_i^{(m)}}\, |i\rangle|\theta_i\rangle$$

2. **Controlled rotation:** Apply rotation by $\theta_i$ on the $(m+1)$-th qubit:

$$\sqrt{p_i^{(m)}}\, |i\rangle|\theta_i\rangle|0\rangle \;\longrightarrow\; \sqrt{p_i^{(m)}}\, |i\rangle|\theta_i\rangle(\cos\theta_i|0\rangle + \sin\theta_i|1\rangle)$$

3. **Uncompute** the ancilla $|\theta_i\rangle$.

The result is exactly the desired state $|\psi_{m+1}\rangle$ with $2^{m+1}$ regions. Repeat from $m=1$ to $m=n$.

### Handling probabilistic classical algorithms

If the classical integration uses randomness (e.g., Monte Carlo), replace the random bits with ancilla qubits in uniform superposition. The computation runs coherently, and the ancilla qubits are uncomputed after the rotation step. This ensures the procedure works for any efficiently integrable distribution, not just those with deterministic integration.

---

## Key results

- **Total cost:** $O(n)$ controlled rotations (one per qubit/refinement level), each requiring one call to the classical integration algorithm run coherently + the arcsine/rotation computation
- **Applicable to:** Any distribution where $\int_a^b p(x)\,dx$ is efficiently computable — including all log-concave distributions (Gaussian, exponential, gamma, Poisson, etc.)
- **Extends to:** Multivariate distributions by choosing a bisection ordering over dimensions

---

## Comparison with other state preparation methods

| Method | Assumption | Cost | Notes |
|---|---|---|---|
| **Grover-Rudolph (this paper)** | Efficient classical CDF | $O(n)$ rotations + integration cost | Structured distributions only |
| **Grover black-box (1997)** | Oracle access to coefficients | $O(\sqrt{N} \cdot \text{arcsine cost})$ | General but expensive |
| [[Black-Box Quantum State Preparation Without Arithmetic (Sanders-Low-Scherer-Berry 2019) — Paper Notes|Sanders-Low-Scherer-Berry (2019)]] | Oracle access to coefficients | $O(\sqrt{N} \cdot n)$ Toffolis | Arithmetic-free version of Grover |
| QROM lookup | Explicit coefficient table | $O(N)$ T gates | No structure needed, but linear in $N$ |

The key distinction: Grover-Rudolph exploits *structure* in the distribution (efficient integrability) to get $O(n) = O(\log N)$ depth. Black-box methods treat coefficients as opaque and pay $O(\sqrt{N})$.

---

## Why this matters

This is the standard reference for "quantum state preparation of structured distributions." It's cited as a subroutine in:

- **Quantum Monte Carlo / finance:** [[Quantum Computational Finance — Monte Carlo Pricing of Financial Derivatives (Rebentrost-Gupt-Bromley 2018) — Paper Notes|Rebentrost-Gupt-Bromley (2018)]] uses Grover-Rudolph to load the risk-neutral Gaussian into superposition for derivative pricing.
- **Quantum search with non-uniform priors:** The paper itself suggests this — if your prior over solutions is log-concave, load it and search with [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover's algorithm]] starting from a non-uniform initial state.
- **Amplitude estimation applications:** Load the distribution, encode the function of interest, estimate via [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|amplitude estimation]].

---

## Limits / caveats

- **Structure required.** The $O(\log N)$ cost holds *only* when the CDF is efficiently computable. For arbitrary distributions with no structure, you're back to black-box state preparation at $O(\sqrt{N})$.
- **Rotation angle computation.** Each refinement step requires computing $\arccos\sqrt{f(i)}$ coherently — this involves quantum arithmetic. [[Black-Box Quantum State Preparation Without Arithmetic (Sanders-Low-Scherer-Berry 2019) — Paper Notes|Sanders-Low-Scherer-Berry (2019)]] later addressed the arithmetic cost of similar rotations in the black-box setting.
- **Not a formal paper.** This is a 2-page note, never published in a journal. The idea is correct and widely used, but the analysis is brief.
- **Multivariate complexity.** For $d$-dimensional distributions, the bisection tree has $O(dn)$ levels. The cost per level depends on the integration algorithm. For high-dimensional log-concave distributions, the classical integration itself may dominate.
- **No speedup for mean estimation when integration is numerical.** Herbert (2021, [arXiv:2101.02240](https://arxiv.org/abs/2101.02240)) proved that using Grover-Rudolph state preparation for quantum Monte Carlo integration yields *no quantum advantage* for estimating the mean of a log-concave distribution, when the CDF integrals are computed numerically (i.e., by classical or quantum sampling). The state preparation error from the numerical integration propagates into the mean estimate, and suppressing it to $\hat{\epsilon}$ requires $\tilde{\Omega}(1/\hat{\epsilon}^2)$ total operations — the same as classical Monte Carlo. The speedup *does* survive if the CDF can be computed analytically (no sampling needed in state preparation), or if a sufficiently complex function is applied to the random variables before the expectation is taken.

---

## Applications mentioned in the paper

1. **Non-uniform priors for quantum search** — load a Gaussian or Poisson prior, then run Grover search. Focuses the search on high-probability regions.
2. **Sampling non-log-concave distributions** — prepare a log-concave state, then apply a unitary (e.g., Walsh-Hadamard) to transform it into a non-log-concave distribution via interference. The resulting distribution $q_j = \frac{1}{N}\left(\sum_i (-1)^{i \cdot j} \sqrt{p_i}\right)^2$ is generically not log-concave.
3. **Estimating Fourier components** — prepare the state, apply QFT, then use amplitude estimation for a quadratic speedup over classical sampling.

---

## Reusable ideas

1. **[[Recursive Bisection State Preparation for Integrable Distributions]]** — Load a discretised probability distribution into amplitudes by recursively bisecting the domain, one qubit at a time. Each step computes the conditional split ratio from the CDF and performs a controlled rotation. $O(\log N)$ depth for efficiently integrable distributions.

---

## References within this paper

- Aharonov-Kitaev-Nisan (1998, STOC) — random bits don't help quantum computers
- Aharonov-Ambainis-Kempe-Vazirani (2001, STOC) — quantum random walks
- Applegate-Kannan (1991, STOC) — log-concave volume computation
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover (1997)]] — quantum search
- Shor (1997) — quantum Fourier transform

---

## Cross-links

### Paper notes
- [[Black-Box Quantum State Preparation Without Arithmetic (Sanders-Low-Scherer-Berry 2019) — Paper Notes]] — improves on Grover's black-box state preparation (the oracle-based variant, not this structured-distribution variant)
- [[Quantum Computational Finance — Monte Carlo Pricing of Financial Derivatives (Rebentrost-Gupt-Bromley 2018) — Paper Notes]] — uses Grover-Rudolph for loading Gaussian distributions
- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes]] — state preparation as a subroutine
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes]] — amplitude estimation applied after state preparation
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]] — search with non-uniform priors

### Trick cards
- [[Recursive Bisection State Preparation for Integrable Distributions]] — the main trick
- [[Inequality-Test Amplitude Transduction]] — later alternative for black-box (unstructured) state preparation
- [[Comparator-Based Direct Amplitude Sampling from Fixed-Point Coefficients]] — another approach to the same problem
