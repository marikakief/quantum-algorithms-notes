> **Source:** Dominic W. Berry, Kianna Wan, Andrew D. Baczewski, Elliot C. Eklund, Arkin Tikku, Ryan Babbush, *Quantum simulation of chemistry via quantum fast multipole method*, arXiv:2510.07380 (2025)
> **Links:** [arXiv](https://arxiv.org/abs/2510.07380)
> **Tags:** #quantum-simulation #quantum-chemistry #first-quantized #product-formula #real-space #Coulomb #fast-multipole #FMM #sorting-network #QRAM-avoidance #fault-tolerant

---

## The computational problem

Simulate the Born-Oppenheimer electronic structure Hamiltonian

$$H = T + U + V + \sum_{l \neq \kappa} \frac{q_l q_\kappa}{2\|R_l - R_\kappa\|}$$

on a real-space grid in [[First-Quantized Plane-Wave Chemistry Encoding|first quantization]], where $T$ is the kinetic energy (diagonal after QFT), $U$ is the electron-nuclear potential, and $V = \sum_{i \neq j} 1/(2\|r_i - r_j\|)$ is the electron-electron Coulomb interaction. Parameters: $\eta$ electrons, $N$ grid points, $\Omega$ cell volume, precision $\epsilon$, evolution time $t$.

The bottleneck: computing $V$ requires summing over all $\binom{\eta}{2}$ pairs, giving $O(\eta^2)$ cost per Trotter step. The question this paper answers is whether the classical fast multipole method's $\widetilde{O}(\eta)$ scaling can be recovered on a quantum computer.

---

## What the paper does

Yes, it can. The paper constructs a quantum version of the fast multipole method (FMM) that computes the Coulomb potential in $\widetilde{O}(\eta)$ operations per Trotter step, then combines this with high-order [[product formula]]s and the [[Restricted Interaction Norms for Product Formulas|Low et al. Trotter bounds]] to achieve overall gate complexity

$$t\left(\eta^{4/3} N^{1/3} + \eta^{1/3} N^{2/3}\right) \left(\frac{\eta N t}{\epsilon}\right)^{o(1)}$$

in the thermodynamic limit $\Omega \propto \eta$. This is roughly an $O(\eta)$ speedup over all prior first-quantized [[product formula]] approaches (which pay $O(\eta^2)$ for the Coulomb sum), and gives the **lowest known complexity for any approach** when $N < \eta^6$ — the regime that covers essentially all practical molecular and materials simulations.

The space complexity is $O(\eta \log N \cdot \text{polylog}(1/\epsilon))$.

My assessment: this resolves a long-standing open problem in quantum simulation of chemistry — how to avoid the $O(\eta^2)$ Coulomb overhead in product-formula-based first-quantized simulation. The solution is algorithmically interesting: the hard part isn't translating the FMM itself, but avoiding the $O(\eta)$ quantum data-access overhead that would negate the speedup. The [[Quantum Sorting for Fixed-Location Data Access|sorting network approach]] and [[Shifted Morton Orderings for Interaction List Access|shifted Morton orderings]] are the key innovations. The practical relevance is tempered by the requirement that $\eta \gtrsim 10^3$ to see the advantage — same threshold as the classical FMM.

---

## The algorithm / construction

### Setup: real-space first-quantized encoding

The $\eta$-electron state lives on a grid $G = [-(N^{1/3}-1)/2, (N^{1/3}-1)/2]^3$ with positions encoded as binary natural numbers $|x\rangle|y\rangle|z\rangle$ using $3\lceil\log N^{1/3}\rceil$ qubits per electron. This is the [[Real-Space Grid Encoding for Many-Body Simulation|real-space grid encoding]] from [[Polynomial-Time Quantum Algorithm for the Simulation of Chemical Dynamics (Kassal-Jordan-Love-Mohseni-Aspuru-Guzik 2008) — Paper Notes|Kassal et al. (2008)]].

Time evolution uses a split-operator Trotter step: evolve under $T$ (via QFT → phase kickback → inverse QFT, cost $\widetilde{O}(\eta)$) then under $U + V$ (via computing the potential and applying [[Phase Kickback from Oracle Queries|phase kickback]]).

### The classical FMM in brief

The FMM computes the Coulomb potential by:
1. Building an $L$-level octree over the simulation cell
2. **Upward pass:** Accumulate multipole moments (charges, then higher moments) from leaf boxes up to level 3
3. **Downward pass:** For each box $b$ at each level, compute the far-field potential from boxes in $b$'s **interaction list** $I(b)$ — boxes that are not nearest neighbours but whose parents are neighbours. $|I(b)| \leq 6^3 - 3^3 = 189$ in 3D.
4. **Near-field:** Compute exact pairwise interactions between particles in neighbouring leaf boxes

With $O(1)$ particles per leaf box (choosing $n_b = O(\eta)$ leaf boxes), the total cost is $O(\eta \cdot \text{polylog}(1/\epsilon))$.

### Why naïve quantization fails

Translating FMM directly to a quantum circuit requires placing each electron into its box — but which box depends on the position register, which is in superposition. A direct lookup costs $O(\eta)$ per electron (scanning all boxes), so $O(\eta^2)$ total, destroying the speedup. This is the QRAM problem. The approach in Childs et al. (2022) [Ref. 21] suffers exactly this overhead.

### The fix: sorting networks + per-particle registers

The paper's approach eliminates quantum addressing entirely through three interconnected ideas:

**1. [[Quantum Sorting for Fixed-Location Data Access|Quantum sorting to fixed locations]]:** Sort all $\eta$ particle registers by position (using a [[Sorting Networks as Quantum Control-Flow Compilers|quantum sorting network]]). After sorting, particles in the same spatial box are contiguous in register space. All subsequent operations happen at classically predictable register offsets.

**2. [[Per-Particle Box Registers (Avoiding Quantum Addressing)|Per-particle box registers]]:** Instead of one data structure per box (requiring quantum-addressed lookup), each particle $j$ carries its own multipole/charge registers $Q(j, \ell)$ for every tree level $\ell$. All particles in the same box store identical values — information is replicated, not shared.

**3. [[Forward-Backward Sweep for Hierarchical Aggregation|Forward-backward linear sweep]]:** Aggregate child-box charges into parent-box charges by a forward pass ($j = 0$ to $\eta-1$) that copies charges across box boundaries, a backward pass that copies in the opposite direction, then a local add. This replaces the tree recursion with linear sweeps over the sorted sequence.

### Evenly distributed case (Section IV)

For uniformly distributed particles, each leaf box has at most $c = O(1)$ particles. The algorithm:

1. **Sort** particle registers into Morton order (interleaved $xyz$ bits). Cost: $O(\eta \log^2 \eta \log N)$ gates.
2. **Swap** particles into correct child-box registers at each level, recursively subdividing. This is Algorithm 2 in the paper.
3. **Compute charges** at each level via summation over child boxes (quantum arithmetic on box registers).
4. **Compute far-field potential** by iterating over the interaction list at each level: for each box, multiply its charge $Q_b$ by the precomputed potential $\Phi(c_a, c_b)$ from each box $a \in I(b)$.
5. **Compute near-field potential** via exact pairwise Coulomb interactions between particles in neighbouring leaf boxes, using [[QROM Interpolation plus Newton-Raphson for Inverse Square Root|QROM + Newton iteration]] for inverse square root evaluation.

Dominant cost: the pairwise near-field interactions in step 5, giving $O(\eta \log^2(1/\epsilon) \log\log(1/\epsilon))$.

### Adaptive case for arbitrary distributions (Section V)

This is where the paper earns its keep. Non-uniform particle distributions break the even-distribution assumption. The adaptive FMM subdivides boxes until each contains at most one particle (leaf level = full position resolution).

**Charge aggregation** (Algorithm 3): Uses the [[Forward-Backward Sweep for Hierarchical Aggregation|forward-backward sweep]] on Morton-sorted particles. For each tree level $\ell$ from $L$ down to 3, copy charges across child-box boundaries, then add.

**Potential computation** (Algorithm 5): This is the hard part. The interaction list at level $\ell$ can include boxes up to 63 positions away in the Morton ordering (in 3D). The paper uses [[Shifted Morton Orderings for Interaction List Access|shifted Morton orderings]]:

- For each of $2^d$ shift vectors $z \in \{0, 2\}^d$, add $z$ to the box coordinates and re-sort into Morton order.
- **Lemma 1** proves that for any box $b$ and any box $a$ in its interaction list, there exists a shift $z$ such that $|M(b+z) - M(a+z)| \leq 4^d - 1 = 63$ (in 3D).
- For each shifted ordering, use Algorithm 4 (a linear sweep copying charge data from up to $K = 4^d - 1$ neighbours) to make interaction-list data available at each particle's register location.

Key subtlety: only the forward direction is needed (not backward), because $a \in I(b) \Leftrightarrow b \in I(a)$ — each box-box interaction is counted once.

### Overall complexity

Per-step cost for the adaptive quantum FMM:

- Sorting: $O(\eta \log \eta \log^2 N)$ — $O(\log N)$ sorts, each $O(\eta \log \eta)$ comparators on $O(\log N)$-bit items
- Charge aggregation: $O(\eta \log N \cdot \text{polylog}(1/\epsilon))$
- Potential computation: $O(\eta \log \eta \log N \log(1/\epsilon))$ — dominant term

Combined with the [[Restricted Interaction Norms for Product Formulas|product formula step count]]:

$$t\left(\frac{\eta^{2/3} N^{1/3}}{\Omega^{1/3}} + \frac{N^{2/3}}{\Omega^{2/3}}\right) \left(\frac{\eta N t}{\Omega \epsilon}\right)^{o(1)}$$

Trotter steps, each costing $\widetilde{O}(\eta)$ for the FMM. Total gate complexity:

$$t\left(\frac{\eta^{5/3} N^{1/3}}{\Omega^{1/3}} + \frac{\eta N^{2/3}}{\Omega^{2/3}}\right) \left(\frac{\eta N t}{\Omega \epsilon}\right)^{o(1)}$$

which simplifies to $t(\eta^{4/3} N^{1/3} + \eta^{1/3} N^{2/3})(\eta N t/\epsilon)^{o(1)}$ in the thermodynamic limit.

Qubit count: $O(\eta \log N \log^3(1/\epsilon))$ for the full multipole expansion.

---

## Key results

**Theorem (informal, Eq. 7/42):** The electronic structure Hamiltonian in first-quantized real-space representation can be simulated with gate complexity

$$t\left(\eta^{4/3} N^{1/3} + \eta^{1/3} N^{2/3}\right) \left(\frac{\eta N t}{\epsilon}\right)^{o(1)}$$

where the $o(1)$ exponent can be made arbitrarily small by increasing the [[product formula]] order.

**Lemma 1 (Shifted Morton orderings):** For any point $p$ in the grid and any $q$ in its interaction-list neighbourhood ($|\lfloor p_j/2 \rfloor - \lfloor q_j/2 \rfloor| \leq 1$ for all coordinates $j$), there exists a shift $z \in \{0, 2\}^d$ such that the shifted Morton indices satisfy $|M(p+z) - M(q+z)| \leq 4^d - 1$.

---

## Comparison with prior work

| Algorithm | Year | Gate complexity (thermodynamic limit) | Best regime |
|---|---|---|---|
| Kassal et al. (1st quant, Trotter, naïve) | 2008 | $\widetilde{O}(\eta^2 \cdot \text{Trotter steps})$ | — |
| [[Quantum Simulation of Chemistry with Sublinear Scaling in Basis Size (Babbush, Berry, McClean, Neven 2019) — Paper Notes\|Babbush et al. 2019]] (interaction picture) | 2019 | $\widetilde{O}(\eta^{8/3} N^{1/3} / \epsilon)$ | $N > \eta^6$ |
| [[Quantum Simulation of Exact Electron Dynamics Can Be More Efficient Than Classical Mean-Field Methods (Babbush, Huggins, Berry et al 2023) — Paper Notes\|Babbush et al. 2023]] (1st quant Trotter) | 2023 | $(\eta^{7/3} N^{1/3} + \eta^{4/3} N^{2/3})(Nt/\epsilon)^{o(1)}$ | — |
| [[Quantum Computation of Stopping Power for Inertial Fusion Target Design (Rubin, Berry, Babbush et al 2023) — Paper Notes\|Rubin et al. 2024]] (1st quant Trotter) | 2024 | $\eta^2 (\eta^{2/3} N^{1/3} + N^{2/3}/\eta^{2/3})(\eta N/\epsilon)^{o(1)}$ | — |
| Stetina & Wiebe 2025 (Gauss's law) | 2025 | $\eta^2 N^{2/3} (\eta N/\epsilon)^{o(1)}$ | — |
| **This work** (quantum FMM) | 2025 | $(\eta^{4/3} N^{1/3} + \eta^{1/3} N^{2/3})(\eta N t/\epsilon)^{o(1)}$ | $N < \eta^6$ |

The improvement over Rubin et al. (2024) is roughly a factor of $\eta$. The interaction-picture method wins only when $N > \eta^6$, which is well outside the regime of practical interest for most molecular and materials simulations.

---

## Limits / caveats

1. **Crossover at $\eta \gtrsim 10^3$:** Just like the classical FMM, the quantum version only beats direct summation for large particle counts. For small molecules ($\eta < 100$), the sorting, shifting, and sweeping overhead dominates. This is an asymptotic result, not a near-term one.

2. **Subpolynomial factors hide real cost:** The $(\eta N t/\epsilon)^{o(1)}$ factor absorbs all logarithmic terms from sorting, multipole order, and arithmetic precision. At moderate parameter values, these factors matter.

3. **No constant-factor resource estimates:** Unlike [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes|Su et al. (2021)]] or [[Quantum Computation of Stopping Power for Inertial Fusion Target Design (Rubin, Berry, Babbush et al 2023) — Paper Notes|Rubin et al. (2024)]], this paper provides no concrete Toffoli counts for specific molecules. The practical competitiveness remains to be established.

4. **Product formula precision scaling:** The $\epsilon$ dependence is $\epsilon^{-o(1)}$ (subpolynomial), not $\log(1/\epsilon)$ as in [[Qubitization (Quantum Walk for Spectral Encoding)|qubitization]]-based approaches. For high-precision applications (e.g., phase estimation to chemical accuracy), the precision scaling may matter.

5. **Translation operators left to future work:** The paper acknowledges that the optimal choice of multipole-to-local translation operators (which determines constant factors in the polylog$(1/\epsilon)$ terms) requires further study. This is non-trivial — classical FMM implementations spent decades optimizing these.

6. **Redundancy in Morton orderings:** The $2^d$ shifted orderings provide more interaction-list coverage than strictly needed, leading to duplicate potential calculations that must be filtered. The paper suggests Hilbert orderings or alternative shifts as potential improvements.

---

## Reusable ideas

1. [[Quantum Sorting for Fixed-Location Data Access]] — Sort quantum registers so all subsequent operations happen at classically known locations, avoiding QRAM entirely.

2. [[Per-Particle Box Registers (Avoiding Quantum Addressing)]] — Replicate hierarchical metadata per particle instead of per box, trading register space for elimination of quantum addressing.

3. [[Forward-Backward Sweep for Hierarchical Aggregation]] — Replace tree recursion with linear sweeps over sorted particle sequences for bottom-up aggregation.

4. [[Shifted Morton Orderings for Interaction List Access]] — Use $2^d$ shifted versions of the Morton (Z-order) space-filling curve to guarantee that all interaction-list boxes are within a fixed distance in at least one ordering.

5. [[Restricted Interaction Norms for Product Formulas]] — Tighten Trotter error bounds by computing commutator norms restricted to the physical $\eta$-particle subspace.

---

## References within this paper

Key cited papers with vault notes:
- [[Quantum Simulation of Chemistry with Sublinear Scaling in Basis Size (Babbush, Berry, McClean, Neven 2019) — Paper Notes|Babbush, Berry, McClean, Neven (2019)]] — First-quantized interaction-picture simulation; best scaling for $N > \eta^6$
- [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes|Su, Berry, Wiebe, Rubin, Babbush (2021)]] — First constant-factor first-quantized chemistry compilation
- [[Quantum Computation of Stopping Power for Inertial Fusion Target Design (Rubin, Berry, Babbush et al 2023) — Paper Notes|Rubin, Berry, Babbush et al. (2024)]] — First-quantized [[product formula]] for stopping power; direct predecessor
- [[Quantum Simulation of Exact Electron Dynamics Can Be More Efficient Than Classical Mean-Field Methods (Babbush, Huggins, Berry et al 2023) — Paper Notes|Babbush, Huggins, Berry et al. (2023)]] — First-quantized Trotter for dynamics; same framework without FMM
- [[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes|Low & Wiebe (2018)]] — Interaction-picture simulation; second-quantized predecessor
- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|Low & Chuang (2017)]] — QSP-based simulation
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes|Low & Chuang (2019)]] — Qubitization framework
- [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes|Babbush et al. (2018)]] — Plane-wave dual basis simulation

Papers not yet in the vault:
- Kassal, Jordan, Love, Mohseni, Aspuru-Guzik (2008) — Original first-quantized real-space chemistry simulation; this paper's direct ancestor
- Childs, Leng, Li, Liu, Zhang (2022) — "Quantum simulation of real-space dynamics"; discusses QRAM-based FMM (with the $O(\eta)$ overhead this paper avoids)
- Low, Su, Tong, Tran (2023) — "Complexity of implementing Trotter steps"; [[product formula]] error bounds used here
- Stetina & Wiebe (2025) — First-quantized non-relativistic QED with Gauss's law constraint; concurrent work avoiding $O(\eta^2)$ Coulomb overhead via a different mechanism
- Greengard & Rokhlin (1985–1988) — Original classical fast multipole method

---

## Cross-links

### Related paper notes
- [[Quantum Simulation of Chemistry with Sublinear Scaling in Basis Size (Babbush, Berry, McClean, Neven 2019) — Paper Notes]] — First-quantized interaction picture; wins for $N > \eta^6$
- [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes]] — Compiled first-quantized qubitization circuits
- [[Quantum Computation of Stopping Power for Inertial Fusion Target Design (Rubin, Berry, Babbush et al 2023) — Paper Notes]] — Direct predecessor; same Trotter framework without FMM
- [[Quantum Simulation of Exact Electron Dynamics Can Be More Efficient Than Classical Mean-Field Methods (Babbush, Huggins, Berry et al 2023) — Paper Notes]] — First-quantized dynamics; $O(\eta^2)$ Coulomb version
- [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes]] — Second-quantized plane-wave simulation
- [[Chemical Basis of Trotter-Suzuki Errors in Quantum Chemistry Simulation (Babbush-McClean-Wecker-Aspuru-Guzik-Wiebe 2015) — Paper Notes]] — Trotter error analysis for chemistry
- [[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes]] — Interaction-picture simulation framework
- [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes]] — THC-based second-quantized approach; different regime

### Related trick cards
- [[Quantum Sorting for Fixed-Location Data Access]] — Core technique enabling QRAM-free FMM
- [[Per-Particle Box Registers (Avoiding Quantum Addressing)]] — Data structure design avoiding quantum addressing
- [[Forward-Backward Sweep for Hierarchical Aggregation]] — Linear-sweep replacement for tree recursion
- [[Shifted Morton Orderings for Interaction List Access]] — Space-filling curve shifts for interaction list coverage
- [[Restricted Interaction Norms for Product Formulas]] — Tight Trotter bounds on physical subspace
- [[QROM Interpolation plus Newton-Raphson for Inverse Square Root]] — Used for near-field pairwise potentials
- [[Split-Operator Real-Space Simulation on a Quantum Computer]] — Split-operator Trotter framework
- [[Real-Space Grid Encoding for Many-Body Simulation]] — Grid encoding used here
- [[First-Quantized Plane-Wave Chemistry Encoding]] — Related first-quantized encoding
- [[Sorting Networks as Quantum Control-Flow Compilers]] — Sorting network framework
- [[Phase Kickback from Oracle Queries]] — Phase kickback for time evolution
