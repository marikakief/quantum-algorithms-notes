# Richardson Extrapolation over Randomized Step Sizes

**Source:** Watson, arXiv:2411.04240 (2025). Builds on multiproduct weights from LKW19 and the randomised multiproduct formula line (Faehrmann-Steudtner-Kueng-Kieferová-Eisert 2022).

---

## The trick

Run a randomised Hamiltonian simulation protocol (e.g., qDRIFT) at $m$ different step sizes $s_1 < s_2 < \cdots < s_m$, collecting $R$ shot estimates of an observable $O$ at each. Form the Richardson estimator:
$$\hat{A}_m = \sum_{k=1}^m b_k \bar{O}(s_k)$$
where $\bar{O}(s_k)$ is the sample mean of the observable at step size $s_k$.

The weights $b = (b_1, \ldots, b_m)$ solve the Vandermonde system:
$$\sum_{k=1}^m b_k = 1, \qquad \sum_{k=1}^m b_k s_k^j = 0 \quad \text{for } j = 1, \ldots, m-1$$

This forces the estimator's bias to vanish to order $m-1$ in $s$:
$$\mathbb{E}[\hat{A}_m] - \langle O \rangle_T = O(s_m^m)$$

If $s_m = O(1/N_{\min})$ where $N_{\min}$ is the largest circuit depth used, then the systematic error drops as $N_{\min}^{-m}$, which is exponentially small in $m$.

---

## Why it works

The trick relies on the randomised channel's expected observable having a clean power series expansion in the step size $s$:
$$\mathbb{E}_{s}[\langle O \rangle] = \langle O \rangle_{\text{exact}} + c_1 s + c_2 s^2 + \cdots$$
(see [[Channel Series Expansion via Variation-of-Parameters]]). The Vandermonde weights are precisely designed to zero out the $c_1, \ldots, c_{m-1}$ terms.

This is the incoherent/classical analogue of deterministic multiproduct formulas, which cancel error terms by taking coherent linear combinations of product formula circuits. Here the combination is classical: you're averaging measurement outcomes, not superposing quantum states.

---

## The conditioning issue and LKW19 fix

The Vandermonde system for general step sizes is numerically ill-conditioned for large $m$: the weights $b_k$ can grow exponentially, which would amplify the shot noise proportionally. Using $R$ shots at each of $m$ step sizes, the total statistical variance of $\hat{A}_m$ scales as:
$$\mathrm{Var}(\hat{A}_m) \sim \frac{\|b\|_1^2}{R}$$

For arbitrary step sizes, $\|b\|_1$ can grow as $m!$ or worse. The LKW19 construction (choosing step sizes in a specific geometric progression) achieves:
$$\|b\|_1 = O(\log m)$$

This is the key conditioning result. With $\|b\|_1 = O(\log m)$ and $R = O(1/\varepsilon^2)$ shots per point, the variance is $O(\log^2(m)/\varepsilon^2 \cdot 1/R) = O(\log^2(m))$ times smaller than $1/\varepsilon^2$, which is fine.

---

## Resource trade-off

| Quantity | Vanilla qDRIFT | qFLO (Richardson over qDRIFT) |
|---|---|---|
| Max circuit depth | $O((\lambda T)^2/\varepsilon)$ | $O((\lambda T)^2 \log(1/\varepsilon))$ |
| Shot cost per run | $O(1)$ | $O(1/\varepsilon^2)$ total |
| Ancilla qubits | 0 | 0 |
| Controlled gates | 0 | 0 |
| Protocol complexity | One circuit family | $m = O(\log(1/\varepsilon))$ circuit families |

The depth improvement is exponential in $1/\varepsilon$ (from $1/\varepsilon$ to $\log(1/\varepsilon)$). The price is an $O(1/\varepsilon^2)$ shot overhead, which is the standard cost of estimating $m$ expectations each to precision $O(\varepsilon/\|b\|_1)$ via sampling.

---

## When to use it

- You have a randomised simulation primitive whose expected channel error expands in powers of the step size.
- You're targeting observable estimation (not full state simulation).
- You want to avoid ancilla qubits and controlled operations.
- Moderate precision: the $(\lambda T)^2$ prefactor still bites for long times or large $\lambda$.

Not useful when:
- You need the full output state, not just expectation values.
- Shot cost is the bottleneck (e.g., limited hardware repetitions).
- $\lambda T$ is large: the depth prefactor $(\lambda T)^2$ dominates over the $\log(1/\varepsilon)$ gain.

---

## Generality

The trick is not specific to qDRIFT. Any randomised simulation protocol whose channel error expands as a power series in the step size admits this treatment. The analytical work is in establishing that the power series exists and bounding the coefficients; the extrapolation itself is purely classical.

---

## Links

- [[Channel Series Expansion via Variation-of-Parameters]] — the analytic engine that justifies the power series
- [[Randomly Compiled Quantum Simulation with Exponentially Reduced Circuit Depths (Watson 2025) — Paper Notes]] — source paper
- [[Term-Weighted Random Hamiltonian Sampling (qDRIFT)]] — the base protocol Watson applies this to
- [[Randomized Product Formulas for Hamiltonian Simulation (Quantum 2019-09-02-182) — Paper Notes]] — coherent multiproduct version of the same idea
- [[Permutation Averaging for Product-Formula Error Cancellation]] — related error cancellation by averaging
