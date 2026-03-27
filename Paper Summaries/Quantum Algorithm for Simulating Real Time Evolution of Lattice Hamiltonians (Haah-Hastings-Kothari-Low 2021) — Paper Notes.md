> **Source:** Jeongwan Haah, Matthew B. Hastings, Robin Kothari, Guang Hao Low, *Quantum Algorithm for Simulating Real Time Evolution of Lattice Hamiltonians*, arXiv:1801.03922, SIAM Journal on Computing (2021). Preliminary version in IEEE FOCS 2018, pp. 350–360.
> **Links:** [arXiv](https://arxiv.org/abs/1801.03922) · [FOCS 2018](https://doi.org/10.1109/FOCS.2018.00041)
> **Tags:** #hamiltonian-simulation #lattice #Lieb-Robinson #gate-complexity #lower-bound #product-formula #geometrically-local #time-dependent

---

## The computational problem

Simulate the time evolution $e^{-iHT}$ of a **geometrically local** lattice Hamiltonian $H = \sum_{X \subseteq \Lambda} h_X$ on $n$ qubits arranged in a $D$-dimensional lattice $\Lambda \subset \mathbb{R}^D$, where each term $h_X$ acts on qubits within a unit ball ($\mathrm{diam}(X) \leq 1$) and $\|h_X\| \leq 1$. The goal is to construct a quantum circuit approximating the time-evolution unitary to spectral-norm error $\epsilon$, using as few 2-qubit gates as possible.

The natural target is $O(nT)$ gates — matching the spacetime volume of the physical system — with polylogarithmic overhead in $1/\epsilon$. Prior to this work, no algorithm achieved this:

| Prior method | Gate cost |
|---|---|
| Sparse simulation ([[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes\|QSP]], [[Simulating Hamiltonian Dynamics with a Truncated Taylor Series (Berry-Childs-Cleve-Kothari-Somma 2015) — Paper Notes\|Taylor series]]) | $O(n^2 T \operatorname{polylog}(nT/\epsilon))$ — the oracle costs $O(n)$ gates |
| High-order [[Product formula\|Suzuki formulas]] | $O(nT(nT/\epsilon)^\delta)$ for any $\delta > 0$ — no polylog in $1/\epsilon$ |

The extra factor of $n$ in sparse simulation comes from implementing the sparse-access oracle for a geometrically local Hamiltonian, which requires $O(n)$ gates per query because the oracle must be able to address any row of the $2^n \times 2^n$ matrix. Geometric locality should let you avoid this.

---

## What the paper does

Achieves the optimal gate count $O(nT \operatorname{polylog}(nT/\epsilon))$ with depth $O(T \operatorname{polylog}(nT/\epsilon))$ using only geometrically local 2-qubit gates and $O(1)$ ancillas per system qubit. Also proves a matching lower bound: any simulation of a 1D piecewise-constant bounded local Hamiltonian requires $\tilde{\Omega}(nT)$ gates.

This is the first algorithm to achieve gate cost quasilinear in the spacetime volume $nT$ with polylogarithmic precision dependence, and the first nontrivial gate-complexity lower bound (as opposed to query-complexity lower bound) for Hamiltonian simulation.

---

## The algorithm / construction

### Core idea: Lieb-Robinson decomposition

The algorithm exploits the fact that in geometrically local systems, information propagates at bounded speed (the [[Lieb-Robinson bound]]). For time evolution over a short period $t = O(1)$, a qubit at position $x$ is essentially unaffected by terms far away — the influence decays exponentially with distance beyond the "light cone" $v_{\mathrm{LR}} t$.

This means the full time-evolution unitary $e^{-itH}$ can be decomposed into a product of **small** unitaries, each acting on $O(\ell^D)$ qubits where $\ell = O(\log(nT/\epsilon))$, with exponentially small error in $\ell$.

### The decomposition (1D case)

Split the chain into two overlapping regions $A$ and $Y \cup B$ where $Y$ is the overlap of size $\ell$:

$$e^{-itH} \approx e^{-itH_A} \cdot e^{+itH_Y} \cdot e^{-itH_{Y \cup B}}$$

where $H_A = \sum_{X \subseteq A} h_X$, etc. The error is $O(e^{-\mu\ell})$ by the Lieb-Robinson bound (Lemma 6), where $\mu > 0$ is a constant depending on the locality structure.

The key identity (Lemma 6): for disjoint regions $A, B, C$,

$$\left\| U_t^{H_{A \cup B}} (U_t^{H_B})^\dagger U_t^{H_{B \cup C}} - U_t^{H_{A \cup B \cup C}} \right\| \leq O\left(e^{-\mu\, \mathrm{dist}(A,C)}\right) \sum_{X: \mathrm{bd}(AB,C)} \|h_X\|$$

This follows from combining:
1. **Lemma 4** (perturbation bound): if $\|A_s - B_s\| \leq \delta$ for all $s \in [0,t]$, then $\|U_t^A - U_t^B\| \leq t\delta$.
2. **Lemma 5** (Lieb-Robinson bound): $\|(U_t^H)^\dagger O_X U_t^H - (U_t^{H_\Omega})^\dagger O_X U_t^{H_\Omega}\| \leq |X|\|O_X\| \frac{(2\zeta_0|t|)^\ell}{\ell!}$ where $\ell = \lfloor\mathrm{dist}(X, \Lambda \setminus \Omega)\rfloor$ and $\zeta_0 = \max_p \sum_{Z \ni p} |Z|\|h_Z\|$.

### Recursive application

Apply the decomposition recursively to the large blocks $A$ and $B$ until all blocks have size $O(\ell^D)$. For a 1D chain of length $L$, this produces $O(L/\ell)$ blocks. The pattern (see Figure 1 in the paper) alternates forward and backward time evolution, with overlapping regions providing the "glue."

For $D$ dimensions, the decomposition proceeds in $D$ rounds:
1. First round: cut into $O(L/\ell)$ slabs of size $L^{D-1} \times 2\ell$ (codimension-1 hyperplanes). Error: $O(e^{-\mu\ell} L^D/\ell)$.
2. Second round: cut each slab into blocks of size $L^{D-2} \times 2\ell \times 2\ell$.
3. After $D$ rounds: $O((L/\ell)^D)$ blocks of size $O(\ell^D)$ each. Total decomposition error: $O(D e^{-\mu\ell} L^D/\ell)$.

### Full algorithm

1. Divide total time $T$ into $O(T)$ steps of $O(1)$ duration.
2. For each time step, apply the Lieb-Robinson decomposition to get $O(n/\ell^D)$ blocks of $O(\ell^D)$ qubits.
3. Simulate each block using any Hamiltonian simulation algorithm with polylog$(1/\epsilon)$ dependence — e.g., [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|QSP]], [[Simulating Hamiltonian Dynamics with a Truncated Taylor Series (Berry-Childs-Cleve-Kothari-Somma 2015) — Paper Notes|truncated Taylor series]], or [[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes|Dyson series]].
4. Choose $\ell = O(\log(nT/\epsilon))$ so the decomposition error is $\leq \epsilon$.

Total: $m = O(Tn/\ell^D)$ blocks, each on $O(\ell^D)$ qubits simulated to error $\epsilon/m$. Gate cost per block: $\operatorname{poly}(\ell^D) \cdot \operatorname{polylog}(\ell^D/(\epsilon/m))$. Total gate cost:

$$O\left(Tn \operatorname{polylog}\frac{Tn}{\epsilon}\right)$$

Circuit depth: $O(T \operatorname{polylog}(Tn/\epsilon))$ since blocks within a time slice can run in parallel.

### Time-dependent extension

The algorithm extends to time-dependent Hamiltonians $H(t) = \sum_X h_X(t)$ where each entry of $h_X(t)$ is **piecewise slowly varying** (derivative bounded by $O(1)$ per piece, with $O(T)$ pieces) and efficiently computable. The Lieb-Robinson bound still holds, and the block simulations use time-dependent algorithms like the [[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes|truncated Dyson series]].

### Merging optimisation

Adjacent blocks share backward/forward time evolutions that can be merged (Eq. 13 in the paper):

$$e^{-itH} e^{-itH} \approx e^{-itH_A} e^{+itH_Y} e^{-i2tH_{Y \cup B}} e^{+itH_Y} e^{-itH_A}$$

This reduces the total number of block simulations from $m$ to $\frac{3m}{2}$ or $\frac{2m}{3}$ depending on the counting.

---

## Key results

### Theorem 1 (main algorithm)

For a lattice Hamiltonian $H(t) = \sum_X h_X(t)$ on $n$ qubits in $\mathbb{R}^D$ with $D = O(1)$, each term geometrically local, piecewise slowly varying, and $\|h_X(t)\| \leq 1$:

$$\text{Gate count} = O\left(Tn \operatorname{polylog}\frac{Tn}{\epsilon}\right), \qquad \text{Depth} = O\left(T \operatorname{polylog}\frac{Tn}{\epsilon}\right)$$

using $O(1)$ ancillas per system qubit. All gates are geometrically local 2-qubit gates.

### Theorem 2 (gate lower bound)

For any $n, T \leq 4^n$, there exists a piecewise-constant bounded 1D Hamiltonian whose simulation to constant error requires $\tilde{\Omega}(Tn)$ 2-qubit gates — even with unlimited ancillae and non-local gates from an infinite gate set.

### Theorem 3 (local measurement lower bound)

Even if we only require correctness on local observables (measuring the first qubit), the lower bound is $\tilde{\Omega}(\min(Tn, T^2))$.

The lower bounds use a circuit-size hierarchy theorem: they show that depth-$T$ local circuits can compute $2^{\tilde{\Omega}(Tn)}$ distinct Boolean functions (Lemma 8), while circuits with $G$ arbitrary gates can compute at most $2^{\tilde{O}(G \log n)}$ functions (Lemma 9). Setting these equal forces $G = \tilde{\Omega}(Tn)$.

### Numerical benchmarks (Heisenberg model)

For the 1D antiferromagnetic Heisenberg model $H = \sum_j (X_jX_{j+1} + Y_jY_{j+1} + Z_jZ_{j+1} + z_jZ_j)$:
- The Lieb-Robinson decomposition error scales as $\epsilon_{\mathrm{LR}} = \alpha \left(\frac{t\beta}{\ell+\gamma}\right)^{\ell+\gamma}$, nearly independent of where the overlap is placed.
- For $T = n$ and $\epsilon = 10^{-3}$, the algorithm achieves $O(n^2)$ T-gate scaling, crossing below direct QSP (which scales as $\tilde{O}(n^3)$) at around $n \sim 100$ qubits.

---

## Comparison with prior work

| Method | Gate cost | Depth | Geometry? | $1/\epsilon$ dep. |
|---|---|---|---|---|
| Sparse QSP/Taylor | $O(n^2 T \operatorname{polylog})$ | $O(n^2 T \operatorname{polylog})$ | No | polylog |
| $k$-th order Suzuki | $O(nT(nT/\epsilon)^\delta)$ | $O(T(nT/\epsilon)^\delta)$ | Yes | polynomial |
| [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes\|Childs et al. (2019) Trotter]] | $O(n^{1+1/k} T^{1+1/k} / \epsilon^{1/k})$ | — | Yes | polynomial |
| **This paper** | $O(nT \operatorname{polylog}(nT/\epsilon))$ | $O(T \operatorname{polylog}(nT/\epsilon))$ | **Yes** | **polylog** |
| Lower bound | $\tilde{\Omega}(nT)$ | — | — | — |

The key conceptual point: prior optimal algorithms (QSP, Taylor series) treated the Hamiltonian as a black-box sparse matrix and couldn't exploit geometric locality for gate-level compilation. Product formulas naturally respect geometry but pay polynomial cost in $1/\epsilon$. This paper gets both: geometric locality **and** polylog precision.

---

## Limits / caveats

- The algorithm is designed for geometrically local Hamiltonians on bounded-dimensional lattices. For non-geometrically-local Hamiltonians (e.g., all-to-all interactions from second-quantised chemistry), the decomposition doesn't apply and sparse simulation methods remain needed.
- The polylog factors hide dependence on $D$: the number of decomposition layers is $2D+1$ with the basic hyperplane approach, reduced to $2\alpha - 1$ with an $\alpha$-colourable tessellation (e.g., hexagonal in 2D gives 5 layers instead of 9).
- The crossover point where this algorithm beats direct QSP on the Heisenberg model is around $n \sim 100$ qubits. For smaller systems, the overhead from the Lieb-Robinson decomposition isn't worth it.
- The lower bound holds for time-dependent Hamiltonians via circuit-to-Hamiltonian embedding. For time-independent Hamiltonians, it's an open question whether one can do better.
- The algorithm requires $O(1)$ ancillas per system qubit, interspersed with the system qubits in the lattice geometry. This is a mild requirement but does mean the physical qubit layout must accommodate them.

---

## Reusable ideas

1. [[Lieb-Robinson Decomposition for Lattice Simulation]] — Decompose the full time-evolution unitary into a product of small block unitaries via Lieb-Robinson bounds: $e^{-itH} \approx e^{-itH_A} e^{+itH_Y} e^{-itH_{Y \cup B}}$ where $Y$ is an overlap region of size $\ell = O(\log(nT/\epsilon))$. Error decays exponentially in $\ell$. Then simulate each block with any polylog-precision algorithm. This reduces an $n$-qubit simulation to $O(n/\ell^D)$ independent simulations on $O(\ell^D)$ qubits.

2. [[Circuit-Size Hierarchy for Gate-Complexity Lower Bounds]] — Count how many distinct Boolean functions (or distinguishable unitaries) can be computed by depth-$T$ geometrically-local circuits vs. $G$-gate arbitrary circuits. The mismatch forces $G = \tilde{\Omega}(nT)$. Applies to any simulation problem where the dynamics can embed arbitrary local circuits.

3. [[Commutator-Sensitive Lieb-Robinson Bound]] — When Hamiltonian terms have small commutators $\|[h_X, h_Y]\| \leq 2\eta\|h_X\|\|h_Y\|$, the Lieb-Robinson velocity reduces from $O(\zeta_0)$ to $O(\zeta\sqrt{8\eta})$. This gives zero velocity in the commuting limit ($\eta \to 0$), unlike standard Lieb-Robinson bounds. The improved velocity directly translates to smaller overlap regions $\ell$ and fewer blocks.

---

## References within this paper

- [[Simulating Hamiltonian Dynamics with a Truncated Taylor Series (Berry-Childs-Cleve-Kothari-Somma 2015) — Paper Notes|Berry, Childs, Cleve, Kothari, Somma (2015)]] — truncated Taylor series simulation; used as the block simulator
- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|Low, Chuang (2016, 2017)]] — QSP and qubitization; used for block simulation and numerical benchmarks
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes|Low, Chuang (2019)]] — qubitization; used in conjunction with QSP for encoding the block Hamiltonians
- Osborne (2006), arXiv:quant-ph/0508031 — first explicit decomposition of time evolution using Lieb-Robinson bounds for 1D spin chains
- Lieb, Robinson (1972) — the original Lieb-Robinson bound on information propagation speed
- Hastings (2004), Nachtergaele-Sims (2006), Hastings-Koma (2006) — improved Lieb-Robinson bounds
- [[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes|Kieferová, Scherer, Berry (2018)]] — truncated Dyson series for time-dependent Hamiltonians; applicable to the blocks when $H$ is time-dependent
- Prémont-Schwarz, Hamma, Klich, Markopoulou-Kalamara (2010) — prior Lieb-Robinson bounds with commutator conditions, but under stronger structural assumptions
- Childs, Maslov, Nam, Ross, Su (2017), arXiv:1711.10980 — numerical benchmark for Hamiltonian simulation gate counts (Heisenberg model); noted the $n^3$ bottleneck
- Verstraete, Cirac (2005) — mapping fermionic Hamiltonians to local qubit Hamiltonians using auxiliary fermions; used to extend the result to fermionic systems
- [[Resonant Equiangular Composite Gates (Low-Yoder-Chuang 2016) — Paper Notes|Low, Yoder, Chuang (2016)]] — QSP phase-angle computation; used in the numerical benchmarks (Appendix E)
- Haah (2019), arXiv:1806.10236 — product decomposition of periodic functions in QSP; classical algorithm for computing QSP phase angles

---

## Cross-links

### Paper notes
- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes]] — QSP used as the block simulator; this paper shows QSP's polylog precision is essential
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]] — qubitization provides the encoding for block simulation
- [[Simulating Hamiltonian Dynamics with a Truncated Taylor Series (Berry-Childs-Cleve-Kothari-Somma 2015) — Paper Notes]] — alternative block simulator with polylog precision
- [[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes]] — Dyson series for time-dependent block Hamiltonians
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]] — tight Trotter error bounds; product formulas are an alternative block simulator (but with polynomial $1/\epsilon$)
- [[Exponential Quantum Speedup in Simulating Coupled Classical Oscillators (Babbush, Berry, Kothari, Somma, Wiebe 2023) — Paper Notes]] — Kothari co-author; uses similar lattice structure
- [[Quantum Algorithms for Quantum Field Theories (Jordan-Lee-Preskill 2012) — Paper Notes]] — lattice QFT simulation; their Section 4.3 claimed $O(nT(nT/\epsilon)^\delta)$ gate cost, which this paper makes rigorous
- [[Faster Digital Quantum Simulation by Symmetry Protection (Tran-Su-Childs-Wiebe 2021) — Paper Notes]] — symmetry protection can further improve the block simulations
- [[Efficient Phase-Factor Evaluation in QSP (Dong-Meng-Whaley-Lin 2021) — Paper Notes]] — improved QSP angle-finding; relevant for the block simulation subroutine

### Trick cards
- [[Lieb-Robinson Decomposition for Lattice Simulation]]
- [[Circuit-Size Hierarchy for Gate-Complexity Lower Bounds]]
- [[Commutator-Sensitive Lieb-Robinson Bound]]
- [[Truncated Taylor Series Simulation]]
- [[Block-Diagonal Decomposition for Parallel Hamiltonian Simulation]]
- [[Order-Condition Cancellation in Product Formulas]]
