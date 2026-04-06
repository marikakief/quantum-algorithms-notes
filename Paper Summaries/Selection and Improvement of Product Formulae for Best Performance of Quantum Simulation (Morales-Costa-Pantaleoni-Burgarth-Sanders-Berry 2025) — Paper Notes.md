> **Source:** Mauro E. S. Morales, Pedro C. S. Costa, Giacomo Pantaleoni, Daniel K. Burgarth, Yuval R. Sanders, Dominic W. Berry, *Selection and improvement of [[Product Formulas]]e for best performance of quantum simulation*, arXiv:2210.15817, Quantum Information & Computation **25**(1), 1 (2025)
> **Links:** [arXiv](https://arxiv.org/abs/2210.15817) · [QIC](https://doi.org/10.26421/QIC25.1-2-1)
> **Tags:** #hamiltonian-simulation #product-formulas #trotter #suzuki #symplectic-integrators #numerical-optimization #quantum-chemistry

---

## The computational problem

Given a Hamiltonian $H = \sum_{j=1}^J H_j$, simulate $e^{-iHt}$ to error $\varepsilon$ using [[Order-Condition Cancellation in Product Formulas|product formulas]] — sequences of exponentials $\prod e^{c_j H_{\sigma(j)} t}$. The cost metric is the total number of exponentials in the product. Higher-order [[Product Formulas]]s reduce the number of time steps needed, but the coefficients and structure of the formula determine the constant factor in the error, which can vary by orders of magnitude between formulas of the same order.

The paper asks: among all [[Product Formulas]]s of a given order and length, which ones actually perform best for quantum simulation?

---

## What the paper does

Finds new 8th-order [[Product Formulas]]s that are ~100× more accurate than any previously known (without processing) and ~300× more accurate (with [[Processed Product Formula Kernel-Processor Decomposition|processing]]). Establishes that 8th order is optimal over the parameter range $T/\varepsilon \sim 10^7$ to $10^{16}$ relevant to quantum chemistry, making 10th order unnecessary for any realistic quantum simulation. Also introduces a principled framework for comparing [[Product Formulas]]s of different orders and lengths using eigenvalue error rather than spectral-norm error.

This is a paper that changes the practical landscape of [[Hamiltonian simulation]] via product formulas. The improvement factors are large enough to matter for real resource estimates. Yuval and Dominic are coauthors — directly relevant to Marika's environment.

---

## The algorithm / construction

### Background: [[Product Formulas]] structure

A $k$-th order [[Product Formulas]] satisfies $S_k(t) = e^{-iHt} + O(t^{k+1})$. The standard construction uses [[Suzuki Order as a Tunable Knob for Time Scaling|Suzuki's fractal recursion]]: $S_{2\kappa}(t) = S_{2\kappa-2}(s_\kappa t)^2 \cdot S_{2\kappa-2}((1-4s_\kappa)t) \cdot S_{2\kappa-2}(s_\kappa t)^2$ with $s_\kappa = 1/(4-4^{1/(2\kappa-1)})$. This yields exponential blowup in the number of terms: $2(J-1)5^{\kappa-1}+1$ for the five-fold recursion.

### Yoshida's non-fractal method

Instead of recursive composition, [[Higher Order Decompositions of Ordered Operator Exponentials (Wiebe-Berry-Høyer-Sanders 2010) — Paper Notes|Yoshida's approach]] writes:
$$
S^{(m)}(t) = \prod_{j=1}^m S_2(w_j t) \cdot S_2(w_0 t) \cdot \prod_{j=m}^1 S_2(w_j t)
$$
with $M = 2m+1$ stages of $S_2$. The coefficients $w_0, \ldots, w_m$ must satisfy a system of nonlinear polynomial equations derived from the symmetric BCH expansion. For 8th order, the minimum is $m=7$ (15 stages) with 7 equations; for 10th order, $m=15$ (31 stages) with 15 equations.

The key insight: **using $m$ larger than the minimum** gives more free parameters than equations, creating freedom to optimize for low error. This paper pushes to $m=10$ for 8th order (21 stages, 10 unknowns for ~7 equations).

### Numerical search procedure

1. **Solve from random starts**: Use Levenberg-Marquardt (Matlab's `fsolve`) with $\vec{w}$ drawn from $\mathcal{N}(0, 0.6^2)$ for 8th order, $\mathcal{N}(0, 0.9^2)$ for 10th order.
2. **Filter by error testing**: For each solution, compute the error on 1024 random $64 \times 64$ Hermitian matrices with unit-norm components.
3. **Refine via Taylor expansion**: Use the Taylor-series method (matching operator products order by order) to further reduce error — adjust the 8th-order formula by minimizing the 9th-order Taylor error, then re-solve for 8th order. This iterative refinement often produces lower-error solutions.

### [[Processed Product Formula Kernel-Processor Decomposition|Processed product formulas]]

A processed formula has the form $S_k = P \Sigma P^{-1}$ where $\Sigma$ is a kernel and $P$ is a processor. For long-time simulation $S_k^r = P \Sigma^r P^{-1}$, so the cost per step is dominated by the kernel length. The kernel satisfies fewer order conditions than a complete symmetric formula (missing the $B_{5,m} = 0$ condition at 6th order, for instance), allowing either shorter kernels or lower error at the same length.

The paper finds an 8th-order kernel with $m=8$ (17 stages) whose eigenvalue error constant $\zeta = 8.1 \times 10^{-10}$ is comparable to their best non-processed formula with $m=10$ (21 stages), but the shorter length gives better overall performance.

---

## Key results

### New 8th-order [[Product Formulas]]s

| Formula | Stages ($M$) | Processing | Eigenvalue error $\zeta$ | Performance $M\zeta^{1/k}$ |
|---|---|---|---|---|
| Best prior (SS8s19, Sofroniou-Spaletta 2005) | 19 | No | $5.3 \times 10^{-8}$ | 2.34 |
| **Y8m10b** (this work) | 21 | No | $5.4 \times 10^{-10}$ | **1.46** |
| Best prior processed (PP8s13, Blanes-Casas-Murua 2006) | 13 | Yes | $6.5 \times 10^{-7}$ | 2.19 |
| **YP8m8** (this work) | 17 | Yes | $8.1 \times 10^{-10}$ | **1.24** |

The processed formula YP8m8 has the best overall performance metric $M\zeta^{1/k}$ across all orders and all formulas tested.

### Fair comparison metric

For order-$k$ formulas, the total number of exponentials for simulation time $T$ to error $\varepsilon$ is:
$$
N_{\mathrm{exp}} \approx 2(J-1)M \left(\frac{\zeta T}{\varepsilon}\right)^{1/k} T
$$

So the figure of merit for comparing same-order formulas is $M\zeta^{1/k}$ — not just the error constant $\zeta$ alone.

### Order thresholds

For [[Product Formulas]]s of orders $k_1 < k_2$, the ratio $T/\varepsilon$ beyond which order $k_2$ becomes preferable is:
$$
\frac{T}{\varepsilon} = \left(\frac{M_2 \zeta_2^{1/k_2}}{M_1 \zeta_1^{1/k_1}}\right)^{\frac{1}{1/k_1 - 1/k_2}}
$$

| Comparison | Asymptotic threshold $T/\varepsilon$ | Non-asymptotic threshold |
|---|---|---|
| 6th → 8th order | ~68,000 | ~$3.2 \times 10^7$ |
| 8th → 10th order | ~$9.3 \times 10^{15}$ | Similar |

The 8th→10th threshold is absurdly large: at $10^4$ Toffolis per exponential, a simulation needing 10th order would take millions of years. So **10th order [[Product Formulas]]s are irrelevant for quantum computing**, despite the exceptional 10th-order solution SS10s35 from Sofroniou-Spaletta.

### Eigenvalue error vs. spectral-norm error

The paper argues (and demonstrates) that eigenvalue error is the correct metric for long-time simulation. Key argument: decompose $\tilde{U} = V D V^\dagger$ where $D$ captures eigenvalue error and $V$ captures basis error. Over $r$ steps:
$$
\|\tilde{U}^r - U^r\| \leq 2\|V - I\| + r\|D - U\|
$$

The eigenvalue error accumulates linearly with $r$; the basis error does not. For large $r$, eigenvalue error dominates.

### Fermionic Hamiltonians

For $H = \sum_{pq} \tau_{pq} a_p^\dagger a_q + \sum_{pq} \nu_{pq} a_p^\dagger a_p a_q^\dagger a_q$, the error scales (per [[Recursive Trotterization with L1-Norm Reduction and Low-Rank Decomposition (2022) — Paper Notes|Low-Su-Tong-Tran 2023]]) as:
$$
\|S_k(t) - e^{-iHt}\|_{W_\eta} = O\!\left(\omega (\|\tau\|_1 + \|\nu\|_{1,[\eta]})^{k-1} \|\tau\|_1 \|\nu\|_{1,[\eta]} \eta \cdot t^{k+1}\right)
$$

The constant $\omega$ is independent of system size. The paper verifies this for $d=4$ and $d=6$ orbitals and finds the relative performance of formulas is unchanged from random matrices. For quantum chemistry parameters (e.g., $\eta \approx 100$, $N/\Omega \sim 10^3$–$10^9$), the relevant $T/\varepsilon$ always falls in the range where 8th order is optimal.

### Transverse-field Ising model test

For an 8-qubit chain with time step $t=1$ (not small!), Y8m10b achieves eigenvalue error $2.7 \times 10^{-12}$ per step, compared to $\sim 10^{-10}$ for the best prior formula. The error remains far smaller over 1000 steps.

---

## Comparison with prior work

| Aspect | Prior state of the art | This paper |
|---|---|---|
| 8th-order non-processed | SS8s19 (Sofroniou-Spaletta 2005): $\zeta = 5.3 \times 10^{-8}$ | Y8m10b: $\zeta = 5.4 \times 10^{-10}$ (~100× better) |
| 8th-order processed | PP8s13 (Blanes-Casas-Murua 2006): $\zeta = 6.5 \times 10^{-7}$ | YP8m8: $\zeta = 8.1 \times 10^{-10}$ (~300× better, shorter kernel too) |
| 10th-order | SS10s35 (2005): $\zeta = 3.1 \times 10^{-11}$ — exceptionally good | Y10m17: $\zeta = 6.1 \times 10^{-11}$ (2× worse, but irrelevant — 10th order not useful) |
| Error metric | Spectral-norm error | Eigenvalue error (more relevant for long-time simulation) |
| Comparison method | Ad hoc | Principled $M\zeta^{1/k}$ metric with non-asymptotic threshold analysis |

The paper also tests the recently proposed formulas of [[Faster Quantum Simulation by Randomization (Childs-Ostrander-Su 2019) — Paper Notes|Childs-Ostrander-Su (2019)]] and Ostmeyer (2023), finding them competitive at 4th and 6th order but not at 8th.

---

## Limits / caveats

1. **Two-term splitting only tested numerically.** The formulas are derived for $H = A + B$ and proven to work for arbitrary sums by the $S_2$-product construction, but the numerical comparison uses two-term Hamiltonians. Structure in many-term splittings could change the landscape.

2. **Non-asymptotic regime matters.** The asymptotic threshold formula (Eq. 48) can be wildly wrong when time steps are large. The paper handles this with numerical threshold computation, but the 6th→8th threshold depends sensitively on the specific formulas and Hamiltonians used.

3. **Norm-based selection misleading.** The $\ell_1$ and $\ell_{k+1}$ norms of coefficients, used by prior work (Blanes-Casas-Murua 2006, Sofroniou-Spaletta 2005) to select formulas, do not predict actual performance. Solutions with lower error have larger norms. This means prior heuristics for identifying good formulas were systematically suboptimal.

4. **Randomisation caveats.** The eigenvalue-vs-basis-error decomposition implies that [[qDRIFT Randomized Hamiltonian Simulation (Campbell 2018) — Paper Notes|randomised product formula]] techniques may provide less improvement than anticipated: if successive steps aren't identical, the basis error cancellation breaks.

5. **10th order search incomplete.** Almost every new 10th-order solution found was distinct from previous ones, suggesting the solution space is enormous and unexplored. Better 10th-order formulas likely exist, but won't matter for quantum computing due to the threshold.

---

## Reusable ideas

1. [[Processed Product Formula Kernel-Processor Decomposition]] — Decompose a [[Product Formulas]] as $P\Sigma P^{-1}$ to reduce the effective per-step cost by solving fewer order conditions for the kernel.

2. [[Eigenvalue Error as the Correct Product Formula Metric]] — For long-time simulation, eigenvalue error in a single step dominates the total spectral-norm error; basis error cancels across steps.

3. [[Over-Parameterized Product Formula Search]] — Use more stages than the minimum required for a given order to create free parameters for numerical optimization of the error constant.

4. [[Cross-Order Product Formula Threshold]] — Compare formulas of different orders via the $T/\varepsilon$ threshold at which the higher-order formula becomes preferable, accounting for both error constants and formula lengths.

---

## References within this paper

Key citations and their roles:

- [[Higher Order Decompositions of Ordered Operator Exponentials (Wiebe-Berry-Høyer-Sanders 2010) — Paper Notes|Wiebe, Berry, Høyer, Sanders (2010)]] — Extended Suzuki [[Product Formulas]]s to ordered exponentials; this paper builds on the same Yoshida construction
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes|Childs, Su, Tran, Wiebe, Zhu (2021)]] — Showed practical [[Product Formulas]] error is governed by commutators, motivating the search for better constant factors
- [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes|Berry, Ahokas, Cleve, Sanders (2007)]] — First use of high-order Suzuki formulas for quantum simulation
- [[Chemical Basis of Trotter-Suzuki Errors in Quantum Chemistry Simulation (Babbush-McClean-Wecker-Aspuru-Guzik-Wiebe 2015) — Paper Notes|Babbush, McClean, Wecker, Aspuru-Guzik, Wiebe (2015)]] — Showed norm-based Trotter bounds are loose by $10^{16}$ for chemistry
- Yoshida (1990) — Original non-fractal method for deriving product formulas via BCH expansion
- Suzuki (1990, 1991) — Fractal recursions for generating arbitrary-order product formulas
- Sofroniou and Spaletta (2005) — Best prior 8th and 10th order product formulas
- Blanes, Casas, Murua (2006) — Best prior processed product formulas; introduced kernel-processor framework
- Blanes, Casas, Murua (2024) — Recent survey of product formulas / symplectic integrators (Acta Numerica)
- McLachlan (1995) — Optimal 8th-order product formulas at 15 and 17 stages
- Kahan and Li (1997) — Confirmed McLachlan's 8th-order results
- [[Recursive Trotterization with L1-Norm Reduction and Low-Rank Decomposition (2022) — Paper Notes|Low, Su, Tong, Tran (2023)]] — Error bound for fermionic Hamiltonians (Theorem 4) used for chemistry threshold analysis
- [[Quantum Computation of Stopping Power for Inertial Fusion Target Design (Rubin, Berry, Babbush et al 2023) — Paper Notes|Rubin, Berry, Babbush et al. (2024)]] — Used bespoke 8th-order product formulas for stopping power simulation
- [[Quantum Simulation of Exact Electron Dynamics Can Be More Efficient Than Classical Mean-Field Methods (Babbush, Huggins, Berry et al 2023) — Paper Notes|Babbush, Huggins, Berry et al. (2023)]] — First-quantized high-order Trotter for real-time dynamics
- Ostmeyer (2023) — Recent 4th-order formula tested here

---

## Cross-links

### Paper notes
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]] — tight error analysis that this paper's formulas benefit from
- [[Higher Order Decompositions of Ordered Operator Exponentials (Wiebe-Berry-Høyer-Sanders 2010) — Paper Notes]] — Yoshida/Suzuki framework extended to time-dependent case; same construction used here
- [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes]] — first systematic use of high-order [[Product Formulas]]s in quantum simulation
- [[Chemical Basis of Trotter-Suzuki Errors in Quantum Chemistry Simulation (Babbush-McClean-Wecker-Aspuru-Guzik-Wiebe 2015) — Paper Notes]] — showed norm bounds dramatically overestimate Trotter error
- [[Product Formulas for Exponentials of Commutators (Childs-Wiebe 2013) — Paper Notes]] — related [[Product Formulas]] constructions
- [[Faster Quantum Simulation by Randomization (Childs-Ostrander-Su 2019) — Paper Notes]] — randomisation may be less helpful than expected given eigenvalue error analysis
- [[Randomizing Multi-Product Formulas for Hamiltonian Simulation (Faehrmann-Steudtner-Kueng-Kieferová-Eisert 2022) — Paper Notes]] — multi-product approaches; different error trade-offs
- [[qDRIFT Randomized Hamiltonian Simulation (Campbell 2018) — Paper Notes]] — randomised approach whose improvement may be limited by basis error issues
- [[Simulating Quantum Dynamics on a Quantum Computer (Wiebe-Berry-Høyer-Sanders 2011) — Paper Notes]] — earlier work from overlapping author set on product formulas
- [[Quantum Computation of Stopping Power for Inertial Fusion Target Design (Rubin, Berry, Babbush et al 2023) — Paper Notes]] — directly uses bespoke 8th-order product formulas
- [[Recursive Trotterization with L1-Norm Reduction and Low-Rank Decomposition (2022) — Paper Notes]] — fermionic Trotter error bound used in this paper's chemistry analysis
- [[Faster Algorithmic Quantum and Classical Simulations by Corrected Product Formulas (Bagherimehrab-Berry-Schleich-Aldossary-Angulo-Aspuru-Guzik 2024) — Paper Notes]] — complementary approach to reducing product-formula error constants: injecting BCH correctors rather than optimizing formula coefficients; both papers target the constant-factor gap between norm bounds and actual Trotter costs
- [[Doubling the Order of Approximation via the Randomized Product Formula (Cho-Berry-Hsieh 2022) — Paper Notes]] — randomized order-doubling via symmetric formula corrections; uses the same Suzuki structure this paper optimizes
- [[Faster Digital Quantum Simulation by Symmetry Protection (Tran-Su-Childs-Wiebe 2021) — Paper Notes]] — symmetry kicks for error reduction; complementary practical technique for systems with accessible symmetries
- [[Improved Fault-Tolerant Quantum Simulation of Condensed-Phase Correlated Electrons via Trotterization (Kivlichan, Gidney, Babbush et al 2020) — Paper Notes]] — fault-tolerant Trotter resource estimates that the improved formulas found here would directly reduce

### Trick cards
- [[Order-Condition Cancellation in Product Formulas]] — the order conditions this paper solves
- [[Suzuki Order as a Tunable Knob for Time Scaling]] — the fractal approach this paper improves upon
- [[Suzuki-Like Recursion for Even-k Commutator Exponentials]] — related recursive construction
- [[Trotter Commutator-Scaling Bound]] — commutator-based error that motivates constant-factor optimization
- [[Smoothness-Order Saturation for Product Formulas]] — smoothness constraints on achievable order
- [[Processed Product Formula Kernel-Processor Decomposition]] — new trick card
- [[Eigenvalue Error as the Correct Product Formula Metric]] — new trick card
- [[Over-Parameterized Product Formula Search]] — new trick card
- [[Cross-Order Product Formula Threshold]] — new trick card
