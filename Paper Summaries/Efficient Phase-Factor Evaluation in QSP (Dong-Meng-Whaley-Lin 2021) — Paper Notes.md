> **Source:** Yulong Dong, Xiang Meng, K. Birgitta Whaley, and Lin Lin, *Efficient phase-factor evaluation in quantum signal processing*, arXiv:2002.11649, Phys. Rev. A **103**, 042419 (2021)
> **Links:** [arXiv](https://arxiv.org/abs/2002.11649) · [PRA](https://doi.org/10.1103/PhysRevA.103.042419)
> **Tags:** #QSP #phase-factors #optimization #numerical-methods #QSPPACK #Hamiltonian-simulation #QLSP #eigenstate-filtering

---

## The computational problem

Given a target polynomial $f(x)$ of degree $d$ satisfying the [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|QSP]] feasibility conditions ($|f(x)| \leq 1$ on $[-1,1]$, definite parity), find phase factors $\Phi = (\phi_0, \ldots, \phi_d)$ such that

$$f(x) = \operatorname{Re}\bigl[\langle 0 | U_\Phi(x) | 0 \rangle\bigr],$$

where $U_\Phi(x) = e^{i\phi_0 \sigma_z} \prod_{j=1}^{d} W(x) e^{i\phi_j \sigma_z}$ and $W(x) = e^{i \arccos(x) \sigma_x}$.

This is the classical preprocessing bottleneck for all [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|QSVT]]-family algorithms. Prior methods (the [[Recursive Phase Extraction from Polynomial Coefficients|recursive extraction]] approach of GSLW and the Prony-based method of Haah) require variable-precision arithmetic and become numerically unstable for $d \gtrsim$ a few hundred.

## What the paper does

Introduces an **optimisation-based** approach to computing QSP phase factors that works in standard double-precision (64-bit) floating-point arithmetic for polynomials of degree $d > 10{,}000$ with error below $10^{-12}$.

The method uses L-BFGS on a Chebyshev-node loss function, with a smart initial guess derived from a perturbative expansion (Theorem 5). It also exploits [[Symmetry Reduction for Phase Factor Uniqueness|symmetric phase factors]] to halve the search space and regularise the problem.

This is the paper that introduced **QSPPACK** — the software package that made QSP/QSVT practically compilable for the first time at scale. Before this, the theory said "optimal algorithms exist" but nobody could actually compute the circuits for interesting degrees. After this paper, you could.

**My assessment:** This is one of the most practically important papers in the QSP/QSVT ecosystem. The theoretical novelty is moderate — it's an optimisation paper, not a complexity paper — but it solved a real bottleneck. The perturbative initial guess (Theorem 5) and the Chebyshev-node loss function (Theorem 4) are the key ideas. The later [[Energy Landscape of Symmetric QSP (Wang-Dong-Lin 2022) — Paper Notes|Wang-Dong-Lin (2022)]] paper explains *why* this works (the optimisation landscape is well-behaved near the maximal solution).

## The algorithm / construction

### The optimisation problem

Restrict to **symmetric** phase factors: $\Phi = (\phi_0, \phi_1, \ldots, \phi_1, \phi_0) \in [-\pi,\pi)^{e_d}$ where $e_d = \lceil(d+1)/2\rceil$. Define the loss function:

$$L(\Phi) = \frac{1}{\tilde{d}} \sum_{k=1}^{\tilde{d}} \bigl|g(x_k, \Phi) - f(x_k)\bigr|^2, \quad \tilde{d} = \lceil d/2 \rceil,$$

where $x_k$ are the positive roots of $T_{2\tilde{d}}(x)$ (Chebyshev nodes on $[0,1]$) and $g(x,\Phi) = \operatorname{Re}[\langle 0|U_\Phi(x)|0\rangle]$.

Symmetry halves the variables from $d+1$ to $e_d$, which matches the number of sampling constraints. This is why [[Symmetry Reduction for Phase Factor Uniqueness|symmetry matters]] — without it the Hessian is singular at every global minimum.

### Chebyshev coefficient control (Theorem 4)

If $L(\Phi) \leq \varepsilon$, then the Chebyshev coefficients of $g(\cdot, \Phi)$ and $f$ agree to within $2\sqrt{\varepsilon}$ in max-norm. So minimising $L$ at $\tilde{d}$ Chebyshev nodes is sufficient to control the full polynomial approximation.

This is cleaner than it looks: the proof uses discrete orthogonality of Chebyshev polynomials on their own roots. The Chebyshev-node choice isn't accidental — it's the optimal sampling set for trigonometric interpolation after the variable change $x = \cos\theta$.

### Initial guess via perturbative expansion (Theorem 5)

When $\|f\|_\infty \ll 1$, the QSP phases are close to $\Phi_0 = (\pi/4, 0, 0, \ldots, 0, \pi/4)$. Theorem 5 gives an explicit first-order correction:

$$\tilde{\phi}_k \approx \frac{1}{2}\hat{f}_k, \quad k = 0, \ldots, e_d - 1,$$

where $\hat{f}_k$ are the Chebyshev coefficients of $f$ and $\tilde{\phi}_k = \phi_k - \frac{\pi}{4}(\delta_{k,0} + \delta_{k,d})$. This is the "small-angle" regime where QSP phases are approximately half the Chebyshev coefficients of the target.

For $\|f\|_\infty$ close to 1, a **scaling-and-squaring** strategy is used: solve for $f_\delta(x) = \delta f(x)$ with small $\delta$, then iteratively double via the identity $T_2(y) = 2y^2 - 1$ (complementary polynomials get doubled in a controlled way using QSP composition).

### The full algorithm (Algorithm 1 in the paper)

1. **If** $\|f\|_\infty > \delta_0$ (a threshold, typically $\sim 0.9$):
   - Recursively find phases for $f_\delta = \delta f$ with smaller $\delta$
   - Double via composition until target norm is reached
2. **Else:**
   - Compute Chebyshev coefficients $\hat{f}_k$
   - Set initial guess $\tilde{\phi}_k = \hat{f}_k/2$ (Theorem 5)
   - Run L-BFGS on $L(\Phi)$

### Gradient computation (Algorithm 2)

The gradient $\nabla_\Phi L$ is computed by automatic differentiation through the QSP circuit. Each evaluation of $U_\Phi(x_k)$ is a product of $d$ SU(2) matrices, so the gradient costs $O(d \cdot \tilde{d})$ — the same order as a single loss evaluation times the number of sample points.

### Phase-shift lemma (Lemma 3)

A useful identity: shifting all phases by $\pi/4$ in $\sigma_z$ swaps $\operatorname{Re}$ and $\operatorname{Im}$ of the $(0,0)$ matrix element. This means the same optimisation machinery handles both even and odd target polynomials — you just apply a global phase shift.

## Key results

### Numerical performance

| Application | Degree $d$ | Target error | Achieved $L(\Phi)$ | Wall time |
|---|---|---|---|---|
| Hamiltonian simulation ($e^{-iHt}$, Jacobi-Anger) | 10,000 | $10^{-12}$ | $< 10^{-25}$ | ~minutes |
| Eigenvalue filtering (rectangular window) | 4,000 | $10^{-12}$ | $< 10^{-25}$ | ~minutes |
| Matrix inversion ($1/x$ via QLSP) | 10,000 | $10^{-12}$ | $< 10^{-25}$ | ~minutes |

All in standard 64-bit floating point. For comparison, prior methods (GSLW recursive extraction, Haah's Prony method) required hundreds of bits of extended precision and were limited to $d \lesssim 200$.

### Comparison with prior methods

| Method | Precision required | Max degree demonstrated | Stable? |
|---|---|---|---|
| GSLW recursive extraction | Variable-precision ($O(d)$ bits) | $d \sim 200$ | Breaks for large $d$ |
| Haah (2019) Prony-based | Variable-precision | $d \sim 500$ | Better but still limited |
| **This paper** (L-BFGS) | Standard double precision | $d > 10{,}000$ | Yes |

The GSLW method is the [[Recursive Phase Extraction from Polynomial Coefficients|recursive peeling]] approach: extract one phase at a time by dividing out leading coefficients. It's exact in exact arithmetic, but each step amplifies rounding errors — the condition number grows exponentially with $d$.

Haah's method uses Prony's algorithm on the complementary polynomial. Better conditioned but still needs extended precision.

### Why optimisation works here

The paper doesn't prove convergence guarantees (that comes later in [[Energy Landscape of Symmetric QSP (Wang-Dong-Lin 2022) — Paper Notes|Wang-Dong-Lin (2022)]]). But empirically:

- The initial guess from Theorem 5 lands in the basin of attraction of the [[Maximal Solution Selection via Mahler Measure|maximal solution]]
- Symmetric phases eliminate the continuous family of global minima that plague the unconstrained problem
- L-BFGS converges in $O(1)$ iterations for the scaled-down problem; the doubling steps each converge quickly too

## Applications demonstrated

### 1. Hamiltonian simulation

Target: $f(x) = \operatorname{Re}[e^{-itx}]$ or $\operatorname{Im}[e^{-itx}]$ (Jacobi-Anger expansion in Chebyshev polynomials). Degree $d = O(t + \log(1/\varepsilon))$.

Phase factors for degree 10,000 computed successfully — this means you can in principle compile QSP-based simulation circuits for $t \sim 10^4$ evolution time at double-precision cost.

### 2. Eigenvalue filtering

Target: a rectangular-ish function that's $\sim 1$ in a window $[\lambda_* - \Delta, \lambda_* + \Delta]$ and $\sim 0$ outside. Used for [[Optimal Polynomial Based Quantum Eigenstate Filtering (Dong-An-Lin 2020) — Paper Notes|eigenstate filtering]] in ground-state preparation. The polynomial approximation uses Chebyshev expansion of a smoothed step function.

### 3. Quantum linear systems

Target: $f(x) = 1/(\kappa x)$ on $[-1, -1/\kappa] \cup [1/\kappa, 1]$ (odd function). Polynomial degree $d = O(\kappa \log(\kappa/\varepsilon))$.

## Limits / caveats

1. **No convergence proof.** The optimisation is empirically reliable but no theoretical guarantee of convergence to a global minimum is given here. The [[Energy Landscape of Symmetric QSP (Wang-Dong-Lin 2022) — Paper Notes|energy landscape paper]] partially addresses this, but only for $\|f\|_\infty = O(d^{-1})$.

2. **Scaling-and-squaring adds overhead.** When $\|f\|_\infty$ is close to 1, the doubling procedure requires multiple optimisation solves. Each is fast, but the total work is $O(\log(1/\delta_0))$ optimisation runs.

3. **Standard precision has limits.** For $d \gg 10{,}000$ or target errors below $10^{-15}$, double precision may not suffice. The method doesn't eliminate the precision issue — it pushes the boundary much further.

4. **Complementary polynomial not computed explicitly.** The optimisation finds phases directly without constructing the complementary polynomial pair $(P_{\mathrm{Im}}, Q)$. This is a feature for compilation but means you don't get the full QSP decomposition "for free" — you'd need to extract it separately if needed.

5. **Only real target polynomials.** The symmetric-phase restriction means $g(x, \Phi) = \operatorname{Re}[\langle 0|U_\Phi|0\rangle]$ is a real polynomial. Complex target polynomials need two separate optimisations (real and imaginary parts).

## Reusable ideas

1. [[Chebyshev-Node Loss for QSP Phase Optimisation]] — sample the loss function at Chebyshev nodes to get coefficient-level control from pointwise error. The discrete orthogonality of Chebyshev polynomials on their roots makes this exact, not approximate.

2. [[Perturbative Initial Guess for QSP Phases]] — for small target polynomials, QSP phases are approximately half the Chebyshev coefficients. Combined with scaling-and-squaring, this gives a reliable initial point for optimisation even when $\|f\|_\infty \approx 1$.

## References within this paper

- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|Low & Chuang (2017)]] — introduced QSP for Hamiltonian simulation; this paper makes it compilable
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|Gilyén, Su, Low & Wiebe (2019)]] — QSVT framework; the [[Recursive Phase Extraction from Polynomial Coefficients|recursive extraction]] algorithm that this paper replaces
- [[Grand Unification of Quantum Algorithms (Martyn-Rossi-Tan-Chuang 2021) — Paper Notes|Martyn et al. (2021)]] — pedagogical review of QSP/QSVT that uses QSPPACK for all its examples
- [[Optimal Polynomial Based Quantum Eigenstate Filtering (Dong-An-Lin 2020) — Paper Notes|Dong, An & Lin (2020)]] — eigenstate filtering application; same group, concurrent work
- Haah (2019) — Prony-based phase-factor computation, arXiv:1806.10236. Better than GSLW but still needs extended precision
- [[Energy Landscape of Symmetric QSP (Wang-Dong-Lin 2022) — Paper Notes|Wang, Dong & Lin (2022)]] — explains *why* the optimisation works: the landscape is well-behaved near the [[Maximal Solution Selection via Mahler Measure|maximal solution]]
- Low, Yoder & Chuang (2016) — composite quantum gates, introduced complementary polynomial construction via [[Partial-Quadrature Completion via Root Grouping|root grouping]]

## Cross-links

### Paper notes
- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes]]
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]]
- [[Grand Unification of Quantum Algorithms (Martyn-Rossi-Tan-Chuang 2021) — Paper Notes]]
- [[Optimal Polynomial Based Quantum Eigenstate Filtering (Dong-An-Lin 2020) — Paper Notes]]
- [[Energy Landscape of Symmetric QSP (Wang-Dong-Lin 2022) — Paper Notes]]
- [[QET-U — Ground-State Preparation and Energy Estimation on Early Fault-Tolerant QC (Dong-Lin-Tong 2022) — Paper Notes]]
- [[Resonant Equiangular Composite Gates (Low-Yoder-Chuang 2016) — Paper Notes]]

### Trick cards
- [[Chebyshev-Node Loss for QSP Phase Optimisation]]
- [[Perturbative Initial Guess for QSP Phases]]
- [[Recursive Phase Extraction from Polynomial Coefficients]]
- [[Symmetry Reduction for Phase Factor Uniqueness]]
- [[Laurent Polynomial Root Grouping for QSP Complementary Polynomials]]
- [[Maximal Solution Selection via Mahler Measure]]
