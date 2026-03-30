> **Source:** Sarah K. Leyton, Tobias J. Osborne, *A quantum algorithm to solve nonlinear differential equations*, arXiv:0812.4423, 2008
> **Links:** [arXiv](https://arxiv.org/abs/0812.4423)
> **Tags:** #nonlinear-ODE #quantum-algorithm #differential-equations #Euler-method #polynomial-map #nondeterministic

---

## The computational problem

**Input:** A system of $n$ first-order nonlinear ODEs $\dot{z}_j(t) = f_j(z_1, \ldots, z_n)$, $j = 1, \ldots, n$, where the $f_j$ are sparse polynomials (initially described for degree 2, extended to arbitrary degree). Initial condition $z(0) = b$ with $\|z\|^2 = 1$.

**Output:** Expectation values $\langle M \rangle = \sum_{j,k} z_j(t) M_{jk} z_k(t)$ of efficiently simulable observables $M$ on the quantum state encoding the solution at time $t$.

The variables $z_j(t)$ are encoded as amplitudes of a quantum state:

$$|\phi\rangle = \frac{1}{\sqrt{2}}|0\rangle + \frac{1}{\sqrt{2}}\sum_{j=1}^n z_j |j\rangle$$

using $O(\log n)$ qubits.

## What the paper does

This is the first attempt at a quantum algorithm for nonlinear ODEs. The approach is conceptually simple: implement Euler's method on the probability amplitudes of a quantum state, where each Euler step is a nonlinear polynomial map $z \mapsto z + h\cdot f(z)$. Since quantum mechanics is linear, implementing a nonlinear transformation requires consuming multiple copies of the input state and postselecting — making the algorithm nondeterministic.

The resources scale as $\text{poly}(\log n)$ in the number of variables (exponential improvement over classical $O(n)$), but exponentially in the integration time $t$ and inverse step size $1/h$. This exponential-in-$t$ scaling was the central limitation that later work by [[Efficient Quantum Algorithm for Dissipative Nonlinear Differential Equations (Liu-Kolden-Krovi-Loureiro-Trivisa-Childs 2021) — Paper Notes|Liu-Kolden-Krovi et al. (2021)]] addressed via [[Carleman Linearisation for Quantum Nonlinear ODE Solvers|Carleman linearisation]].

**My assessment:** This is a historically important proof-of-concept, not a practical algorithm. The exponential scaling in $t$ and $1/h$ is devastating — you're paying exponentially to run a first-order method that has poor convergence even classically. The paper is upfront about this limitation. The real contribution is the conceptual insight that nonlinear amplitude transformations can be implemented nondeterministically by consuming tensor products of the state and postselecting. This idea influenced all subsequent work in the area, even though the specific implementation (Euler's method) was superseded. The paper also raised the prescient question of whether a polynomial-in-$\log t$ algorithm exists — a question partially answered (positively, under conditions) by the Carleman linearisation papers.

---

## The algorithm / construction

### Nonlinear amplitude transformation

Given a state $|\phi\rangle$ encoding unknown amplitudes $\{z_j\}$, prepare two copies $|\phi\rangle|\phi\rangle$. The tensor product state has amplitudes $z_j z_k$ for all pairs $(j,k)$ — every degree-2 monomial appears.

Construct the operator $A = \sum_{\alpha,k,l} a_{kl}^{(\alpha)} |\alpha 0\rangle\langle kl|$ encoding the polynomial map coefficients, and form the Hamiltonian

$$H = -iA \otimes |1\rangle_P\langle 0| + iA^\dagger \otimes |0\rangle_P\langle 1|$$

using an ancilla pointer qubit $P$. Evolve the initial state $|\phi\rangle|\phi\rangle|0\rangle_P$ under $H$ for time $\varepsilon$:

$$e^{i\varepsilon H}|\phi\rangle|\phi\rangle|0\rangle = |\phi\rangle|\phi\rangle|0\rangle + \varepsilon A|\phi\rangle|\phi\rangle|1\rangle + O(\varepsilon^2).$$

Since $A|\phi\rangle|\phi\rangle = \frac{1}{\sqrt{2}}|\phi'\rangle|0\rangle$ where $|\phi'\rangle$ encodes $f_\alpha(z)$, measuring the pointer qubit and postselecting on $|1\rangle$ yields $|\phi'\rangle$ with probability $\approx \varepsilon^2/2$.

**Sparsity assumptions:** The algorithm requires that each polynomial $f_\alpha$ involves at most $s = O(1)$ monomials, and each variable appears in at most $O(1)$ polynomials. This ensures efficient Hamiltonian simulation of $H$ in $\text{poly}(\log n)$ time.

### Euler integration

To advance from $|\phi(t)\rangle$ to $|\phi(t+h)\rangle$:
1. Apply the nonlinear transformation to implement $z \mapsto z + hf(z)$
2. Each step consumes 2 copies and succeeds with probability $\approx \varepsilon^2/2$
3. For $m = t/h$ steps, start with $(16/\varepsilon^2)^m$ copies of $|\phi(0)\rangle$
4. Apply the transformation in parallel, discarding failures at each round

### Resource analysis

The total number of initial copies needed is $N = (p/16)^{-m}$ where $p = \varepsilon^2/2$, i.e., **exponential in $m = t/h$**.

**Spatial resources:** $O((16/\varepsilon^2)^m \log n)$ — polynomial in $\log n$, exponential in $m$.

**Running time:** $T \sim m\, \text{poly}(\log n \cdot \log^* n)\, s^2\, 9^{\kappa\sqrt{m}}$ — subexponential in $m$, polynomial in $\log n$.

**Error accumulation:** After $m$ steps, the error satisfies $\delta_m \leq \eta \frac{(3\gamma)^{m+1} - 1}{3\gamma - 1}$ where $\eta$ is the per-step simulation error and $\gamma = O(s)$ depends on sparsity. This requires the simulation error to satisfy $\eta < (3\gamma)^{-m}$.

---

## Key results

**Theorem (Proposition 1):** Starting with $N = (p/16)^{-m}$ copies (where $p = \varepsilon^2/2$), Algorithm 1 produces at least one copy of $|\phi^{(m)}\rangle$ (encoding the $m$-th Euler iterate) with probability $\geq 1/3$ for $m \geq 6$.

**Theorem (Proposition 2):** The accumulated error after $m$ iterations is bounded by $\delta_m \leq \eta \frac{(3\gamma)^{m+1}-1}{3\gamma-1}$ where $\eta$ is the per-step Hamiltonian simulation error and $\gamma = O(s)$.

**Scaling summary:**

| Resource | Scaling |
|---|---|
| Qubits per copy | $O(\log n)$ |
| Total copies needed | $(16/\varepsilon^2)^m$ |
| Per-step simulation | $\text{poly}(\log n)$ |
| Integration steps | $m = t/h$ |
| Total time | $\text{poly}(\log n) \cdot \exp(O(m))$ |

---

## Comparison with prior work

| Approach | Variables | Time dependence | Nonlinearity |
|---|---|---|---|
| [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|HHL (2009)]] | $O(\log n)$ | N/A (static) | Linear only |
| **Leyton-Osborne (2008)** | $O(\log n)$ | $\exp(t/h)$ | Polynomial |
| [[Efficient Quantum Algorithm for Dissipative Nonlinear Differential Equations (Liu-Kolden-Krovi-Loureiro-Trivisa-Childs 2021) — Paper Notes|Liu-Kolden-Krovi (2021)]] | $O(\log n)$ | $\text{poly}(T)$ for $R < 1$ | Polynomial (dissipative) |
| [[Further Improving Quantum Algorithms for Nonlinear DEs via Higher-Order Methods and Rescaling (Costa-Schleich-Morales-Berry 2023) — Paper Notes|Costa-Schleich-Morales-Berry (2023)]] | $O(\log n)$ | Near-linear in $T$ | Polynomial (dissipative) |

The key advance from Leyton-Osborne to the Carleman papers: instead of directly implementing the nonlinear map (which requires consuming exponentially many copies), linearise the nonlinear ODE first (via [[Carleman Linearisation for Quantum Nonlinear ODE Solvers|Carleman linearisation]]) and then apply standard quantum linear ODE solvers.

---

## Limits / caveats

1. **Exponential in $t/h$:** The copy count $(16/\varepsilon^2)^m$ grows exponentially with integration time. For any practically interesting evolution time, this is prohibitive. This is the fundamental limitation that the Carleman approach overcame (for dissipative systems).

2. **First-order Euler:** The time-stepping accuracy is $O(h)$, so achieving error $\varepsilon$ in the ODE solution requires $h \sim \varepsilon$ and hence $m \sim t/\varepsilon$ steps. Combined with exponential copy scaling, the total cost has polynomial $\varepsilon$-dependence hidden inside the exponent.

3. **Measure-preservation assumption:** The algorithm assumes $\sum_j |z_j|^2$ is conserved by the dynamics. If violated, the postselection probability degrades further.

4. **Sparsity restrictions:** Each polynomial and each variable must appear in $O(1)$ terms. This excludes fully coupled systems like dense Navier-Stokes discretisations.

5. **Only published as preprint:** Never appeared in a journal. The analysis is correct but limited — the error bounds are loose and the algorithm was never optimised.

---

## Reusable ideas

1. [[Nonlinear Amplitude Transformation via Tensor Product and Postselection]] — the core technique for implementing a polynomial map on quantum state amplitudes: take tensor products of the state to generate all monomials, apply a sparse operator encoding the polynomial coefficients, and postselect. The success probability is $O(\varepsilon^2)$ per step, giving exponential-in-$m$ total cost, but the idea is natural and was the starting point for all subsequent nonlinear ODE quantum algorithms.

---

## References within this paper

- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|Harrow-Hassidim-Lloyd (2009)]] — cited as the inspiration; the operator $A$ and simulation approach are directly adapted from HHL
- Berry-Ahokas-Cleve-Sanders (2007) — [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes|sparse Hamiltonian simulation]], provides the simulation subroutine
- Grover-Rudolph (2002) — [[Creating Superpositions That Correspond to Efficiently Integrable Probability Distributions (Grover-Rudolph 2002) — Paper Notes|state preparation for initial conditions]]

---

## Cross-links

### Paper notes
- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes]]
- [[Efficient Quantum Algorithm for Dissipative Nonlinear Differential Equations (Liu-Kolden-Krovi-Loureiro-Trivisa-Childs 2021) — Paper Notes]]
- [[Improved Quantum Algorithms for Linear and Nonlinear Differential Equations (Krovi 2023) — Paper Notes]]
- [[Further Improving Quantum Algorithms for Nonlinear DEs via Higher-Order Methods and Rescaling (Costa-Schleich-Morales-Berry 2023) — Paper Notes]]
- [[Quantum Algorithm for Nonhomogeneous Linear PDEs (Arrazola-Kalajdzievski-Weedbrook-Lloyd 2019) — Paper Notes]]
- [[Quantum vs. Classical Algorithms for Solving the Heat Equation (Linden-Montanaro-Shao 2022) — Paper Notes]]

### Trick cards
- [[Nonlinear Amplitude Transformation via Tensor Product and Postselection]]
- [[Carleman Linearisation for Quantum Nonlinear ODE Solvers]]
