> **Source:** Dominic W. Berry, Andrew M. Childs, Robin Kothari, *Hamiltonian simulation with nearly optimal dependence on all parameters*, Proc. 56th IEEE FOCS, pp. 792–809 (2015). arXiv:1501.01715
> **Links:** [arXiv](https://arxiv.org/abs/1501.01715) · [FOCS](https://doi.org/10.1109/FOCS.2015.54)
> **Tags:** #hamiltonian-simulation #quantum-walk #bessel-functions #LCU #lower-bounds

---

## What the paper does

Combines the quantum walk approach (optimal in sparsity $d$, poor in precision $\varepsilon$) with the fractional-query/LCU approach (optimal in $\varepsilon$, poor in $d$) to get both:

$$O\!\left(\tau \cdot \frac{\log(\tau/\varepsilon)}{\log\log(\tau/\varepsilon)}\right) \text{ queries}, \qquad \tau = d\|H\|_{\max}t$$

Note: $\tau = d\|H\|_{\max}t$, not $d^2\|H\|_{\max}t$ as in the earlier BCCKS papers. The extra factor of $d$ is removed by using the Szegedy walk (which costs $O(1)$ queries per step rather than $O(d)$ from the self-inverse decomposition).

The paper also proves a new $\Omega(\tau)$ lower bound, showing the linear dependence on $d\|H\|_{\max}t$ is necessary.

---

## The key idea: Bessel linearisation of the walk

### The problem

The Szegedy quantum walk operator $U$ for a $d$-sparse Hamiltonian $H$ has eigenvalues $\mu_\pm = \pm e^{\pm i \arcsin(\lambda/Xd)}$ corresponding to eigenvalue $\lambda$ of $H$. For Hamiltonian simulation, you want the phase $e^{-i\lambda t}$, but you get $e^{\pm i\arcsin(\lambda/Xd)}$ — a nonlinear function.

Previous approaches [[Black-Box Hamiltonian Simulation and Unitary Implementation (Berry-Childs 2011) — Paper Notes|(Berry-Childs 2012)]] used phase estimation to correct this, giving $O(1/\sqrt{\varepsilon})$ precision dependence. The [[Exponential Improvement in Precision for Simulating Sparse Hamiltonians (Berry-Childs-Cleve-Kothari-Somma 2014) — Paper Notes|BCCKS STOC 2014]] approach decomposed $H$ into self-inverse terms (costing an extra factor of $d$) to avoid the walk entirely.

### The solution: Bessel function LCU

Apply a linear combination of walk steps:

$$V_k = \sum_{m=-k}^{k} a_m U^m$$

Choose coefficients from the Bessel function generating identity:

$$e^{i\nu z} = \sum_{m=-\infty}^{\infty} J_m(z)\, \mu_\pm^m, \qquad \nu = \lambda/Xd$$

This works because $\mu_\pm = \pm e^{\pm i\arcsin\nu}$ and $\nu = -\frac{i}{2}(\mu_\pm - 1/\mu_\pm)$, so the generating function for Bessel functions of the first kind directly gives $e^{i\nu z}$.

**Two key properties:**

1. The expression is the **same** for both $\mu_+$ and $\mu_-$ — no need to distinguish eigenspaces (unlike phase estimation approaches)
2. Bessel functions decay super-exponentially: $|J_m(z)| \sim |z/2|^m / m!$ for large $m$, so truncating at $k = O(\log(1/\varepsilon)/\log\log(1/\varepsilon))$ suffices

Take $z = -1/2$ (constant), so each segment simulates time $1/(2Xd)$. The number of segments is $O(\tau)$, each requiring $O(k)$ walk steps, giving total complexity $O(\tau k)$.

---

## Algorithm structure

1. **Walk operator** $U$ from the Szegedy construction. One walk step = $O(1)$ oracle queries (isometry $T$, swap $S$, reflection)
2. **LCU of walk steps**: $V_k = \sum_{m=-k}^{k} a_m U^m$ with $a_m = J_m(z) / \sum_j J_j(z)$
3. **Implement via LCU Lemma** (Lemma 4): PREPARE the Bessel coefficients, SELECT via $\text{select}(\vec{U})$
4. **[[Oblivious Amplitude Amplification (Robust)|Robust OAA]]** to make each segment deterministic
5. **Compose segments**: $O(\tau)$ segments, each with $O(k)$ walk steps

---

## Key results

| Parameter | Scaling |
|---|---|
| **Queries** | $O\!\left(\tau\,\frac{\log(\tau/\varepsilon)}{\log\log(\tau/\varepsilon)}\right)$, $\tau = d\|H\|_{\max}t$ |
| **Gates** | $O\!\left(\tau[n + \log^{5/2}(\tau/\varepsilon)]\,\frac{\log(\tau/\varepsilon)}{\log\log(\tau/\varepsilon)}\right)$ |
| **Lower bound** (Theorem 2) | $\Omega\!\left(\tau + \frac{\log(1/\varepsilon)}{\log\log(1/\varepsilon)}\right)$ |
| **Tradeoff** (Theorem 3) | $O(\tau^{1+\alpha/2} + \tau^{1-\alpha/2}\log(1/\varepsilon))$ for $\alpha \in (0,1]$ |

### Improvement over prior work

| Method | $\tau$ parameter | Precision |
|---|---|---|
| Walk (Berry-Childs 2012) | $d\|H\|_{\max}t$ (linear in $d$) | $O(1/\sqrt{\varepsilon})$ |
| BCCKS 2014/2015 (fractional/Taylor) | $d^2\|H\|_{\max}t$ (quadratic in $d$) | $\widetilde{O}(\log(1/\varepsilon))$ |
| **This paper** | $d\|H\|_{\max}t$ (linear in $d$) | $\widetilde{O}(\log(1/\varepsilon))$ |

---

## The lower bound (Theorem 2)

Two-part construction:

1. **$\Omega(\tau)$ bound (Lemma 12):** Take the parity Hamiltonian from BCCKS 2014 and replace each edge with a complete bipartite graph $K_{d,d}$. This boosts the effective Hamiltonian by factor $d$ while making the matrix $2d$-sparse. The resulting walk in the invariant subspace computes parity, requiring $\Omega(N) = \Omega(td)$ queries.

2. **$\Omega(\log(1/\varepsilon)/\log\log(1/\varepsilon))$ bound:** From [[Exponential Improvement in Precision for Simulating Sparse Hamiltonians (Berry-Childs-Cleve-Kothari-Somma 2014) — Paper Notes|BCCKS 2014]], via parity at $N$th order in Taylor series.

Combined: $\Omega(\tau + \log(1/\varepsilon)/\log\log(1/\varepsilon))$. The gap between this (sum) and the upper bound (product) remains open.

---

## The $\tau$-$\varepsilon$ tradeoff (Theorem 3)

Taking $z = -\tau^\alpha$ instead of constant:

- Each segment simulates time $\tau^\alpha / Xd$, requiring $k = O(\tau^\alpha + \log(1/\varepsilon))$ walk steps
- Number of segments: $O(\tau^{1-\alpha})$  
- Bessel coefficient sum: $\sum |a_m| = O(\sqrt{|z|}) = O(\tau^{\alpha/2})$, giving $O(\tau^{\alpha/2})$ rounds of OAA
- Total: $O(k \cdot \tau^{\alpha/2} \cdot \tau^{1-\alpha}) = O(\tau^{1+\alpha/2} + \tau^{1-\alpha/2}\log(1/\varepsilon))$

For $\alpha = 1$: single segment, $O(\tau^{3/2} + \log(1/\varepsilon)\sqrt{\tau})$ — better when $\varepsilon$ is extremely small.

---

## Multi-step robust OAA (Lemma 6)

This paper provides the full multi-step version of robust OAA, generalising the single-step result from the [[Simulating Hamiltonian Dynamics with a Truncated Taylor Series (Berry-Childs-Cleve-Kothari-Somma 2015) — Paper Notes|truncated Taylor paper]]. The proof uses Chebyshev polynomials to show that after $\ell$ rounds:

$$PR^\ell WP = Z \frac{(-1)^\ell T_{2\ell+1}(\sqrt{Z^\dagger Z})}{\sqrt{Z^\dagger Z}}$$

where $Z = PWP = \frac{1}{s}(|\varsigma\rangle\langle\varsigma| \otimes \tilde{V})$. The connection to Chebyshev polynomials makes the link to [[QSVT Meta-Template|QSVT]] explicit — OAA is polynomial transformation of singular values.

---

## Reusable ideas

1. **[[Bessel Linearisation of Quantum Walk Phases]]** — uses the Bessel generating function to convert $e^{\pm i\arcsin(\nu)}$ walk phases into the desired $e^{i\nu z}$ Hamiltonian phases. This avoids both phase estimation and self-inverse decomposition.

2. **Multi-step robust OAA** — generalisation of single-step OAA with explicit Chebyshev structure. Already captured in [[Oblivious Amplitude Amplification (Robust)]].

3. **$K_{d,d}$ graph blowup for lower bounds** — replace each edge with a complete bipartite graph to boost the Hamiltonian strength by $d$ at the cost of $d$-fold increase in sparsity. Clean technique for proving $\Omega(d)$ lower bounds.

---

## Limits / caveats

- **Still not strictly optimal.** The $\log\log(\tau/\varepsilon)$ denominator persists. [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|Low-Chuang (2017)]] achieved $O(\tau + \log(1/\varepsilon)\log\log(1/\varepsilon))$ via QSP — additive rather than multiplicative, and with better log factors.
- **Upper-lower gap.** Upper bound has $\tau \cdot \log(\tau/\varepsilon)$ (product); lower bound has $\tau + \log(1/\varepsilon)$ (sum). Still open whether the tight answer is the product or the sum.
- **Gate complexity.** The $\log^{5/2}$ factor from classical elementary function evaluation is not pretty. Improved classical arithmetic helps but only for extremely high precision.
- **Superseded in practice** by QSP-based methods, which achieve better constants and don't need the walk construction.

---

## Why this matters

This paper completed the quest for near-optimal Hamiltonian simulation in all parameters simultaneously. The BCS 2013 / BCCKS 2014 line had optimal $\varepsilon$-dependence but quadratic $d$-dependence. The walk approach had linear $d$ but poor $\varepsilon$. This paper married the two via a beautiful application of Bessel functions.

The Bessel-LCU approach also connects to the broader principle that LCU can implement *any* desired function of a walk operator's eigenvalues — the generating function just provides the right coefficients. [[QSVT Meta-Template|QSVT]] later formalised this principle completely, but the Bessel construction here was an early and striking example.

---

## References within this paper

- [[Universal Quantum Simulators (Lloyd 1996) — Paper Notes|Lloyd (1996)]] [Ref 24]
- [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes|BACS (2007)]] [Ref 5]
- [[Black-Box Hamiltonian Simulation and Unitary Implementation (Berry-Childs 2011) — Paper Notes|Berry-Childs (2012)]] [Refs 6, 12]
- [[Exponential Improvement in Precision for Simulating Sparse Hamiltonians (Berry-Childs-Cleve-Kothari-Somma 2014) — Paper Notes|BCCKS STOC 2014]] [Ref 7]
- [[Simulating Hamiltonian Dynamics with a Truncated Taylor Series (Berry-Childs-Cleve-Kothari-Somma 2015) — Paper Notes|BCCKS truncated Taylor (2015)]] [Ref 8]
- [[Limitations on Simulation of Non-Sparse Hamiltonians (Childs-Kothari-Somma 2010) — Paper Notes|Childs-Kothari-Somma (2010)]] [Ref 15]
- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|HHL (2009)]] [Ref 21]

---

## Cross-links

### Paper notes
- [[Exponential Improvement in Precision for Hamiltonian-Evolution Simulation (Berry-Cleve-Somma 2013) — Paper Notes]] — first polylog-precision simulation
- [[Exponential Improvement in Precision for Simulating Sparse Hamiltonians (Berry-Childs-Cleve-Kothari-Somma 2014) — Paper Notes]] — introduced OAA; quadratic $d$ dependence
- [[Simulating Hamiltonian Dynamics with a Truncated Taylor Series (Berry-Childs-Cleve-Kothari-Somma 2015) — Paper Notes]] — simplified LCU approach, same precision
- [[Black-Box Hamiltonian Simulation and Unitary Implementation (Berry-Childs 2011) — Paper Notes]] — walk-based simulation this builds on
- [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes]] — product-formula sparse simulation
- [[LCU Origins (Childs-Wiebe 2012) — Paper Notes]] — LCU primitive
- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes]] — strictly optimal via QSP; supersedes this
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]] — reformulated via block-encoding
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]] — generalised the polynomial framework
- [[Limitations on Simulation of Non-Sparse Hamiltonians (Childs-Kothari-Somma 2010) — Paper Notes]] — related lower bound techniques

### Trick cards
- [[Oblivious Amplitude Amplification (Robust)]] — multi-step version proved here
- [[Fractional-Query to Discrete-Query Reduction]] — conceptual ancestor
- [[Linear Combination of Unitaries (LCU)]] — the PREPARE/SELECT framework used here
