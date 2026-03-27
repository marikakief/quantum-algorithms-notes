> **Source:** Dong An, Andrew M. Childs, and Lin Lin, *Quantum algorithm for eigenvalue transformation of general matrices using the Laplace representation*, arXiv:2411.04010 (2024)
> **Links:** [arXiv](https://arxiv.org/abs/2411.04010)
> **Tags:** #quantum-algorithm #hamiltonian-simulation #non-unitary #eigenvalue-transform #LCHS #LCU #dissipative #ODE #block-encoding

---

## The computational problem

Given a block-encoded matrix $A \in \mathbb{C}^{N \times N}$ whose Hermitian part $L = (A + A^\dagger)/2$ satisfies $L \succeq 0$ (dissipative), and a function $h$ expressible as a Laplace transform $h(z) = \int_0^\infty g(t)\, e^{-zt}\, dt$ with $g \in L^1(\mathbb{R}_+)$, implement the eigenvalue transformation $h(A)$ as a block-encoded operator on a quantum computer.

Equivalently: prepare the state $h(A)|\psi\rangle / \|h(A)|\psi\rangle\|$ for a given input state $|\psi\rangle$.

---

## What the paper does

Generalises [[LCHS Kernel for Non-Unitary Dynamics|LCHS]] from implementing a single matrix exponential $e^{-tA}$ to implementing arbitrary eigenvalue transforms $h(A)$ that have well-behaved inverse Laplace transforms. The construction — called **Lap-LCHS** — writes $h(A)$ as a double integral over [[Hamiltonian simulation]] unitaries, then discretises and implements via [[Linear Combination of Unitaries (LCU)|LCU]] with [[Standard Amplitude Amplification|amplitude amplification]].

The key identity: if $h(z) = \int_0^\infty g(t)\, e^{-zt}\, dt$ and $e^{-tA}$ has the LCHS representation $e^{-tA} = \int_\mathbb{R} f(k)\,(1-ik)^{-1}\, e^{-it(kL+H)}\, dk$ (where $A = L + iH$), then:

$$h(A) = \int_0^\infty \int_\mathbb{R} \frac{f(k)\, g(t)}{1-ik}\, e^{-it(kL+H)}\, dk\, dt.$$

This double integral is a continuous [[Linear Combination of Unitaries (LCU)|LCU]] of [[Hamiltonian simulation]] unitaries. Discretise both integrals, implement each $e^{-it(kL+H)}$ via [[QSVT Meta-Template|QSVT]]-based Hamiltonian simulation, and combine via LCU.

This is a genuinely different approach from [[Quantum Eigenvalue Processing and Transformation for Non-Normal Matrices (arXiv 2401.06240) — Paper Notes|QEVT]] (which uses history-state polynomial bases and QLSA) and from direct [[QSVT Meta-Template|QSVT]] (which processes singular values, not eigenvalues). Lap-LCHS works through the Laplace transform representation, which is natural for operator functions arising from dissipative dynamics and ODEs.

---

## The algorithm / construction

### Step 1: LCHS identity (restated from An-Liu-Lin 2023)

For $A = L + iH$ with $L \succeq 0$, choose a kernel $f \in H^1(\mathbb{C}^-)$ (the Hardy space of the left half-plane) with $\int_\mathbb{R} f(k)/(1-ik)\, dk = 1$. Then for $t \geq 0$:

$$e^{-tA} = \int_\mathbb{R} \frac{f(k)}{1-ik}\, e^{-it(kL+H)}\, dk.$$

**Near-optimal kernel choice:** $f(k) = \frac{1}{2\pi} e^{-2\beta}\, e^{(1+ik)^\beta}$ for $\beta \in (0,1)$. This gives near-exponential decay $|f(k)| \sim e^{-c|k|^\beta}$, so truncation to $|k| \leq K$ with $K = O((\log 1/\varepsilon)^{1/\beta})$ introduces error $\leq \varepsilon$.

### Step 2: Lap-LCHS representation

Substitute the LCHS identity into $h(A) = \int_0^\infty g(t)\, e^{-tA}\, dt$ and interchange integrals (justified by Fubini when $f \cdot g$ is jointly $L^1$):

$$h(A) = \int_0^\infty \int_\mathbb{R} w(k,t)\, e^{-it(kL+H)}\, dk\, dt, \quad w(k,t) = \frac{f(k)\, g(t)}{1-ik}.$$

### Step 3: Discretisation

Truncate: $k \in [-K, K]$, $t \in [0, T]$. Apply Riemann sums (or higher-order quadrature) with grid spacings $h_k$, $h_t$, giving $M_k \times M_t$ quadrature points.

- **$k$-truncation error:** controlled by $\|f\|_{L^1(\mathbb{R} \setminus [-K,K])}$, which decays like $e^{-cK^\beta}$ for the near-optimal kernel.
- **$t$-truncation error:** controlled by $\|g\|_{L^1((T,\infty))}$, which depends on the tail decay of the specific transform $h$.
- **Quadrature error:** standard Riemann/trapezoidal bounds involving $h_k$, $h_t$, and derivatives of $f$, $g$.

### Step 4: Quantum implementation

1. Prepare a two-register state encoding the quadrature weights $w(k_j, t_l)$ (state-preparation oracle).
2. Conditionally apply $e^{-it_l(k_j L + H)}$ via [[QSVT Meta-Template|QSVT]]-based [[Hamiltonian simulation]]. The Hamiltonian $k_j L + H$ is formed from the block-encoding of $A$ via [[Linear Combination of Unitaries (LCU)|LCU]].
3. Un-compute the weight register (LCU protocol).
4. Apply [[Standard Amplitude Amplification|amplitude amplification]] to boost success probability.

---

## Key results

### Theorem 6 (Block encoding of $h(A)$)

Given an $(\alpha_A, m, 0)$-block encoding of $A$, the algorithm produces an $(\|f\|_{L^1} \cdot \|g\|_{L^1},\, m',\, \varepsilon)$-block encoding of $h(A)$ using:

$$\tilde{O}\!\left(\alpha_A K T + \log(1/\varepsilon)\right)$$

queries to the block encoding of $A$.

### Corollary 7 (State preparation)

To prepare $h(A)|\psi\rangle / \|h(A)|\psi\rangle\|$ to error $\varepsilon$:

$$\tilde{O}\!\left(\frac{\|g\|_{L^1}}{\|h(A)|\psi\rangle\|} \cdot \alpha_A T \cdot \left(\log \frac{1}{\varepsilon}\right)^{1+o(1)}\right)$$

queries to the block encoding of $A$ and $O(\|g\|_{L^1} / \|h(A)|\psi\rangle\|)$ preparations of $|\psi\rangle$.

The state-preparation cost (number of copies of $|\psi\rangle$) is optimal up to log factors — matching the LCHS optimality from the original paper.

---

## Applications

### Inhomogeneous ODE: $u'(t) = -Au(t) + |\psi\rangle$, $u(0) = 0$

Solution operator: $h(z) = (1 - e^{-zT})/z$ with $g(t) = \mathbf{1}_{[0,T]}(t)$.

**Corollary 8:** Prepare $u(T)/\|u(T)\|$ with
$$O\!\left(\frac{\alpha_A T^2}{\|u(T)\|} \cdot \left(\log \frac{T}{\|u(T)\|\varepsilon}\right)^{1/\beta}\right)$$
queries to $A$ and $O(T/\|u(T)\|)$ preparations of $|\psi\rangle$.

Compared to truncated-Taylor/QLSA methods: better state-preparation cost (fewer copies of $|\psi\rangle$), improved log-precision factor, but the $T^2$ scaling in matrix queries may be worse than QLSA-based methods that achieve $O(T)$ dependence. Same trade-off as in [[Time-Marching Quantum Solvers for Linear ODEs (Fang-Lin-Tong 2023) — Paper Notes|Fang-Lin-Tong (2023)]].

### Fractional power inverse: $h(z) = (\eta + z)^{-p}$, $p > 0$

**Corollary 9:** Prepare $(\eta I + A)^{-p}|b\rangle / \|\cdot\|$ with
$$O\!\left(\frac{\alpha_A}{\eta^{-(p+1)} \|x\|} \cdot \left(\log \frac{1}{\varepsilon \eta \|x\|}\right)^{1+1/\beta}\right)$$
queries to $A$.

This extends quantum linear system solvers to non-integer matrix powers — fractional resolvent operators that appear in fractional diffusion equations and regularisation theory.

### Mass-matrix ODE: $A\, du/dt = -u$, solution $u(T) = e^{-TA^{-1}} u(0)$

Lap-LCHS implements $e^{-TA^{-1}}$ without explicitly computing $A^{-1}$, using Laplace representations involving Bessel functions. Requires $L \succeq \gamma I > 0$ (bounded away from zero).

---

## Comparison with prior work

| Method | Target | Matrix queries | State prep cost | Precision | Restrictions |
|--------|--------|---------------|-----------------|-----------|--------------|
| [[QSVT Meta-Template\|QSVT]] | $p(\sigma_i)$ (singular values) | $O(d)$ for degree-$d$ poly | 1 | $\log(1/\varepsilon)$ | Normal/Hermitian for eigenvalues |
| [[Quantum Eigenvalue Processing and Transformation for Non-Normal Matrices (arXiv 2401.06240) — Paper Notes\|QEVT]] | $f(\lambda_i)$ (eigenvalues) | $\tilde{O}(\kappa_V / \varepsilon)$ | 1 | Heisenberg-limited | Diagonalisable, condition $\kappa_V$ |
| LCHS (An-Liu-Lin 2023) | $e^{-tA}$ | $\tilde{O}(\alpha_A t)$ | Optimal | $(\log 1/\varepsilon)^{1+o(1)}$ | Dissipative ($L \succeq 0$) |
| **Lap-LCHS** (this paper) | $h(A)$ via Laplace | $\tilde{O}(\alpha_A T)$ | Optimal | $(\log 1/\varepsilon)^{1+o(1)}$ | Dissipative + $g \in L^1$ |
| [[Time-Marching Quantum Solvers for Linear ODEs (Fang-Lin-Tong 2023) — Paper Notes\|Time-marching]] | ODE propagator | $O(\alpha^2 T^2 Q)$ | $O(Q)$ | $\log(1/\varepsilon)$ | General |

Lap-LCHS sits between LCHS (which it directly extends) and QEVT (which handles non-normal matrices via completely different machinery). When $h$ has a nice Laplace representation, Lap-LCHS is simpler and often cheaper than QEVT. When the matrix is non-dissipative or $h$ lacks a Laplace representation, QEVT is the tool.

---

## Limits / caveats

1. **Dissipativity is non-negotiable.** The method requires $L = (A + A^\dagger)/2 \succeq 0$. Matrices with eigenvalues in the left half-plane (growing dynamics) are excluded. This is inherited from the original LCHS identity and can't be removed without a fundamentally different representation.

2. **Tail decay of $g(t)$ controls $T$.** If the inverse Laplace transform $g$ decays only polynomially, the truncation parameter $T$ grows polynomially in $1/\varepsilon$, degrading the complexity. The nice applications in the paper (fractional powers, matrix exponentials of inverses) have exponential or compact-support $g$. For general $h$, this needs case-by-case analysis.

3. **$T^2$ matrix-query scaling for ODEs.** The inhomogeneous ODE application scales as $O(\alpha_A T^2)$ in matrix queries, versus $O(T)$ for QLSA-based approaches. Whether this gap is fundamental or an artifact of the Lap-LCHS approach is open.

4. **Block-encoding factor $\|f\|_{L^1} \cdot \|g\|_{L^1}$.** The overall normalisation of the block encoding scales with these $L^1$ norms. If either is large, amplitude amplification cost grows proportionally.

5. **Not applicable when $L$ has a nontrivial kernel.** Several applications (mass-matrix ODE) require the stronger condition $L \succeq \gamma I > 0$. The gap $\gamma$ enters the complexity.

---

## My assessment

This is a clean, well-motivated generalisation of LCHS. The Laplace-transform representation is the natural way to extend from $e^{-tA}$ to a broader function class, and the paper does a thorough job of working through the discretisation analysis and applications.

The technique is most compelling for problems where (a) the matrix is naturally dissipative and (b) the target function arises from an ODE or PDE context where the Laplace representation is already standard. The fractional-power inverse and mass-matrix ODE applications are good examples.

For non-normal matrices without the dissipativity constraint, [[Quantum Eigenvalue Processing and Transformation for Non-Normal Matrices (arXiv 2401.06240) — Paper Notes|QEVT]] remains the more general (if more complex) tool. And for Hermitian/normal matrices, [[QSVT Meta-Template|QSVT]] is still optimal. Lap-LCHS carves out a middle ground: dissipative non-normal matrices with Laplace-friendly target functions.

The connection to Marika's work is through the [[Hamiltonian simulation]] underpinning — each term in the LCU is a standard [[Hamiltonian simulation]] problem, so improvements to simulation (whether via [[Randomizing Multi-Product Formulas for Hamiltonian Simulation (Faehrmann-Steudtner-Kueng-Kieferová-Eisert 2022) — Paper Notes|randomised multi-product formulas]], [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes|truncated Dyson series]], or stochastic QSP) directly improve Lap-LCHS.

---

## Reusable ideas

1. [[Laplace-Transform LCU Lifting for Eigenvalue Transforms]] — the core idea of writing $h(A)$ as a double LCU via its Laplace representation
2. [[Near-Optimal Hardy-Space Kernel for LCHS]] — the specific kernel choice $f(k) = \frac{1}{2\pi} e^{-2\beta} e^{(1+ik)^\beta}$ with near-exponential decay
3. [[Double-Integral Quadrature for Operator LCU]] — the discretisation scheme for converting a continuous 2D LCU into a finite sum

---

## References within this paper

- [[LCHS Kernel for Non-Unitary Dynamics|An-Liu-Lin (2023)]] — the original LCHS paper (Phys. Rev. Lett. 131, 150603). This paper directly extends it.
- [[Quantum Algorithm for Linear Non-Unitary Dynamics with Near-Optimal Dependence on All Parameters (An-Childs-Lin 2023) — Paper Notes|An-Childs-Lin (arXiv:2312.03916)]] — introduces the generalised kernel family and near-optimal precision scaling that Lap-LCHS builds on.
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|Gilyén et al. (2019)]] — [[QSVT Meta-Template|QSVT]]-based [[Hamiltonian simulation]] used as the inner subroutine
- [[Quantum Eigenvalue Processing and Transformation for Non-Normal Matrices (arXiv 2401.06240) — Paper Notes|An-Childs-Lin (2024)]] — QEVT, the alternative (history-state-based) approach to non-normal eigenvalue transforms by the same authors
- [[Simulating Hamiltonian Dynamics with a Truncated Taylor Series (Berry-Childs-Cleve-Kothari-Somma 2015) — Paper Notes|Berry et al. (2015)]] — truncated Taylor series + [[Linear Combination of Unitaries (LCU)|LCU]] method
- [[Time-Marching Quantum Solvers for Linear ODEs (Fang-Lin-Tong 2023) — Paper Notes|Fang-Lin-Tong (2023)]] — time-marching ODE solver, comparison point for the inhomogeneous ODE application
- [[High-Order Quantum Algorithm for Solving Linear Differential Equations (Berry 2014) — Paper Notes|Berry (2014)]] — history-state ODE solver, earlier approach
- [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes|Berry-Childs-Ostrander-Wang (2017)]] — linear ODE algorithms
- Childs & Kothari (2017) — Fourier/Chebyshev [[Linear Combination of Unitaries (LCU)|LCU]] methods for matrix functions
- Hale, Higham & Trefethen (2008) — classical numerical contour integrals for matrix functions (the classical analogue of what Lap-LCHS does quantumly)

---

## Cross-links

### Paper notes
- [[Quantum Algorithm for Linear Non-Unitary Dynamics with Near-Optimal Dependence on All Parameters (An-Childs-Lin 2023) — Paper Notes]]
- [[LCHS Kernel for Non-Unitary Dynamics]]
- [[Quantum Eigenvalue Processing and Transformation for Non-Normal Matrices (arXiv 2401.06240) — Paper Notes]]
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]]
- [[Simulating Hamiltonian Dynamics with a Truncated Taylor Series (Berry-Childs-Cleve-Kothari-Somma 2015) — Paper Notes]]
- [[Time-Marching Quantum Solvers for Linear ODEs (Fang-Lin-Tong 2023) — Paper Notes]]
- [[Quantum Linear Matrix Equations — Paper Notes]]
- [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes]]
- [[Randomizing Multi-Product Formulas for Hamiltonian Simulation (Faehrmann-Steudtner-Kueng-Kieferová-Eisert 2022) — Paper Notes]]
- [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes]]
- [[Hamiltonian Simulation — Comparison Tables]]

### Trick cards
- [[Laplace-Transform LCU Lifting for Eigenvalue Transforms]]
- [[Near-Optimal Hardy-Space Kernel for LCHS]]
- [[Double-Integral Quadrature for Operator LCU]]
- [[LCHS Kernel for Non-Unitary Dynamics]]
- [[Linear Combination of Unitaries (LCU)]]
- [[QSVT Meta-Template]]
