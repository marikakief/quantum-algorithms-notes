> **Source:** Andrew M. Childs, Jiaqi Leng, Tongyang Li, Jin-Peng Liu, Chenyi Zhang, *Quantum simulation of real-space dynamics*, arXiv:2112.05027, Quantum **6**, 860 (2022)
> **Links:** [arXiv](https://arxiv.org/abs/2112.05027) · [Quantum](https://doi.org/10.22331/q-2022-11-17-860)
> **Tags:** #hamiltonian-simulation #real-space #grid #kinetic-potential #product-formulas #QSP #qubitization #first-quantized

---

## The computational problem

Simulate time evolution $e^{-iHt}$ of a quantum system described in **first-quantized real space**: $\eta$ particles in $d$ spatial dimensions, with positions discretized on a uniform grid. The Hamiltonian is

$$H = T + V, \quad T = \sum_{j=1}^\eta \frac{p_j^2}{2m}, \quad V = V(x_1, \ldots, x_\eta)$$

where the potential $V$ can be pairwise (e.g. Coulomb) or more general. Each particle's coordinate is stored on a grid with $N$ points per dimension, using $n = \lceil \log N \rceil$ qubits per coordinate; the full state lives in a Hilbert space of dimension $N^{\eta d}$.

The goal is a quantum circuit implementing $\tilde{U}$ with $\|{}\tilde{U} - e^{-iHt}\| \leq \varepsilon$, with complexity measured in gates (or queries to a potential oracle).

This is the natural setting for simulating chemical dynamics, scattering, and condensed matter directly in position space — as opposed to the second-quantized orbital-basis approaches used in most quantum chemistry papers.

---

## What the paper does

This paper provides **tight complexity analyses for all major simulation methods applied to real-space Hamiltonians**: product formulas (first through fourth order and beyond), the [[Truncated Taylor Series Simulation|truncated Taylor series / LCU method]], and [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes|qubitization]]/[[Quantum Signal Processing]] (QSP). The results are tight in the grid dimension $N$, particle number $\eta$, spatial dimension $d$, time $t$, and precision $\varepsilon$.

The key findings are: (1) **product formulas are near-optimal for real-space simulation** when error is measured in spectral norm, achieving complexity $(Nt)^{1+o(1)}$ with the right analysis; (2) **qubitization does not improve over high-order product formulas** in this setting because the block-encoding subnormalization factor scales as $\lambda \sim N$, cancelling the log-precision advantage; (3) **the spectral-method / QFT-based approach for kinetic energy** is the right lens — the kinetic energy in momentum space is diagonal, enabling efficient simulation that doesn't pay for the non-locality of $T$ in position space.

My assessment: this is a satisfying "close the book" paper on real-space simulation complexity. The analysis is tight and honest about where each method wins. The conclusion that product formulas are hard to beat here is counterintuitive given the general wisdom that qubitization dominates, but the argument is correct.

---

## The algorithm / construction

### Grid encoding

The wavefunction $\psi(x_1, \ldots, x_\eta)$ is stored on a grid with $N$ points per spatial direction. Each particle's $d$-dimensional position is encoded in $dn$ qubits ($n = \log N$ bits per dimension). Total qubit count: $\eta d n$ for positions, plus ancillas.

The Hamiltonian splits into:
- **Kinetic part $T$:** diagonal in momentum (Fourier) basis, non-local in position basis. $T = \sum_j \frac{p_j^2}{2m}$ where $p$ acts on each particle's register independently.
- **Potential part $V$:** diagonal in position basis, non-local in momentum basis.

### QFT basis switching for kinetic energy

The core structural insight ([[Basis Switching via QFT for Kinetic-Potential Splitting]]): apply the QFT per-particle to move between position and momentum bases. In momentum space, $T = \sum_j k_j^2 / 2m$ is diagonal, so $e^{-iT\delta t}$ is just a phase gate on each computational basis state. The cost is $O(\eta d n^2)$ gates per QFT, i.e., $O(\eta d \log^2 N)$ per basis switch.

### Product formulas

The paper analyzes the $p$-th order Suzuki formula $S_p(t/r)^r$ applied to $H = T + V$. The commutator-based error bound from [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]] gives

$$\|S_p(t/r)^r - e^{-iHt}\| \leq C_p \frac{\alpha_\text{comm}^{(p+1)} t^{p+1}}{r^p}$$

where $\alpha_\text{comm}^{(p+1)}$ is the nested commutator norm at level $p+1$.

**The key calculation** is bounding $\|[T, [T, \ldots [T, V]\ldots]]\|$ and related nested commutators for the real-space Hamiltonian. Because $T$ in momentum space is $\sum_j k_j^2/2m$ and $V$ is a multiplication operator, the commutator $[T, V]$ involves $\nabla V$ — the gradient of the potential. Higher commutators involve higher derivatives. The norm of the $l$-fold nested commutator of $T$ and $V$ scales as $\|V^{(l)}\|$ — the $l$-th derivative of the potential. For smooth potentials on a grid of spacing $h = L/N$, this grows as $O(N^l / L^l)$, leading to the $N$-dependent complexity.

The paper proves tight bounds showing the Trotter error scales as $O(N^{p+1}/r^p)$ for $p$-th order formulas, giving query complexity $O(N^{1+1/p} t^{1+1/p} / \varepsilon^{1/p})$ per particle (up to log factors). Taking $p \to \infty$ gives near-linear scaling $(Nt)^{1+o(1)}$.

### LCU / Taylor series

The [[Truncated Taylor Series Simulation]] method requires a block-encoding of $H$. For real-space Hamiltonians, the LCU 1-norm is $\lambda = \|T\|_\lambda + \|V\|_\lambda$. The kinetic term has $\|T\|_\lambda \sim N^2 / L^2$ (because the maximum kinetic energy on the grid scales as $k_\text{max}^2 \sim (N/L)^2$). The subnormalization by $\lambda$ means the Taylor series simulation costs $O(\lambda t \log(1/\varepsilon) / \log\log(1/\varepsilon))$ which scales as $O(N^2 t)$ — strictly worse than product formulas at any fixed order.

### Qubitization / QSP

[[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes|Qubitization]] achieves complexity $O(\lambda t + \log(1/\varepsilon))$ where $\lambda$ is the block-encoding subnormalization. For real-space Hamiltonians, $\lambda \sim N^2$, giving $O(N^2 t + \log(1/\varepsilon))$. The log-precision advantage of qubitization over product formulas is real — but the $N^2$ prefactor (vs. $N^{1+o(1)}$ for high-order product formulas) means qubitization loses in the large-$N$ regime, which is exactly the regime where real-space simulation is used.

The paper makes this comparison precise: for fixed $t$ and varying $N$, high-order product formulas win by a factor $N^{1-o(1)}$ in gate complexity.

---

## Key results

Let $N$ = grid points per dimension, $\eta$ = particles, $d$ = spatial dimension, $t$ = time, $\varepsilon$ = error. All results are for a single particle ($\eta = 1$) in $d$ dimensions unless stated; multi-particle generalizes by $N \to N^\eta$ effectively.

**Theorem (Product formula, order $p$):**
$$\text{Gates} = \tilde{O}\!\left(\left(\frac{Nt}{\varepsilon^{1/p}}\right)^{1+1/(p-1)}\right)$$
More precisely, $r = \tilde{O}(N t (N t / \varepsilon)^{1/p})$ Trotter steps suffice for $p$-th order formula.

**Corollary (Near-optimal product formula):** For any $\delta > 0$, choosing $p = \lceil 1/(2\delta)\rceil$ gives
$$\text{Gates} = \tilde{O}\!\left((Nt)^{1+\delta}\right)$$

**Theorem (LCU / Taylor series):**
$$\text{Gates} = \tilde{O}(N^2 t)$$
independent of $\varepsilon$ (up to log factors). Beats product formulas only for small $N$ and high precision.

**Theorem (Qubitization / QSP):**
$$\text{Gates} = O(N^2 t + \log(1/\varepsilon))$$

**Lower bound:** Any algorithm that accesses $V$ as a black-box oracle requires $\Omega(Nt)$ queries, showing the $(Nt)^{1+o(1)}$ complexity of high-order product formulas is near-tight.

The method comparison in terms of $N$-scaling at large $N$:

| Method | Gate complexity ($N$-dependence, fixed $t, \varepsilon$) | Optimal? |
|---|---|---|
| 1st order Trotter | $O(N^3)$ | No |
| 2nd order Trotter | $O(N^{3/2})$ | No |
| $p$-th order Trotter | $O(N^{1+1/p})$ | Near-optimal for large $p$ |
| $p \to \infty$ (any $\delta > 0$) | $O(N^{1+\delta})$ | Near-optimal |
| LCU / Taylor | $O(N^2)$ | No |
| Qubitization / QSP | $O(N^2)$ | No |
| Lower bound | $\Omega(N)$ | — |

---

## Comparison with prior work

| Paper | Method | $N$-scaling | Tight? |
|---|---|---|---|
| [[Polynomial-Time Quantum Algorithm for the Simulation of Chemical Dynamics (Kassal-Jordan-Love-Mohseni-Aspuru-Guzik 2008) — Paper Notes\|Kassal et al. (2008)]] | QFT-based Trotter (1st/2nd order) | $O(N^3)$ or $O(N^{3/2})$ | Loose |
| [[Bounding the Costs of Quantum Simulation of Many-Body Physics in Real Space (Kivlichan, Wiebe, Babbush, Aspuru-Guzik 2017) — Paper Notes\|Kivlichan et al. (2017)]] | LCU / Taylor series | $O(N^2)$ | Tight for LCU |
| [[Nearly Optimal Lattice Simulation by Product Formulas (Childs-Su 2019) — Paper Notes\|Childs-Su (2019)]] | Product formulas, lattice | $(Nt)^{1+o(1)}$ for lattice | Near-tight |
| [[High-Precision Quantum Algorithms for Partial Differential Equations (Childs-Liu-Ostrander 2021) — Paper Notes\|Childs-Liu-Ostrander (2021)]] | Spectral / QLSA for PDEs | $\text{poly}(\log(1/\varepsilon))$ | For elliptic PDEs |
| **This work** | All methods, unified | $(Nt)^{1+o(1)}$ for PF; $O(N^2)$ for QSP | Tight |

The 2017 Kivlichan et al. paper established the LCU cost correctly but did not analyze product formulas at high order or compare against QSP. The Childs-Su 2019 paper established near-optimal product formula bounds for lattice models with local interactions, but the real-space Hamiltonian is not a local lattice model (the kinetic term in position space is long-range). This paper extends that analysis to the non-local kinetic term via the momentum-basis representation.

---

## Limits / caveats

**The $N^2$ bound for LCU/QSP comes from the kinetic energy.** The kinetic operator on a grid of $N$ points per dimension has maximum eigenvalue $k_\text{max}^2 \sim N^2/L^2$ (in natural units). Qubitization pays for the full spectrum width. Product formulas avoid this because they directly exponentiate $T$ in momentum space (diagonal), paying only for the commutator structure.

**Product formulas still have large constant prefactors at high order.** The near-$N$ scaling requires $p \to \infty$ (i.e., astronomically high-order Suzuki formulas). In practice, 4th or 6th order is the reasonable limit. The practical crossover between product formulas and qubitization in terms of actual circuit depth (not just asymptotic $N$-scaling) is not resolved here.

**Coulomb singularities are excluded.** Like the qHOP paper ([[qHOP — Time-Dependent Simulation of Highly Oscillatory Dynamics (An-Fang-Lin 2022) — Paper Notes]]), the commutator bounds require $V$ to be smooth. The Coulomb potential $1/r$ has a cusp at the origin; bounding higher derivatives requires regularization (see [[Regularized Coulomb Potential (Delta Cutoff)]]) or a different analysis. Real chemistry applications require this extra step.

**Single-particle results generalize to $\eta$ particles with care.** The multi-particle case introduces pairwise potentials $V_{ij}(x_i - x_j)$. The commutator analysis extends, but the commutator norms now depend on $\eta$ and the inter-particle interaction structure.

**The lower bound is oracle-based.** The $\Omega(Nt)$ lower bound holds in the oracle model (where $V$ is a black box). Non-oracle approaches exploiting structure in $V$ (e.g., Coulomb, FMM methods like [[Quantum Simulation of Chemistry via Quantum Fast Multipole Method (Berry, Wan, Baczewski, Eklund, Tikku, Babbush 2025) — Paper Notes]]) may do better.

---

## Reusable ideas

1. [[Basis Switching via QFT for Kinetic-Potential Splitting]] — existing trick card, this paper adds the tight complexity analysis showing why QFT switching is near-optimal for real-space simulation.

2. [[Trotter Error via Nested Commutators for Real-Space Hamiltonians]] — bound the $p$-th order product formula error for $H = T + V$ in terms of higher derivatives of the potential, using the pseudo-differential structure of the $[T, V]$ commutator. The key identity: the $l$-fold commutator $[T, [T, \ldots [T, V]\ldots]]$ has norm scaling as $\|V^{(l)}\|$ — the $l$-th spatial derivative of $V$.

3. [[Subnormalization Gap Between Product Formulas and Qubitization for Real-Space Hamiltonians]] — the kinetic energy spectrum width $\lambda_T \sim N^2$ forces qubitization to pay $O(N^2 t)$, while product formulas pay only $O(N^{1+1/p} t^{1+1/p}/\varepsilon^{1/p})$ at order $p$ by working in the natural diagonal basis of $T$. This subnormalization gap means qubitization's log-precision advantage is outweighed by the $N$ overhead.

4. [[Near-Optimal Product Formula via Order Scaling]] — choosing product formula order $p$ as a function of $N, t, \varepsilon$ to minimize total gate count, achieving $(Nt)^{1+\delta}$ for any $\delta > 0$ (inherited from [[Nearly Optimal Lattice Simulation by Product Formulas (Childs-Su 2019) — Paper Notes\|Childs-Su 2019]] but applied to the real-space non-local kinetic term).

---

## References within this paper

| Reference | What it provides | Vault note? |
|---|---|---|
| Kassal, Jordan, Love, Mohseni, Aspuru-Guzik (2008) | First real-space quantum simulation algorithm (QFT + Trotter) | [[Polynomial-Time Quantum Algorithm for the Simulation of Chemical Dynamics (Kassal-Jordan-Love-Mohseni-Aspuru-Guzik 2008) — Paper Notes]] |
| Kivlichan, Wiebe, Babbush, Aspuru-Guzik (2017) | LCU-based real-space simulation, discretization analysis | [[Bounding the Costs of Quantum Simulation of Many-Body Physics in Real Space (Kivlichan, Wiebe, Babbush, Aspuru-Guzik 2017) — Paper Notes]] |
| Childs, Su (2019) | Near-optimal product formulas for lattice Hamiltonians | [[Nearly Optimal Lattice Simulation by Product Formulas (Childs-Su 2019) — Paper Notes]] |
| Childs, Su, Tran, Wiebe, Zhu (2021) | Theory of Trotter error via nested commutators | [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]] |
| Low, Chuang (2019) | Qubitization / QSP Hamiltonian simulation | [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]] |
| Childs, Liu, Ostrander (2021) | Spectral methods for quantum PDE solving | [[High-Precision Quantum Algorithms for Partial Differential Equations (Childs-Liu-Ostrander 2021) — Paper Notes]] |
| An, Fang, Lin (2022) | qHOP interaction-picture approach for real-space Schrödinger | [[qHOP — Time-Dependent Simulation of Highly Oscillatory Dynamics (An-Fang-Lin 2022) — Paper Notes]] |
| Berry, Childs, Kothari (2015) | Hamiltonian simulation with near-optimal parameter dependence | [[Hamiltonian Simulation with Nearly Optimal Dependence on All Parameters (Berry-Childs-Kothari 2015) — Paper Notes]] |
| Wiesner (1996), Zalka (1998) | Original grid-based wavefunction encoding | No vault note |
| Suzuki (1991) | Recursive high-order product formulas | No vault note — see trick cards |

---

## Cross-links

### Paper notes
- [[Bounding the Costs of Quantum Simulation of Many-Body Physics in Real Space (Kivlichan, Wiebe, Babbush, Aspuru-Guzik 2017) — Paper Notes]] — the direct predecessor on LCU-based real-space simulation; this paper tightens the product formula analysis and adds the qubitization comparison
- [[Nearly Optimal Lattice Simulation by Product Formulas (Childs-Su 2019) — Paper Notes]] — establishes the key local commutator technique for near-$(nt)$ complexity; this paper extends it to the non-local real-space kinetic term
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]] — the general commutator framework this paper applies to the real-space setting
- [[High-Precision Quantum Algorithms for Partial Differential Equations (Childs-Liu-Ostrander 2021) — Paper Notes]] — closely related Childs group paper on spectral discretization; PDE solving instead of Schrödinger dynamics, but shares the QFT basis-switching insight
- [[Polynomial-Time Quantum Algorithm for the Simulation of Chemical Dynamics (Kassal-Jordan-Love-Mohseni-Aspuru-Guzik 2008) — Paper Notes]] — establishes the QFT split-operator approach this paper analyzes and tightens
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]] — the qubitization method compared against here
- [[qHOP — Time-Dependent Simulation of Highly Oscillatory Dynamics (An-Fang-Lin 2022) — Paper Notes]] — contemporaneous work using the interaction picture to get $N$-independent commutator bounds for the same Schrödinger equation (for smooth $V$); complementary perspective
- [[Quantum Simulation of Chemistry via Quantum Fast Multipole Method (Berry, Wan, Baczewski, Eklund, Tikku, Babbush 2025) — Paper Notes]] — later work that beats the lower bound in the non-oracle model by exploiting Coulomb structure via FMM

### Trick cards
- [[Basis Switching via QFT for Kinetic-Potential Splitting]] — the central simulation primitive; this paper gives the tight cost analysis
- [[Trotter Error via Nested Commutators for Real-Space Hamiltonians]] — new card: the commutator bound specific to $H = T + V$ in real space
- [[Subnormalization Gap Between Product Formulas and Qubitization for Real-Space Hamiltonians]] — new card: why QSP doesn't dominate here
- [[Near-Optimal Product Formula via Order Scaling]] — new card: the $p$-tuning argument
- [[Split-Operator Real-Space Simulation on a Quantum Computer]] — the earlier (Kassal-based) trick card this paper provides a tighter analysis for
- [[High-Order Finite Difference Kinetic Energy]] — alternative approach to kinetic energy (LCU-based FD) from the Kivlichan paper; the QFT approach here is cleaner for the product formula setting
- [[Real-Space Grid Encoding for Many-Body Simulation]] — the position-basis encoding used throughout
- [[Regularized Coulomb Potential (Delta Cutoff)]] — needed when extending these bounds to Coulomb potentials
