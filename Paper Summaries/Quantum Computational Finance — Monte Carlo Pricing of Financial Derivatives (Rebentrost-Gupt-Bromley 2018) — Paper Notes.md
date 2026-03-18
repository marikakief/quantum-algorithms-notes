> **Source:** Patrick Rebentrost, Brajesh Gupt, and Thomas R. Bromley, *Quantum computational finance: Monte Carlo pricing of financial derivatives*, arXiv:1805.00109, Phys. Rev. A **98**, 022321 (2018)
> **Links:** [arXiv](https://arxiv.org/abs/1805.00109) · [PRA](https://doi.org/10.1103/PhysRevA.98.022321)
> **Tags:** #quantum-finance #monte-carlo #amplitude-estimation #option-pricing #black-scholes

---

## The computational problem

**Input:** A financial derivative with payoff function $f$ depending on the price trajectory of underlying assets governed by a stochastic model (e.g., Black-Scholes-Merton geometric Brownian motion).

**Output:** The risk-neutral price $\Pi = e^{-rT} \mathbb{E}_Q[f(S_T)]$, estimated to additive error $\epsilon$ with high probability.

**Classical cost:** $O(\lambda^2/\epsilon^2)$ Monte Carlo samples, where $\lambda^2 = \text{Var}_Q[f(S_T)]$.

**Quantum cost:** $\tilde{O}(\lambda/\epsilon)$ — quadratic speedup from [[Quantum Mean Estimation via Amplitude Estimation|quantum mean estimation]].

---

## What the paper does

Shows how to implement the full quantum Monte Carlo pipeline for derivative pricing: prepare the risk-neutral probability distribution in superposition (via [[Creating Superpositions That Correspond to Efficiently Integrable Probability Distributions (Grover-Rudolph 2002) — Paper Notes|Grover-Rudolph]] state preparation), encode the payoff function into a conditional rotation, and extract the price via [[Amplitude Estimation via Phase Estimation on Q|amplitude estimation]]. Works through two concrete examples: European call options and Asian options (path-dependent). Numerical simulations confirm the quadratic scaling advantage.

This is an application paper — the quantum algorithmic ingredients (amplitude estimation, Grover-Rudolph loading) are known. The contribution is showing how they compose in the financial pricing setting and working through the circuit-level details.

---

## Background: Black-Scholes-Merton model

The BSM model describes a risky asset (stock) with price dynamics under the risk-neutral measure $Q$:

$$dS_t = S_t r\, dt + S_t \sigma\, d\tilde{W}_t$$

where $r$ is the risk-free rate, $\sigma$ is volatility, and $\tilde{W}_t$ is a Brownian motion under $Q$. Solution:

$$S_T = S_0 \exp\left(\sigma \tilde{W}_T + (r - \sigma^2/2)T\right)$$

The risk-neutral price of a derivative with payoff $f(S_T)$ is:

$$\Pi = e^{-rT} \mathbb{E}_Q[f(S_T)]$$

For a European call with strike $K$: $f(S_T) = \max(S_T - K, 0)$.

---

## The quantum algorithm

### Step 1: Prepare the probability distribution

The Brownian motion $W_T \sim \mathcal{N}(0, T)$ is discretised on $2^n$ points in $[-x_{\max}, x_{\max}]$, where $x_{\max} = O(\sqrt{T})$ captures several standard deviations.

Prepare a quantum state encoding the Gaussian distribution:

$$G|0^n\rangle = \sum_{j=0}^{2^n - 1} \sqrt{p_T(x_j)} |j\rangle$$

where $p_T(x_j)$ is the discretised normal density. This uses the [[Creating Superpositions That Correspond to Efficiently Integrable Probability Distributions (Grover-Rudolph 2002) — Paper Notes|Grover-Rudolph]] algorithm, which efficiently loads log-concave distributions into amplitudes via a sequence of controlled rotations conditioned on the most-significant bits.

### Step 2: Encode the payoff function

Apply a controlled rotation $R$ on an ancilla qubit:

$$R|j\rangle|0\rangle = |j\rangle\left(\sqrt{1 - \tilde{v}(x_j)}|0\rangle + \sqrt{\tilde{v}(x_j)}|1\rangle\right)$$

where $\tilde{v}$ is a normalised, discretised version of the payoff function rescaled to $[0,1]$. For a call option, this involves:
1. Compute the stock price $S_T(x_j) = S_0 \exp(\sigma x_j + (r - \sigma^2/2)T)$ via quantum arithmetic
2. Compute $\max(S_T - K, 0)$ via a comparator
3. Normalise and rotate the ancilla

The combined operation $F = R(G \otimes I_2)$ yields:

$$F|0^{n+1}\rangle = \sum_j \sqrt{p_T(x_j)} |j\rangle \left(\sqrt{1-\tilde{v}(x_j)}|0\rangle + \sqrt{\tilde{v}(x_j)}|1\rangle\right) =: |\chi\rangle$$

Measuring the ancilla in $|1\rangle$ gives probability $\mu = \sum_j p_T(x_j) \tilde{v}(x_j) \approx \mathbb{E}_Q[v(W_T)]$.

### Step 3: Amplitude estimation

Apply [[Amplitude Estimation via Phase Estimation on Q|amplitude estimation]] to extract $\mu$ with precision $\epsilon$ using $O(1/\epsilon)$ applications of the Grover-like operator $Q = UVUV$, where:
- $U = I - 2|\chi\rangle\langle\chi| = FZF^\dagger$ with $Z = I - 2|0^{n+1}\rangle\langle 0^{n+1}|$
- $V = I - 2(I_{2^n} \otimes |1\rangle\langle 1|)$

Phase estimation on $Q$ resolves the angle $\theta$ such that $\mu = \sin^2(\theta/2)$.

### Total cost

$$\tilde{O}\left(\frac{\lambda}{\epsilon}\right)$$

applications of the distribution preparation + payoff circuit, where $\lambda^2 = \text{Var}_Q[f(S_T)] = O(\text{poly}(S_0, e^{rT}, e^{\sigma^2 T}, K))$.

This is a near-quadratic improvement over the classical $O(\lambda^2/\epsilon^2)$.

---

## Asian option extension

For path-dependent (Asian) options, the payoff depends on the average stock price over $L$ time steps:

$$f = \max\left(\frac{1}{L}\sum_{\ell=1}^L S_{t_\ell} - K, 0\right)$$

The algorithm:
1. Prepare $L$ independent Gaussian states (one per time step) as a product state
2. Sequentially compute stock prices $S_{t_1}, \ldots, S_{t_L}$ using the recurrence $\log S_{t_{\ell+1}} = \log S_{t_\ell} + \sigma x_{j_\ell} + (r - \sigma^2/2)\Delta t$
3. Compute the running average in an auxiliary register
4. Apply the payoff rotation and amplitude estimation as before

The variance bound and quadratic speedup carry over.

---

## Key results

| Quantity | Classical MC | Quantum MC |
|---|---|---|
| European call price estimation (additive $\epsilon$) | $O(\lambda^2/\epsilon^2)$ samples | $\tilde{O}(\lambda/\epsilon)$ applications of $F$ |
| Asian option pricing | $O(\lambda^2/\epsilon^2)$ samples | $\tilde{O}(\lambda/\epsilon)$ applications of $F$ |

The numerical simulations (Fig. 3) confirm the scaling exponent: classical error $\sim k^{-0.5}$, quantum error $\sim k^{-0.98}$ (close to theoretical $k^{-1}$).

---

## Limits / caveats

- **State preparation cost.** The Grover-Rudolph circuit for loading Gaussians requires $O(n)$ controlled rotations (one per qubit), where $n$ is the discretisation precision. Each rotation angle must be classically precomputed. For more complex distributions (non-log-concave, multivariate), state preparation becomes the bottleneck.
- **Arithmetic overhead.** Computing $e^x$, $\max$, and running averages in reversible quantum arithmetic is expensive — many ancilla qubits and gates. The paper doesn't give explicit gate counts; the $\tilde{O}(\lambda/\epsilon)$ hides substantial circuit depth.
- **No exponential speedup.** This is a quadratic improvement, same as Grover. For the speedup to matter practically, $\lambda/\epsilon$ must be large enough that the constant-factor overhead of fault-tolerant quantum operations is compensated. Current estimates suggest millions of logical operations per amplitude estimation step.
- **The Aaronson caveat applies.** As noted in [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|HHL's fine print section]], quadratic quantum speedups for Monte Carlo face the question of whether the problem, once sufficiently structured for quantum implementation, admits faster classical approaches (e.g., quasi-Monte Carlo, importance sampling, multilevel MC).
- **Bounded-variance assumption.** The $\tilde{O}(\lambda/\epsilon)$ cost requires $\lambda$ to be known or bounded. For exotic derivatives with fat-tailed payoffs, the variance bound may be poor.
- **Single underlying asset.** The BSM model here uses a single stock. Multi-asset derivatives would require multivariate Gaussian state preparation and higher-dimensional quantum arithmetic — the paper mentions this as future work.
- **Herbert's no-go for Grover-Rudolph + QMC.** Herbert (2021, [arXiv:2101.02240](https://arxiv.org/abs/2101.02240)) showed that using [[Creating Superpositions That Correspond to Efficiently Integrable Probability Distributions (Grover-Rudolph 2002) — Paper Notes|Grover-Rudolph]] state preparation for quantum Monte Carlo to estimate the mean of a log-concave distribution yields no quantum advantage when the CDF integrals are computed numerically. The state preparation error (from numerical integration of the split ratios) propagates linearly into the mean estimate, requiring $\tilde{\Omega}(1/\hat{\epsilon}^2)$ total operations — matching classical MC. The quadratic speedup survives only if (a) the CDF is analytically computable (no sampling in state prep), or (b) a sufficiently complex payoff function is applied before the expectation. For the BSM Gaussian case in this paper, the CDF *is* analytically available (error function), so the speedup may survive — but Herbert's result means it cannot be taken for granted, and any implementation using numerical integration in the state preparation circuit loses the advantage.

---

## Reusable ideas

1. **[[Recursive Bisection State Preparation for Integrable Distributions]]** — Load a discretised log-concave probability distribution (e.g., Gaussian) into quantum amplitudes via a cascade of controlled rotations. Each rotation conditions on the most significant bits already prepared. Works for any efficiently integrable distribution.

2. **[[Payoff Encoding via Ancilla Rotation]]** — Encode a real-valued function $v(x) \in [0,1]$ into the amplitude of an ancilla qubit: $R|x\rangle|0\rangle = |x\rangle(\sqrt{1-v(x)}|0\rangle + \sqrt{v(x)}|1\rangle)$. The probability of measuring $|1\rangle$ then equals $\mathbb{E}[v]$. Standard technique for connecting function evaluation to amplitude estimation.

---

## References within this paper

- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|Brassard-Høyer-Mosca-Tapp (2002)]] — amplitude estimation (core subroutine)
- [[Quantum Speedup of Monte Carlo Methods (Montanaro 2015) — Paper Notes|Montanaro (2015)]] — general quantum Monte Carlo speedup; this paper specialises Montanaro's Theorem 3 to finance
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover (1996)]] — quadratic search speedup; amplitude estimation generalises Grover
- [[Creating Superpositions That Correspond to Efficiently Integrable Probability Distributions (Grover-Rudolph 2002) — Paper Notes|Grover & Rudolph (2002)]] — state preparation for log-concave distributions
- Black & Scholes (1973), Merton (1973) — the BSM model
- Shreve (2004), Hull (2012) — standard references on stochastic calculus for finance
- Woerner & Egger (2019, 1805.00020) — concurrent independent work on quantum risk analysis, similar approach
- Stamatopoulos et al. (2020, 1905.02666) — subsequent IBM implementation of quantum option pricing

---

## Cross-links

### Paper notes
- [[Quantum Speedup of Monte Carlo Methods (Montanaro 2015) — Paper Notes]] — the general framework this paper instantiates
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes]] — amplitude estimation
- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes]] — the "fine print" caveats about quantum speedups apply here too
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]] — Grover search as the foundation

### Trick cards
- [[Quantum Mean Estimation via Amplitude Estimation]] — the abstract version of what this paper applies
- [[Amplitude Estimation via Phase Estimation on Q]] — the core amplitude estimation primitive
- [[Creating Superpositions That Correspond to Efficiently Integrable Probability Distributions (Grover-Rudolph 2002) — Paper Notes]] — Grover-Rudolph state preparation used here
- [[Recursive Bisection State Preparation for Integrable Distributions]] — the trick card for the Grover-Rudolph method
- [[Payoff Encoding via Ancilla Rotation]]
