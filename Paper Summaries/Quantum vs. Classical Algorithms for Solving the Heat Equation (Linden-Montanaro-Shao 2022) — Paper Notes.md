> **Source:** Noah Linden, Ashley Montanaro, Changpeng Shao, *Quantum vs. classical algorithms for solving the heat equation*, Communications in Mathematical Physics **395**, 601–641, 2022; arXiv:2004.06516
> **Links:** [arXiv](https://arxiv.org/abs/2004.06516) · [CMP](https://doi.org/10.1007/s00220-022-04442-6)
> **Tags:** #PDE #heat-equation #quantum-speedup #lower-bounds #linear-systems #QLSA #random-walk #amplitude-estimation #negative-result

---

## The computational problem

**Input:** The heat equation $\partial_t u = \alpha \nabla^2 u$ in a $d$-dimensional rectangular region $[0,L]^d$ with periodic boundary conditions and initial condition $u_0(x)$, plus a target time $t \in [0,T]$ and a spatial region $S \subseteq [0,L]^d$.

**Output:** An estimate $\tilde{H}$ of the total heat $H = \int_S u(x, t)\, dx$ to additive error $\epsilon$, with probability $\geq 0.99$.

The key design choice: the problem asks for a *classical scalar output*, not a quantum state. This puts classical and quantum algorithms on the same footing and prevents inflated speedup claims from ignoring extraction costs — exactly the issue identified by [[Quantum Algorithms and the Finite Element Method (Montanaro-Pallister 2016) — Paper Notes|Montanaro-Pallister (2016)]].

## What the paper does

Systematically compares ten algorithms (five classical, five quantum) for the heat equation, tracking all complexity parameters. The main finding: **quantum algorithms offer at most a quadratic speedup** for this problem, and **QLSA-based approaches (HHL and its descendants) are never the fastest quantum method**.

The best quantum algorithm for $d \geq 2$ applies [[Amplitude Amplification and Estimation|amplitude estimation]] to a "fast" classical random walk, achieving $\tilde{O}(\epsilon^{-1})$ vs. the classical $\tilde{O}(\epsilon^{-2})$ — exactly a quadratic speedup. For $d = 1$, classical FFT wins outright.

**My assessment:** This is exactly the kind of honest, detailed comparison the field needs. The negative result about QLSA is striking: the condition number of the discretised heat equation scales as $O(L^2/(\alpha \Delta x^2))$, and since $\Delta x$ is determined by the accuracy requirement, the condition number dependence of QLSA wipes out its polylogarithmic precision scaling. The random walk + amplitude estimation approach sidesteps this entirely by never forming a linear system. The broader lesson: for PDEs where the classical bottleneck is *not* linear algebra but rather *sampling* or *time-stepping*, QLSA-based quantum algorithms miss the point entirely. The quantum advantage, when it exists, comes from [[Amplitude Amplification and Estimation|amplitude estimation]] applied to the classical approach that's already winning.

---

## The algorithm / construction

### Discretisation

All methods use FTCS (forward time, central space) finite differences:

$$\tilde{u}(x, t + \Delta t) = \tilde{u}(x, t) + \frac{\alpha \Delta t}{\Delta x^2} \sum_{i=1}^d \left[\tilde{u}(\ldots, x_i + \Delta x, \ldots, t) + \tilde{u}(\ldots, x_i - \Delta x, \ldots, t) - 2\tilde{u}(x, t)\right]$$

Stability requires $\Delta t \leq \Delta x^2/(2d\alpha)$.

The discretisation error satisfies $\|\tilde{u} - u\|_\infty \leq \frac{\zeta \alpha d T}{L^d}\left(\frac{\alpha d \Delta t}{2} + \frac{\Delta x^2}{12}\right)$ where $\zeta$ bounds the fourth spatial derivatives.

### Classical methods analysed

1. **Conjugate gradient** for the discretised linear system: $\tilde{O}(\epsilon^{-d/2-1.5})$
2. **Time-stepping** (forward iteration): $\tilde{O}(\epsilon^{-d/2-1})$
3. **FFT diagonalisation** (rectangular regions only): $\tilde{O}(\epsilon^{-d/2})$
4. **Random walk** (connection between heat equation and Brownian motion): $\tilde{O}(\epsilon^{-3})$
5. **Fast random walk** (using efficient binomial sampling to skip steps): $\tilde{O}(\epsilon^{-2})$

### Quantum methods analysed

1. **QLSA** (best available [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|quantum linear systems algorithm]]): $\tilde{O}(\epsilon^{-d/4-2})$
2. **Coherent random walk acceleration** (quantising the classical random walk): $\tilde{O}(\epsilon^{-d/4-1.5})$
3. **Coherent diagonalisation** (quantum FFT + postselection, rectangular only): $\tilde{O}(\epsilon^{-d/4-1})$
4. **Random walk + amplitude estimation**: $\tilde{O}(\epsilon^{-2})$
5. **Fast random walk + amplitude estimation**: $\tilde{O}(\epsilon^{-1})$

### Why QLSA loses

The condition number of the discretised system is $\kappa = O(L^2/(\alpha \Delta x^2))$. To achieve accuracy $\epsilon$, we need $\Delta x = O((\epsilon L^d/(\zeta \alpha d T))^{1/2})$, so $\kappa = O(\zeta d T L^{d-2} / \epsilon)$. The QLSA cost includes a factor of $\kappa$ (or $\kappa$ times polylog factors), and also requires $O(1/\|u\|)$ overhead to extract the classical answer from the quantum state. Together these produce costs that exceed classical methods.

### Why amplitude estimation wins

The heat equation has a probabilistic interpretation: $u(x,t)$ is the expected density of a random walk started at $x$, run for time $t$. To estimate $\int_S u\, dx$, the classical approach runs many random walks and averages — this is Monte Carlo with variance $O(1/N)$ for $N$ walks, giving $O(1/\epsilon^2)$ cost.

[[Amplitude Amplification and Estimation|Amplitude estimation]] provides a quadratic speedup for this Monte Carlo sampling: $O(1/\epsilon)$ quantum walk invocations suffice. The "fast" variant uses binomial sampling to jump the walk forward in large steps, achieving $\tilde{O}(1/\epsilon)$ total cost.

---

## Key results

**Table of $\epsilon$-scaling (other parameters fixed):**

| Method | $d=1$ | $d=2$ | $d=3$ | $d \geq 4$ |
|---|---|---|---|---|
| Best classical (general region) | $\tilde{O}(\epsilon^{-1.5})$ | $\tilde{O}(\epsilon^{-2})$ | $\tilde{O}(\epsilon^{-2.5})$ | $\tilde{O}(\epsilon^{-d/2-1})$ |
| Best classical (rectangular) | $\tilde{O}(\epsilon^{-0.5})$ | $\tilde{O}(\epsilon^{-1})$ | $\tilde{O}(\epsilon^{-1.5})$ | $\tilde{O}(\epsilon^{-d/2})$ |
| Best quantum (general region) | $\tilde{O}(\epsilon^{-1.75})$ | $\tilde{O}(\epsilon^{-2})$ | $\tilde{O}(\epsilon^{-2})$ | $\tilde{O}(\epsilon^{-2})$ |
| Best quantum (rectangular) | $\tilde{O}(\epsilon^{-1})$ | $\tilde{O}(\epsilon^{-1})$ | $\tilde{O}(\epsilon^{-1})$ | $\tilde{O}(\epsilon^{-1})$ |
| QLSA (quantum) | $\tilde{O}(\epsilon^{-2.5})$ | $\tilde{O}(\epsilon^{-2.5})$ | $\tilde{O}(\epsilon^{-2.75})$ | $\tilde{O}(\epsilon^{-d/4-2})$ |

Key observations:
- For $d = 1$: classical FFT ($\tilde{O}(\epsilon^{-0.5})$) beats all quantum methods
- For $d \geq 2$: quantum advantage is at most quadratic ($\tilde{O}(\epsilon^{-1})$ vs $\tilde{O}(\epsilon^{-2})$)
- QLSA is **never** the best quantum method — always outperformed by random walk + amplitude estimation
- The quantum speedup for general regions is from $\tilde{O}(\epsilon^{-3})$ to $\tilde{O}(\epsilon^{-2})$ (dimension-independent)

---

## Comparison with prior work

| Paper | Claim | This paper's finding |
|---|---|---|
| Clader-Jacobs-Sprouse (2013) | Exponential quantum speedup for EM scattering via FEM | Speedup is at most polynomial (confirmed [[Quantum Algorithms and the Finite Element Method (Montanaro-Pallister 2016) — Paper Notes|Montanaro-Pallister]]) |
| [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes|Berry-Childs-Ostrander-Wang (2017)]] | Efficient quantum algorithm for linear ODEs | Applied to heat equation, but QLSA approach is never competitive |
| [[Quantum Algorithm for Nonhomogeneous Linear PDEs (Arrazola-Kalajdzievski-Weedbrook-Lloyd 2019) — Paper Notes|Arrazola et al. (2019)]] | CV algorithm for inhomogeneous linear PDEs | Same QLSA paradigm; same condition number issues apply |

---

## Limits / caveats

1. **Specific to the heat equation:** The results are rigorously proved for this particular PDE. The authors suggest the findings may be representative, but the analysis would need to be repeated for other PDEs (wave equation, Schrödinger equation, etc.).

2. **Rectangular regions only for best algorithms:** The fastest classical (FFT) and quantum (fast random walk + AE) methods require rectangular geometry with periodic boundaries. General regions allow only weaker methods.

3. **FTCS discretisation only:** Other discretisation schemes (spectral methods, finite elements, adaptive meshes) could change the picture. The paper acknowledges this but argues FTCS is representative for complexity comparisons.

4. **Smooth initial conditions assumed:** The bounds assume the solution has bounded fourth derivatives. Rough initial conditions or singularities could change the scaling.

5. **The quadratic speedup is real but may not be practical:** A factor-of-$\epsilon^{-1}$ improvement is genuine but modest. Whether this translates to practical advantage depends on the constant factors and the overhead of running a quantum computer.

---

## Reusable ideas

1. [[Amplitude Estimation Applied to Random Walk PDE Solvers]] — the key algorithmic insight: when a PDE has a probabilistic interpretation (Feynman-Kac formula, random walk representation), [[Amplitude Amplification and Estimation|amplitude estimation]] applied to the random walk gives a quadratic speedup for estimating functionals of the solution. This completely bypasses the linear systems formulation and its condition number issues.

2. **End-to-end PDE complexity benchmarking** — the paper's methodological contribution: fix a specific PDE, a specific output quantity (heat in a region), and compare all algorithms on the same footing. This avoids the common trap of comparing quantum algorithms for "solving the linear system" against classical algorithms for "computing the full solution."

---

## References within this paper

- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|HHL (2009)]] — the QLSA baseline
- [[Quantum Algorithms and the Finite Element Method (Montanaro-Pallister 2016) — Paper Notes|Montanaro-Pallister (2016)]] — inspired this work; showed FEM speedup is polynomial, not exponential
- [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes|Berry-Childs-Ostrander-Wang (2017)]] — quantum linear ODE algorithm, applied here to the heat equation but found non-competitive
- [[Quantum Speedup of Monte Carlo Methods (Montanaro 2015) — Paper Notes|Montanaro (2015)]] — quantum Monte Carlo speedup framework underlying the amplitude estimation approach
- Scherer et al. (2017) — concrete resource analysis for QLSA applied to electromagnetic scattering; this paper's analysis confirms and extends that pessimistic assessment
- An, Linden, Liu, Montanaro, Shao, Wang (2021) — MLMC for SDEs (arXiv:2012.06283), extends the random walk + quantum approach to stochastic DEs

---

## Cross-links

### Paper notes
- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes]]
- [[Quantum Algorithms and the Finite Element Method (Montanaro-Pallister 2016) — Paper Notes]]
- [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes]]
- [[Quantum Speedup of Monte Carlo Methods (Montanaro 2015) — Paper Notes]]
- [[Quantum Algorithm for Nonhomogeneous Linear PDEs (Arrazola-Kalajdzievski-Weedbrook-Lloyd 2019) — Paper Notes]]
- [[A Quantum Algorithm to Solve Nonlinear Differential Equations (Leyton-Osborne 2008) — Paper Notes]]
- [[Further Improving Quantum Algorithms for Nonlinear DEs via Higher-Order Methods and Rescaling (Costa-Schleich-Morales-Berry 2023) — Paper Notes]]
- [[Efficient Quantum Algorithm for Dissipative Nonlinear Differential Equations (Liu-Kolden-Krovi-Loureiro-Trivisa-Childs 2021) — Paper Notes]]
- [[Improved Quantum Algorithms for Linear and Nonlinear Differential Equations (Krovi 2023) — Paper Notes]]

### Trick cards
- [[Amplitude Estimation Applied to Random Walk PDE Solvers]]

- [[Carleman Linearisation for Quantum Nonlinear ODE Solvers]]
