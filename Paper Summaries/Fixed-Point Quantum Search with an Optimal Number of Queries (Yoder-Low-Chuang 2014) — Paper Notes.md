> **Source:** Theodore J. Yoder, Guang Hao Low, Isaac L. Chuang, *Fixed-Point Quantum Search with an Optimal Number of Queries*, arXiv:1409.3305, Physical Review Letters **113**, 210501 (2014)
> **Links:** [arXiv](https://arxiv.org/abs/1409.3305) · [Journal](https://doi.org/10.1103/PhysRevLett.113.210501)
> **Tags:** #amplitude-amplification #fixed-point #quantum-search #Chebyshev #QSP-precursor #optimal

---

## The computational problem

Given a unitary $A$ preparing state $|s\rangle = A|0\rangle^{\otimes n}$ and an oracle $U$ that recognises target states, amplify the target component $|T\rangle$ (with unknown overlap $\lambda = |\langle T|s\rangle|^2 > 0$) to success probability $\geq 1 - \delta^2$.

The challenge: [[Standard Amplitude Amplification|standard amplitude amplification]] (Grover/[[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|Brassard-Høyer-Mosca-Tapp]]) achieves $O(1/\sqrt{\lambda})$ queries but **oscillates** — if you run too many iterations (because $\lambda$ is larger than expected), the success probability drops back down (the "soufflé problem"). Fixed-point algorithms that avoid this previously required $O(1/\lambda)$ queries (Grover's $\pi/3$-algorithm), losing the quadratic speedup entirely.

---

## What the paper does

Provides the first fixed-point amplitude amplification that retains the optimal $O(1/\sqrt{\lambda})$ query complexity. The algorithm guarantees $P_L \geq 1 - \delta^2$ for all $\lambda \geq w$ simultaneously, where $w$ shrinks as the number of iterations $L$ grows. The query complexity is $L = O\!\left(\frac{\log(2/\delta)}{\sqrt{\lambda}}\right)$, optimal for any algorithm with this fixed-point property.

This is the paper that seeded the entire [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|quantum signal processing]] programme — the idea that by choosing **phases of generalised Grover reflections**, you can implement any bounded polynomial of the overlap, and that Chebyshev polynomials solve the optimal design problem.

---

## The algorithm / construction

### Setup

The 2D subspace $\mathcal{T}$ spanned by $|s\rangle$ and $|t\rangle$ (the normalised target) is the only relevant space. Define $\sin(\phi/2) = \sqrt{\lambda}$ and let $x = \cos(\phi/2) = \sqrt{1 - \lambda}$.

The **generalised Grover iterate** with phases $\alpha, \beta$:

$$G(\alpha, \beta) = -S_s(\alpha) S_t(\beta)$$

where $S_s(\alpha) = I - (1 - e^{-i\alpha})|s\rangle\langle s|$ and $S_t(\beta) = I - (1 - e^{i\beta})|t\rangle\langle t|$ are phase-modified reflections. Standard Grover uses $\alpha = \beta = \pi$.

On the Bloch sphere of $\mathcal{T}$, these are rotations: $S_s(\alpha) = e^{-i\alpha/2} R_\phi(\alpha)$ and $S_t(\beta) = e^{i\beta/2} R_0(\beta)$.

### The sequence

Apply $l$ generalised Grover iterates with different phases:

$$S_L = \prod_{j=1}^{l} G(\alpha_j, \beta_j), \qquad L = 2l + 1$$

The success probability is:

$$P_L = |\langle T|S_L|s\rangle|^2 = 1 - \delta^2 \left[\frac{T_L\!\left(T_{1/L}(1/\delta)\sqrt{1-\lambda}\right)}{1}\right]^2$$

where $T_L(x) = \cos(L\cos^{-1}x)$ is the $L$-th [[Chebyshev polynomial]] of the first kind.

### Explicit phase angles

For $j = 1, 2, \ldots, l$:

$$\alpha_j = -\beta_{l-j+1} = 2\cot^{-1}\!\left(\tan\!\left(\frac{2\pi j}{L}\right)\sqrt{1 - \gamma^2}\right)$$

where $\gamma^{-1} = T_{1/L}(1/\delta)$. These are **analytical closed-form expressions** — no numerical optimisation needed.

**Special cases:**
- $\delta = 1$: $\alpha_j = \pm\pi$, $\beta_j = \pm\pi$ — recovers standard Grover search exactly.
- $\delta = 0$: recovers Grover's $\pi/3$-algorithm (which is fixed-point but $O(1/\lambda)$ query complexity).

### Why it works: polynomial design

The key insight is that $P_L$ is a degree-$L$ polynomial in $x = \cos(\phi/2) = \sqrt{1-\lambda}$. The freedom to choose $2l$ phase angles $\{\alpha_j, \beta_j\}$ gives control over the polynomial coefficients. The Dolph-Chebyshev design (from antenna array theory!) provides the optimal polynomial: it achieves the widest possible interval $[0, w]$ where $P_L \geq 1 - \delta^2$ for a given degree $L$.

The proof reduces to showing that the recurrence relation for the amplitudes under the phase sequence reproduces the Chebyshev recurrence $T_L(x) = 2xT_{L-1}(x) - T_{L-2}(x)$ when $\gamma = \delta = 1$, and a generalised version for other $\gamma$.

### Nesting (concatenation)

Sequences can be **nested**: replace the state preparation $A$ in a sequence $S_{L_2}$ with $S_{L_1} A$ from a prior run. The Chebyshev semigroup property $T_p(T_q(x)) = T_{pq}(x)$ gives:

$$P_{L_2}(P_{L_1}(\lambda, \delta_1), \delta_2) = P_{L_1 L_2}(\lambda, \delta)$$

This means the algorithm is **adaptive**: you can run $S_{L_1}$, check if the result is good enough, and extend to $S_{L_1 L_2}$ without restarting from scratch.

---

## Key results

### Main theorem

For any $\delta \in [0,1]$ and odd $L \geq 1$, there exists a sequence of $L-1$ oracle queries that achieves:

$$P_L \geq 1 - \delta^2 \quad \text{for all } \lambda \geq w = 1 - T_{1/L}(1/\delta)^{-2}$$

The width satisfies $w \approx (\log(2/\delta)/L)^2$ for large $L$.

### Query complexity

To achieve $P_L \geq 1 - \delta^2$ for all $\lambda \geq \lambda_0$:

$$L = O\!\left(\frac{\log(2/\delta)}{\sqrt{\lambda_0}}\right)$$

This is **optimal**: any fixed-point search with error $\delta$ over range $[\lambda_0, 1]$ requires this many queries.

### Comparison

| Algorithm | Queries | Fixed-point? | Analytical phases? |
|---|---|---|---|
| [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes\|Grover (1996)]] | $O(1/\sqrt{\lambda})$ | No (oscillates) | — |
| [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes\|Brassard et al.]] + estimation | $O(1/\sqrt{\lambda} \cdot \log\log(1/\lambda))$ | Effectively | — |
| Grover $\pi/3$-algorithm (2005) | $O(1/\lambda)$ | Yes | Yes |
| Li-Li (2007), Toyama et al. (2009) | Numerical | Yes | No |
| **This paper** | $O(\log(1/\delta)/\sqrt{\lambda})$ | **Yes** | **Yes** |

---

## Comparison with prior work

- **Grover's $\pi/3$-algorithm** (PRL 2005): The first fixed-point search, but with $O(1/\lambda)$ queries. This paper shows $\pi/3$ is a special case — nest the $l=1, \delta=0$ version of this paper's algorithm with itself and you get exactly $\pi/3$.
- **Chakraborty-Radhakrishnan-Raghunathan** (2005): Proved $\pi/3$ is optimal among algorithms with *monotonically improving* error. This paper sidesteps the impossibility by relaxing to *bounded* error (not monotone) — the error is $\leq \delta^2$ for $\lambda \geq w$, but not necessarily monotonically decreasing as a function of $L$.
- **Low-Chuang QSP** (2016-2017): The polynomial-in-eigenvalue framework that grew directly from this paper. The generalised Grover iterate with phases is the prototype of the QSP signal processing operator.

---

## Limits / caveats

- Requires a lower bound $\lambda_0$ on the overlap. With no prior knowledge of $\lambda$, you need amplitude estimation first (but nesting provides adaptive refinement).
- The $\log(1/\delta)$ factor in query complexity is overhead compared to exact Grover when $\lambda$ is known.
- The algorithm works in the standard oracle model for amplitude amplification. For structured problems, oracle construction costs dominate.
- Phase angles have closed form (Eq. 11) but involve $\cot^{-1}$ and $T_{1/L}(1/\delta)$ — numerically stable for moderate $L$ but the Chebyshev evaluation $T_{1/L}(1/\delta)$ can overflow for very large $L$ and very small $\delta$ (use $\cosh$ form in practice).
- The polynomial PL is not monotone in $\lambda$ everywhere — it oscillates for $\lambda < w$. The fixed-point property is that $P_L \geq 1 - \delta^2$ for all $\lambda \geq w$, and $w$ shrinks as $L$ grows.

---

## Reusable ideas

1. [[Chebyshev Polynomial Design for Fixed-Point Amplitude Amplification]] — Choose phases $\{\alpha_j, \beta_j\}$ in generalised Grover iterates so the success probability equals a Dolph-Chebyshev function of the overlap. Achieves the broadest possible range of $\lambda$ values where $P \geq 1 - \delta^2$ for a given query budget.

2. [[Chebyshev Nesting (Semigroup Concatenation)]] — Exploit $T_p(T_q(x)) = T_{pq}(x)$ to concatenate fixed-point search sequences without restarting. A sequence of complexity $L_1$ can be extended to $L_1 L_2$ by nesting inside a second sequence of complexity $L_2$. Enables adaptive, restartless search.

---

## References within this paper

- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover (1996)]] — original quantum search
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|Brassard, Høyer, Mosca, Tapp (2000)]] — amplitude amplification framework
- [[Strengths and Weaknesses of Quantum Computing (Bennett-Bernstein-Brassard-Vazirani 1997) — Paper Notes|Bennett, Bernstein, Brassard, Vazirani (1997)]] — $\Omega(\sqrt{N/M})$ lower bound
- Grover (2005), PRL 95, 150501 — the $\pi/3$ fixed-point algorithm
- Chakraborty, Radhakrishnan, Raghunathan (2005) — optimality of $\pi/3$ under monotone error
- Beals, Buhrman, Cleve, Mosca, de Wolf (2001) — polynomial method for quantum query complexity
- Dolph (1946) — Dolph-Chebyshev antenna design; the signal processing origin of the optimal polynomial
- Long, Li, Zhang, Niu (1999) and Høyer (2000) — generalised phases for Grover's algorithm (precursors)
- [[Quantum Algorithm for the Collision Problem (Brassard-Høyer-Tapp 1997) — Paper Notes|Brassard, Høyer, Tapp (1997)]] — collision problem, application of amplitude amplification

---

## Cross-links

### Paper notes
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes]] — standard (non-fixed-point) amplitude amplification
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]] — original Grover search
- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes]] — QSP grew directly from this polynomial-design approach
- [[Resonant Equiangular Composite Gates (Low-Yoder-Chuang 2016) — Paper Notes]] — generalises the composite gate framework; same authors
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]] — QSVT unifies this and QSP into a single framework
- [[Near-Optimal Ground State Preparation (Lin-Tong 2020) — Paper Notes]] — eigenvalue filtering uses related polynomial design
- [[Recursive Amplitude Amplification (Høyer-Mosca-de Wolf)|Recursive Amplitude Amplification (Høyer-Mosca-de Wolf)]] — earlier recursive approach

### Trick cards
- [[Chebyshev Polynomial Design for Fixed-Point Amplitude Amplification]]
- [[Chebyshev Nesting (Semigroup Concatenation)]]
- [[Standard Amplitude Amplification]]
- [[Fixed-Point Amplitude Amplification with Eigenstate Filtering]]
- [[One-Ancilla Alternating Phase Sequence]]
- [[Oblivious Amplitude Amplification (Robust)]]
- [[SPAM-Tolerant Phase Estimation via Additive Error Bounds]] — Kimmel-Low-Yoder (2015) applied related polynomial design philosophy to SPAM-tolerant gate calibration
- [[Composite Sequence for Off-Resonance Error Amplification]] — from the same authors' gate calibration paper
