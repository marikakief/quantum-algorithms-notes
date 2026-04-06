> **Source:** Andrew M. Childs, Jin-Peng Liu, Aaron Ostrander, *High-precision quantum algorithms for partial differential equations*, Quantum **5**, 574 (2021); arXiv:2002.07868  
> **Links:** [arXiv](https://arxiv.org/abs/2002.07868) · [Quantum](https://doi.org/10.22331/q-2021-11-10-574)  
> **Tags:** #PDE #QLSA #spectral-method #finite-difference #Poisson-equation #elliptic-PDE #high-precision #QFT

---

## The computational problem

Given a second-order elliptic PDE $\mathcal{L}(u(x)) = f(x)$ on the unit hypercube $D = [-1,1]^d$ with Dirichlet/Neumann/periodic boundary conditions, produce a quantum state $|u\rangle$ whose amplitudes are proportional to the solution $u$ evaluated at grid points, with $\ell_2$ error at most $\varepsilon$.

The operator $\mathcal{L}$ has the form:

$$\mathcal{L}(u) = \sum_{j_1, j_2=1}^{d} A_{j_1 j_2} \frac{\partial^2 u}{\partial x_{j_1} \partial x_{j_2}} = f(x)$$

with a "global strict diagonal dominance" condition $C := 1 - \sum_{j_1} \frac{1}{|A_{j_1,j_1}|}\sum_{j_2 \ne j_1} |A_{j_1,j_2}| > 0$.

The Poisson equation $\Delta u = f$ is the canonical special case, with $C = 1$.

## What the paper does

Achieves **$\mathrm{poly}(d, \log(1/\varepsilon))$ complexity for quantum PDE solving** — the first algorithm to get polylogarithmic precision scaling for PDEs. Previous quantum PDE algorithms (Montanaro–Pallister FEM, Cao–Papageorgiou–Petras–Traub FDM) all had $\mathrm{poly}(1/\varepsilon)$ scaling because their discretisation errors were polynomial in the grid spacing, even when using the high-precision [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes|QLSA]].

The insight: the QLSA bottleneck wasn't the only bottleneck. Finite difference and finite element discretisations introduce $\mathrm{poly}(h)$ approximation error (where $h$ is grid spacing), so you need $h = \mathrm{poly}(\varepsilon)$ grid points, which feeds polynomial $\varepsilon$-dependence into the QLSA's condition number. To get polylog precision end-to-end, you need a discretisation whose error also decreases superpolynomially. **Spectral methods** provide exactly this: for smooth solutions, truncated Fourier/Chebyshev series have error decaying faster than any polynomial in $1/n$, so $n = \mathrm{polylog}(1/\varepsilon)$ basis functions suffice.

**My assessment:** This paper connects two well-established classical ideas (spectral methods and high-precision QLSA) in the natural way, but executing that connection cleanly requires substantial technical work — bounding condition numbers of the spectral discretisation matrices, handling boundary conditions via QSFT/QCT, and state preparation. The adaptive FDM part is less exciting (still $d^{6.5}$), but the spectral method result ($d^2$ for general elliptic, $d$ for Poisson) is the main contribution.

## The algorithm / construction

### Approach 1: Adaptive Finite Difference Method (Section 3, for Poisson's equation)

**Step 1 — High-order central differences.** Approximate $u''(x)$ by a $(2k)$th-order central difference formula:

$$u''(x_0) \approx \frac{1}{h^2}\sum_{j=-k}^{k} r_j u(x_0 + jh)$$

where $r_j$ are the standard central-difference coefficients. The error is $O(\|u^{(2k+1)}\| (eh/2)^{2k-1})$.

The key trick: **adapt the order $k$ depending on $\varepsilon$**, rather than fixing $k$ and refining the grid. Choose $k = O(\log(1/\varepsilon)/\log\log(1/\varepsilon))$, which makes the finite-difference error polylogarithmic in $1/\varepsilon$ even on a relatively coarse grid.

**Step 2 — Condition number analysis.** The resulting circulant Laplacian matrix $L$ on $2n$ grid points has condition number $\kappa(L) = O(n^2)$. For the $d$-dimensional tensor-sum Laplacian, $\kappa = O(dn^2)$.

**Step 3 — QFT diagonalisation.** The circulant structure means the matrix is diagonalised by the QFT. Solve the system by: QFT → multiply by inverse eigenvalues → inverse QFT. But the multiply step needs the [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes|high-precision QLSA]] because the condition number is large.

**Result (Theorem 1):**

$$\tilde{O}\!\left(d^{6.5}\,\mathrm{poly}(\log(\|u^{(2k+1)}\|/\varepsilon))\right)$$

### Approach 2: Quantum Spectral Method (Section 4, for general elliptic PDEs)

**Step 1 — Spectral discretisation.** Approximate $u(x)$ as a truncated Fourier series (periodic BC) or Chebyshev series (Dirichlet BC):

$$u(x) = \sum_{\|k\|_\infty \le n} c_k \phi_k(x)$$

where $\phi_k$ are Fourier modes $e^{ik\pi x}$ or Chebyshev polynomials $T_k(x)$. For $C^\infty$ solutions, the spectral convergence gives error $O(g'/(en)^n)$ where $g' = \max_x \max_n \|u^{(n+1)}(x)\|$, so $n = \mathrm{polylog}(g'/\varepsilon)$ suffices.

**Step 2 — Linear system construction.** Substituting the spectral expansion into the PDE gives a linear system $L\vec{x} = \vec{f} + \vec{g}$, where:
- $L = \sum_{\|j\|_1=2} A_j \bar{D}_n^{(j)}$ combines the differential matrices $D_n$ (diagonal for Fourier, upper-triangular for Chebyshev) with boundary-condition rows
- The boundary conditions replace certain rows (those where $D_n$ has a zero row) with identity constraints encoding $u|_{\partial D} = \gamma$

**Step 3 — Sparsification via QFT/QCT.** In the raw spectral-coefficient basis, the linear system can be dense (the Chebyshev differentiation matrix $D_n$ is upper-triangular with $O(n)$ nonzeros per row). The [[Structured PDE Hamiltonian Simulation via QFT-QSFT-QCT (Quantum 2021-11-10-574) — Paper Notes|quantum shifted Fourier transform (QSFT)]] and quantum cosine transform (QCT) diagonalise or sparsify the relevant matrices, bringing the system into a form compatible with the sparse-access QLSA.

**Step 4 — Condition number bound.** For the Poisson equation: $\kappa \le (2n)^4$ (Corollary 1). For general elliptic PDEs: $\kappa \le \frac{\|A\|_\Sigma}{C\|A\|_*}(2n)^4$ (Lemma 10), where $\|A\|_\Sigma = \sum |A_j|$, $\|A\|_* = \sum |A_{j,j}|$, and $C$ is the global strict diagonal dominance constant. The proof uses a decomposition $L = L_1 + L_2$ where $L_1$ is a Kronecker sum (bounded by Lemma 9) and $\|L_2 L_1^{-1}\| \le 1-C$ by AM-GM on singular values.

**Step 5 — State preparation.** Prepare $|B\rangle \propto \sum_k (\hat{f}_k + \sum_j A_{j,j}\hat{\gamma}_k^{j\pm})|k\rangle$ using oracles for $f$ and $\gamma$, inverse QSFT/QCT, and amplitude amplification with a factor of $q$ (measuring the ratio of $\ell_2$ norms before and after combining). Complexity: $O(qd^2 \log n \log\log n)$.

**Result (Theorem 2):**

$$\left(\frac{d\|A\|_\Sigma}{C\|A\|_*} + qd^2\right)\mathrm{poly}(\log(g'/g\varepsilon)) \text{ queries}$$

For Poisson with homogeneous BC: $d\,\mathrm{poly}(\log(g'/g\varepsilon))$.

## Key results

| Setting | Complexity | Prior best |
|---|---|---|
| Poisson, homogeneous BC | $d\,\mathrm{poly}(\log(g'/g\varepsilon))$ | $d\,\mathrm{poly}(\log d, 1/\varepsilon)$ |
| Poisson, Dirichlet BC | $d\,\mathrm{poly}(\log d, \log(g'/g\varepsilon))$ | $\mathrm{poly}(d, 1/\varepsilon)$ |
| General elliptic, Dirichlet BC | $d^2\,\mathrm{poly}(\log(1/\varepsilon))$ | $\mathrm{poly}(d, 1/\varepsilon)$ |

The exponential improvement is in $\varepsilon$-dependence: from $\mathrm{poly}(1/\varepsilon)$ to $\mathrm{poly}(\log(1/\varepsilon))$. The $d$-dependence is polynomial in all cases.

## Comparison with prior work

| Approach | $\varepsilon$-scaling | $d$-scaling | Equation type |
|---|---|---|---|
| Classical FDM/FEM | $\mathrm{poly}((1/\varepsilon)^d)$ | exponential | general |
| Montanaro-Pallister FEM | $\mathrm{poly}(d, 1/\varepsilon)$ | polynomial | Poisson |
| Cao et al. FDM | $d\,\mathrm{poly}(\log d, 1/\varepsilon)$ | polynomial | Poisson |
| Adaptive FDM (this paper) | $d^{6.5}\,\mathrm{polylog}(1/\varepsilon)$ | polynomial | Poisson |
| Spectral (this paper) | $d^2\,\mathrm{polylog}(1/\varepsilon)$ | polynomial | elliptic |

## Limits / caveats

- The complexity depends logarithmically on high-order derivatives $g' = \max \|u^{(n+1)}\|$. For highly oscillatory solutions, $g'$ can be exponentially large, which partially offsets the polylog scaling. This is inherited from classical spectral methods — it's not a quantum artifact.
- **Global strict diagonal dominance** (condition (2.8)) is required for the condition number bound. This holds for Poisson but may fail for general elliptic equations with large off-diagonal coupling.
- Only second-order elliptic PDEs on the unit hypercube. Extension to parabolic/hyperbolic PDEs, non-rectangular domains, and variable coefficients remains open (at time of publication).
- State preparation involves a factor $q$ from amplitude amplification when combining inhomogeneity and boundary data. For badly conditioned boundary data, $q$ can be large.
- As with all QLSA-based PDE solvers, the output is a quantum state. Extracting classical solution values requires $O(N)$ measurements, negating the speedup for full readout.

## Reusable ideas

1. [[Adaptive-Order Finite Differences for Polylog Precision]] — choose the FDM order $k$ as a function of $\varepsilon$ rather than fixing it, getting superpolynomial error decay from a finite-difference scheme.
2. [[Spectral Discretisation for High-Precision Quantum PDE Solving]] — use Fourier/Chebyshev spectral methods instead of finite differences when feeding a PDE into a QLSA, to avoid the $\mathrm{poly}(1/\varepsilon)$ discretisation bottleneck.
3. [[Kronecker Sum Condition Number Bound]] — bound the condition number of a Kronecker sum $L = \bigoplus_j M_j$ by $\kappa_L \le \frac{\sum \sigma_{\max}^{(j)}}{\sum \sigma_{\min}^{(j)}}$, using properties of the matrix exponential and Kronecker products.

## References within this paper

- [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes|Childs, Kothari & Somma (2017)]] — the high-precision QLSA used as a subroutine throughout
- [[Quantum Spectral Methods for Differential Equations (Childs-Liu 2020) — Paper Notes|Childs & Liu (2020)]] — spectral methods for ODEs; this paper extends the approach to PDEs
- [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes|Berry, Childs, Ostrander & Wang (2017)]] — quantum ODE solver with $\mathrm{polylog}(1/\varepsilon)$ for time-independent equations
- Montanaro & Pallister (2015, arXiv:1512.05903) — quantum FEM for Poisson; this paper improves their $\varepsilon$-dependence
- Cao, Papageorgiou, Petras & Traub (2013) — quantum FDM for Poisson with poly$(1/\varepsilon)$ scaling
- Costa, Jordan & Ostrander (2019, arXiv:1901.02510) — quantum algorithm for hyperbolic PDEs via FVM; also poly$(1/\varepsilon)$
- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|Harrow, Hassidim & Lloyd (2009)]] — original QLSA
- [[Quantum Algorithm for Nonhomogeneous Linear PDEs (Arrazola-Kalajdzievski-Weedbrook-Lloyd 2019) — Paper Notes|Arrazola et al. (2019)]] — nonhomogeneous linear PDEs

## Cross-links

### Paper notes
- [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes]] — the QLSA whose polylog precision makes this paper possible
- [[Quantum Spectral Methods for Differential Equations (Childs-Liu 2020) — Paper Notes]] — predecessor; spectral methods for ODEs
- [[Efficient Quantum Algorithm for Dissipative Nonlinear Differential Equations (Liu-Kolden-Krovi-Loureiro-Trivisa-Childs 2021) — Paper Notes]] — related; nonlinear ODE solving by the same group
- [[Quantum Simulation of Real-Space Dynamics (Childs-Leng-Li-Liu-Zhang 2022) — Paper Notes]] — applies spectral discretisation to real-space quantum simulation
- [[Bounding the Costs of Quantum Simulation of Many-Body Physics in Real Space (Kivlichan, Wiebe, Babbush, Aspuru-Guzik 2017) — Paper Notes]] — earlier real-space discretisation analysis with poly$(1/\varepsilon)$
- [[Structured PDE Hamiltonian Simulation via QFT-QSFT-QCT (Quantum 2021-11-10-574) — Paper Notes]] — closely related; may be a duplicate entry for QSFT/QCT techniques from this paper

### Trick cards
- [[Adaptive-Order Finite Differences for Polylog Precision]]
- [[Spectral Discretisation for High-Precision Quantum PDE Solving]]
- [[Kronecker Sum Condition Number Bound]]
- [[Basis Switching via QFT for Kinetic-Potential Splitting]]
