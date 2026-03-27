> **Source:** Mohsen Bagherimehrab, Luis Mantilla Calderón, Dominic W. Berry, Philipp Schleich, Mohammad Ghazi Vakili, Abdulrahman Aldossary, Jorge A. Campos Gonzalez Angulo, Christoph Gorgulla, and Alán Aspuru-Guzik, *Faster Algorithmic Quantum and Classical Simulations by Corrected Product Formulas*, arXiv:2409.08265 (2024; v3 March 2025)  
> **Links:** [arXiv](https://arxiv.org/abs/2409.08265)  
> **Tags:** #hamiltonian-simulation #product-formulas #trotter #commutators #perturbation #early-fault-tolerant #lattice-hamiltonians

---

## The computational problem

Given a time-independent Hamiltonian $H = A + \alpha B$ on a lattice, where $A$ and $B$ are both exactly simulatable (e.g., contain pairwise commuting terms or are efficiently diagonalisable), simulate $e^{-iHt}$ to spectral-norm error $\varepsilon$. The parameter $\alpha \in (0,1]$ measures how perturbed the system is: $\alpha = 1$ is generic, $\alpha \ll 1$ means $B$ is a weak perturbation.

The question: can we beat standard [[Order-Condition Cancellation in Product Formulas|Suzuki product formulas]] without leaving the product-formula framework (no ancilla qubits, no [[Block-Encoding Composition Algebra|block encodings]], no controlled evolutions)?

## What the paper does

Introduces **corrected [[product formula]]s (CPFs)** — standard [[product formula]]s with auxiliary "corrector" terms $e^{\pm C}$ injected at the boundaries. The correctors are exponentials of nested commutators $\mathrm{ad}_A^j(B)$ that systematically cancel leading error terms in the [[Finite Nested-Commutator Expansion|BCH kernel]] of the product formula.

The headline results:

- **Non-perturbed systems ($\alpha = 1$):** A CPF of order $2k$ achieves error $O(t^{2k+3})$ — two orders better than the standard PF$_{2k}$ error $O(t^{2k+1})$.
- **Perturbed systems ($\alpha \ll 1$):** A CPF of order $2k$ achieves error $O(\alpha^2 t^{2k+1})$ — a factor of $\alpha$ better than the PF$_{2k}$ error $O(\alpha t^{2k+1})$.

This matters practically: CPF error bounds match or beat the *empirical* (not just theoretical) performance of standard product formulas, validated on Heisenberg, Ising, and Hubbard models up to $n=8$ sites and $t = 10^3$.

## The algorithm / construction

### Three types of correctors

All correctors modify the kernel $K$ of a standard [[product formula]] $S(λ) = e^K$ to a better kernel $K'$ closer to the exact kernel $λH$.

**1. Symplectic corrector.** Conjugation: $e^C S(\lambda) e^{-C} = e^{K'}$ with $K' = e^{\mathrm{ad}_C} K$. The key property: for $r$ Trotter steps,

$$e^C [S(\lambda/r)]^r e^{-C} = e^C S(\lambda/r)^r e^{-C},$$

so only one application of $e^{\pm C}$ is needed at the start and end of the simulation. Cost: negligible additive overhead.

**2. Symmetric corrector.** Padding: $e^C S(\lambda) e^C = e^{K'}$ with $K' = K + 2C + \text{higher-order commutators}$. Doubles the corrector per step but can kill error terms the symplectic corrector misses.

**3. Composite corrector.** Compose both: e.g., $e^{C_{\mathrm{symp}}} e^{C_{\mathrm{sym}}} S(\lambda) e^{C_{\mathrm{sym}}} e^{-C_{\mathrm{symp}}}$.

### The key identity for PF2

The starting point for perturbed-system correctors is an exact expansion of the PF2 kernel modulo terms quadratic in $B$:

$$e^{\lambda A/2} e^{\alpha \lambda B} e^{\lambda A/2} = \exp\!\left(\lambda H + \sum_{j=1}^{\infty} \frac{B_{2j}(1/2)}{(2j)!} \lambda^{2j} \mathrm{ad}_A^{2j}(\alpha B) + O(\alpha^2)\right),$$

where $B_{2j}(1/2)$ are Bernoulli polynomials at $x = 1/2$. The leading error terms are all first-order in $\alpha$ and involve even-depth nested commutators $\mathrm{ad}_A^{2j}(B)$.

### Symplectic corrector for PF2 (perturbed systems)

The corrector

$$C^{(k)} = \alpha \sum_{j=1}^{k} \frac{B_{2j}(1/2)}{(2j)!} \lambda^{2j} \mathrm{ad}_A^{2j-1}(B)$$

removes all $O(\alpha \lambda^{2j+1})$ error terms for $j \le k$, giving a CPF2 with error $O(\alpha^2 |\lambda|^3 + \alpha |\lambda|^{2k+3})$.

### Recursive high-order CPFs

Since the CPF2 with symplectic corrector preserves time-reversal symmetry ($S_2^c(\lambda) S_2^c(-\lambda) = I$), it plugs directly into [[Suzuki-Like Recursion for Even-k Commutator Exponentials|Suzuki's recursive formula]]:

$$S_{2k}^c(\lambda) = [S_{2k-2}^c(p_k \lambda)]^2 \, S_{2k-2}^c((1-4p_k)\lambda) \, [S_{2k-2}^c(p_k \lambda)]^2$$

with $p_k = 1/(4 - 4^{1/(2k-1)})$. Error: $O(\alpha^2 |\lambda|^{2k+1})$ for perturbed systems.

For non-perturbed systems, a CPF2 with composite corrector gives $O(|\lambda|^5)$ base error, and the recursion parameter shifts to $a_k = 1/(4 - 4^{1/(2k+3)})$, yielding $O(|\lambda|^{2k+3})$.

### Yoshida-based correctors

The paper also develops correctors for [[Product Formulas for Exponentials of Commutators (Childs-Wiebe 2013) — Paper Notes|Yoshida-type product formulas]] (which use fewer exponentials than Suzuki for orders 6, 8, 10). A single symplectic corrector $C = c \lambda^{2k} \mathrm{ad}_A^{2k-1}(\alpha B)$ suffices, with the constant $c$ determined by the specific Yoshida solution's polynomial coefficients.

### Compilation of correctors

The correctors involve nested commutators $\mathrm{ad}_A^j(B)$, which are not directly exponentiable. The paper provides explicit **compilations** — decompositions of $e^C$ into products of $e^{a\lambda A}$ and $e^{b\lambda B}$:

- Key primitive: $Y(a,b) = e^{a\lambda A} e^{b\lambda B} e^{-a\lambda A} e^{-b\lambda B} e^{-a\lambda A} e^{b\lambda B} e^{a\lambda A}$ implements $e^{2ab\lambda^2 [A,B] + \cdots}$.
- Higher-order correctors via Vandermonde system: choose parameters $a_\ell = \ell + 1$ and solve a linear system for $b_\ell$ coefficients using Bernoulli polynomial values. Solutions are rational.
- Compilation cost: $10k$ exponentials for the $k$-th order corrector (Table 3 in paper).

These compilations extend the [[Product Formulas for Exponentials of Commutators (Childs-Wiebe 2013) — Paper Notes|Childs-Wiebe commutator product formulas]] and can independently synthesise unitaries from nested commutators.

## Key results

**Theorem 2 (Perturbed high-order CPF).**

$$\|e^{\lambda H} - S_{2k}^c(\lambda)\| \in O(\alpha^2 |\lambda|^{2k+1})$$

for the recursive CPF$_{2k}$ built from CPF2 with symplectic corrector. Compare PF$_{2k}$: $O(\alpha |\lambda|^{2k+1})$.

**Theorem 4 (Non-perturbed high-order CPF).**

$$\|e^{\lambda H} - S_{2k}^c(\lambda)\| \in O(|\lambda|^{2k+3})$$

for the recursive CPF$_{2k}$ built from CPF2 with composite corrector. Compare PF$_{2k}$: $O(|\lambda|^{2k+1})$.

**Proposition 3 (CPF4 with symplectic corrector, perturbed).**

$$\|e^{\lambda H} - S_4^c(\lambda)\| \in O(\alpha^2 |\lambda|^5 + \alpha |\lambda|^7)$$

with corrector $C = c \lambda^4 \mathrm{ad}_A^3(\alpha B)$ — only a single additive correction at simulation boundaries.

## Comparison with prior work

| Method | Error bound (order $2k$, perturbed) | Ancilla-free? | Extra cost over PF$_{2k}$ |
|---|---|---|---|
| Standard PF$_{2k}$ (Suzuki) | $O(\alpha t^{2k+1})$ | Yes | — |
| **CPF$_{2k}$ (this paper, symplectic)** | $O(\alpha^2 t^{2k+1})$ | Yes | Additive (boundary only) |
| **CPF$_{2k}$ (this paper, recursive)** | $O(\alpha^2 t^{2k+1})$ | Yes | Small multiplicative factor |
| THRIFT (Bosse-Childs-Derby et al. 2024) | $O(\alpha^2 t^{2k+1})$ | Yes | Multiplicative factor; needs $e^{A + \alpha H_j}$ |
| [[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes|Interaction picture]] (Low-Wiebe 2019) | Depends on $\|B\|$ only | No (ancilla + controlled-$A$) | Substantial |
| [[Randomizing Multi-Product Formulas for Hamiltonian Simulation (Faehrmann-Steudtner-Kueng-Kieferová-Eisert 2022) — Paper Notes|Randomised multi-product]] (Faehrmann et al. 2022) | Higher effective order via randomisation | Observables only | Shot overhead |

The CPF approach occupies a sweet spot: same $\alpha^2$ scaling as THRIFT but uses only exponentials of $A$ and $B$ separately (not $A + \alpha H_j$, which can be hard to implement). Symplectic correctors are particularly cheap since they cancel in intermediate steps.

## Limits / caveats

1. **Two-partition restriction.** The theory requires $H = A + \alpha B$ with both partitions exactly simulatable. Many-body Hamiltonians in arbitrary bases don't decompose this way. The paper discusses using CPFs within a divide-and-conquer scheme for general cases, but this remains speculative.

2. **Corrector compilation error.** The nested-commutator compilations introduce their own error (Table 3). For the symplectic corrector of order $k$, the compilation uses $10k$ extra exponentials and has error $O(\alpha^3 |\lambda|^3)$ — small but nonzero. At very high order, the compilation overhead could matter.

3. **Prefactor growth at high order.** Like standard Suzuki formulas, the exponential count per step grows rapidly with $2k$. The paper explicitly notes that low-order CPFs (CPF2, CPF4) are preferred in practice.

4. **Non-perturbed gains are modest.** For $\alpha = 1$, you get two extra orders — nice but not game-changing. The real power is in the perturbed regime where $\alpha \ll 1$ gives a genuine multiplicative improvement.

5. **Hardware validation is limited.** The IBM experiments use only 2–4 qubit Ising models with $r = 10$ steps. At this scale the corrector circuit depth dominates. The paper acknowledges the overhead becomes negligible only at larger $r$.

6. **Concurrent work.** THRIFT (Bosse et al., arXiv:2403.08729) achieves the same $O(\alpha^2 t^{2k+1})$ bound via interaction-frame [[product formula]]s. The $\alpha^2$ scaling is provably optimal for product-formula-type approaches (shown by Bosse et al.).

## My assessment

This is a solid, well-executed paper that improves the practical toolkit for product-formula-based simulation. The main idea — inject commutator corrections at the boundaries to cancel leading error terms — is natural in retrospect, and the connection to symplectic integrator correctors from classical astrophysics (Wisdom-Holman-Touma 1996, Laskar-Robutel 2001) is satisfying. The systematic treatment across all orders and both Suzuki and Yoshida families is thorough.

The practical impact will be largest for **early fault-tolerant simulation of lattice models** in the perturbed regime — Hubbard with weak hopping, weakly-coupled Ising, plane-wave chemistry with $\mu \ll N$ electrons. For these systems, the factor-of-$\alpha$ improvement in the error bound translates directly to fewer Trotter steps, and the symplectic corrector adds negligible overhead.

The limitation is that this stays firmly within the product-formula paradigm. If you can afford ancilla qubits and block encodings, [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|QSP]]/qubitization will eventually win asymptotically. But for resource-constrained early fault-tolerant devices, CPFs look like genuinely useful engineering.

## Reusable ideas

1. [[Symplectic Corrector Injection for Product Formulas]] — the core technique: conjugate a [[product formula]] by $e^{\pm C}$ where $C$ is a linear combination of nested commutators, to cancel leading BCH error terms at negligible additive cost
2. [[Bernoulli Polynomial Kernel Expansion for Product Formulas]] — the exact expansion of PF2 kernel modulo second-order-in-$B$ terms, using Bernoulli polynomials to identify which commutator terms to cancel
3. [[Vandermonde Compilation of Nested Commutator Exponentials]] — the explicit procedure for decomposing $e^{c \lambda^{2m} \mathrm{ad}_A^{2m-1}(B)}$ into products of $e^{a\lambda A}$ and $e^{b\lambda B}$ via a Vandermonde linear system with rational solutions

## References within this paper

- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes|Childs, Su, Tran, Wiebe, Zhu (2021)]] — the [[Trotter Commutator-Scaling Bound|commutator-scaling framework]] that CPFs improve upon
- [[Product Formulas for Exponentials of Commutators (Childs-Wiebe 2013) — Paper Notes|Childs & Wiebe (2013)]] — recursive commutator [[product formula]]s; CPF compilations extend this work
- [[Randomized Product Formulas for Hamiltonian Simulation (Quantum 2019-09-02-182) — Paper Notes|Childs, Ostrander, Su (2019)]] — randomised [[product formula]]s; alternative error-reduction strategy
- [[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes|Low & Wiebe (2019)]] — interaction-picture simulation; CPFs avoid its ancilla/controlled-evolution requirements
- Suzuki (1990, 1995) — the standard recursive product formulas and an early "hybrid" fourth-order formula that foreshadows correctors
- Yoshida (1990) — alternative higher-order product formulas with fewer exponentials
- Wisdom, Holman, Touma (1996) — symplectic correctors for classical planetary simulation (PF2 only, perturbed)
- Laskar & Robutel (2001) — generalised symplectic correctors for perturbed classical Hamiltonians; Proposition 5 originates here
- Bosse, Childs, Derby, Gambetta, Montanaro, Santos (2024) — THRIFT algorithm; same $\alpha^2$ error scaling via interaction-frame product formulas; proved $\alpha^2$ is optimal
- Morales, Costa, Pantaleoni, Burgarth, Sanders, Berry (2024) — selection and improvement of product formulae; eigenvalue error analysis
- Chen, Childs, Hafezi, Jiang, Kim, Xu (2022) — efficient commutator product formulas; compilation primitive $W(a)$ used here
- Nakaji, Bagherimehrab, Aspuru-Guzik (2024) — high-order randomised compiler for [[Hamiltonian simulation]]

## Cross-links

### Paper notes
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]]
- [[Product Formulas for Exponentials of Commutators (Childs-Wiebe 2013) — Paper Notes]]
- [[Randomized Product Formulas for Hamiltonian Simulation (Quantum 2019-09-02-182) — Paper Notes]]
- [[Randomizing Multi-Product Formulas for Hamiltonian Simulation (Faehrmann-Steudtner-Kueng-Kieferová-Eisert 2022) — Paper Notes]]
- [[Doubling the Order of Approximation via the Randomized Product Formula (Cho-Berry-Hsieh 2022) — Paper Notes]] — randomized approach to error-order boosting; doubles order from $2k+1$ to $4k+2$ but outputs a channel; complementary to the deterministic CPF approach here
- [[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes]]
- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes]]
- [[Chemical Basis of Trotter-Suzuki Errors in Quantum Chemistry Simulation (Babbush-McClean-Wecker-Aspuru-Guzik-Wiebe 2015) — Paper Notes]]
- [[qDRIFT Randomized Hamiltonian Simulation (Campbell 2018) — Paper Notes]]
- [[Bounding the Costs of Quantum Simulation of Many-Body Physics in Real Space (Kivlichan, Wiebe, Babbush, Aspuru-Guzik 2017) — Paper Notes]]
- [[Selection and Improvement of Product Formulae for Best Performance of Quantum Simulation (Morales-Costa-Pantaleoni-Burgarth-Sanders-Berry 2025) — Paper Notes]] — finds over-parameterized 8th-order [[product formula]]s ~100× more accurate; the corrector approach here and the coefficient-optimization approach there are complementary strategies for reducing product-formula error constants
- [[Faster Digital Quantum Simulation by Symmetry Protection (Tran-Su-Childs-Wiebe 2021) — Paper Notes]] — symmetry kicks as a different mechanism for practical Trotter error reduction; orthogonal to the deterministic corrector approach here
- [[Improved Fault-Tolerant Quantum Simulation of Condensed-Phase Correlated Electrons via Trotterization (Kivlichan, Gidney, Babbush et al 2020) — Paper Notes]] — concrete resource estimates for condensed-phase Hamiltonians; the CPF error improvements here directly reduce the Trotter step counts reported there
- [[Higher Order Decompositions of Ordered Operator Exponentials (Wiebe-Berry-Høyer-Sanders 2010) — Paper Notes]] — foundational product-formula theory; the BCH error structure analysed there is what the correctors here cancel

### Trick cards
- [[Symplectic Corrector Injection for Product Formulas]]
- [[Bernoulli Polynomial Kernel Expansion for Product Formulas]]
- [[Vandermonde Compilation of Nested Commutator Exponentials]]
- [[Trotter Commutator-Scaling Bound]]
- [[Order-Condition Cancellation in Product Formulas]]
- [[Finite Nested-Commutator Expansion]]
- [[Product-Formula Error Representation Switching]]
- [[Suzuki-Like Recursion for Even-k Commutator Exponentials]]
- [[Interaction-Picture Split for Product Formulas]]
- [[Permutation Averaging for Product-Formula Error Cancellation]]
- [[Vector Norm Error Analysis for Product Formulas]]
