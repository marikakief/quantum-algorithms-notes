> **Source:** Dominic W. Berry, Graeme Ahokas, Richard Cleve, and Barry C. Sanders, *Efficient quantum algorithms for simulating sparse Hamiltonians*, arXiv:quant-ph/0508139, Commun. Math. Phys. **270**, 359–371 (2007)
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/0508139) · [CMP](https://doi.org/10.1007/s00220-006-0150-x)
> **Tags:** #hamiltonian-simulation #sparse #product-formulas #suzuki #lower-bound

---

## The computational problem

The paper gives two formal problem definitions that set the framework for most later work on [[Hamiltonian simulation]].

**Problem 1 (sum-of-terms model):** Given $H = \sum_{j=1}^m H_j$ where each $e^{-iH_j t'}$ can be efficiently simulated for arbitrary $t'$, simulate $e^{-iHt}$ to trace-distance error $\leq \epsilon$. Complexity measure: the number of exponentials $N_{\mathrm{exp}}$ in the product-formula sequence.

**Problem 2 (sparse black-box model):** $H$ is $d$-sparse (at most $d$ nonzero entries per column) on a space of dimension $\leq 2^n$. A black-box function $f$ returns, for column $x$ and index $i \leq d$, the pair $(y_i, H_{x,y_i})$ — the $i$-th nonzero row index and its matrix element. A corresponding unitary $U_f$ (and $U_f^\dagger$) implements this reversibly. Simulate $e^{-iHt}$ to trace-distance error $\leq \epsilon$. Complexity measure: the number of calls $N_{\mathrm{bb}}$ to $U_f$.

These two formulations separate the simulation question cleanly: Problem 1 is about the integrator (how many easy exponentials?), Problem 2 is about oracle access (how many queries?). The paper solves Problem 1 with [[Suzuki Order as a Tunable Knob for Time Scaling|Suzuki integrators]], then reduces Problem 2 to Problem 1 via the [[Edge-Coloring Decomposition for Sparse Hamiltonians|edge-coloring decomposition]].

---

## What the paper does

Shows how to simulate a $d$-sparse Hamiltonian $H$ on $n$ qubits with near-linear scaling in $t$, given black-box access to the matrix entries. The two main contributions:

1. **Decomposition:** Any $d$-sparse Hamiltonian can be written as a sum of $m = 6d^2$ [[1-Sparse Hamiltonian Simulation via 2×2 Blocks|one-sparse Hamiltonians]], each simulable with $O(\log^* n)$ black-box queries.
2. **Near-linear time scaling:** Using [[Suzuki Order as a Tunable Knob for Time Scaling|Suzuki integrators of order $2k$]], the total query complexity is

$$
O\!\left((\log^* n)\, d^2 \cdot 5^{2k} \cdot (d^2 \|H\| t)^{1+1/2k} / \epsilon^{1/2k}\right).
$$

By choosing $k$ appropriately, this becomes nearly linear in $t$ (up to subexponential corrections in $\log(d^2 \|H\| t / \epsilon)$).

This was the first result to break away from the polynomial-in-$t$ scaling of [[Adiabatic Quantum State Generation and Statistical Zero Knowledge (Aharonov-Ta-Shma 2003) — Paper Notes|Aharonov and Ta-Shma's]] earlier approach and achieve near-optimal time dependence for sparse Hamiltonians.

---

## The decomposition: sparse → 1-sparse

A Hamiltonian is **1-sparse** if every row and column has at most one off-diagonal nonzero entry. Such a Hamiltonian acts as a direct sum of $1 \times 1$ and $2 \times 2$ blocks — trivially diagonalizable with one query each (see [[1-Sparse Hamiltonian Simulation via 2×2 Blocks]]).

The problem: a $d$-sparse Hamiltonian isn't 1-sparse. The paper decomposes it into 1-sparse pieces via [[Edge-Coloring Decomposition for Sparse Hamiltonians|graph edge coloring]].

**Step 1 — Build the graph.** Treat $H$ as a bipartite graph: rows on one side, columns on the other, with an edge for each nonzero off-diagonal entry. Each vertex has degree $\leq d$.

**Step 2 — Color the edges.** A bipartite graph with max degree $d$ has a proper edge coloring with $d$ colors (König's theorem). Each color class is a perfect matching — a 1-sparse matrix. But computing an optimal coloring requires global knowledge of the graph.

**Step 3 — Do it with local access only.** The paper uses an explicit local coloring scheme that doesn't need the full graph. It assigns colors based on a deterministic rule using only the black-box neighbor function $f$. The cost: the coloring uses $6d^2$ colors instead of the optimal $d$, and each color query costs $O(\log^* n)$ calls to $f$ (from iterated applications of the local rule).

This gives: $H = \sum_{j=1}^{6d^2} H_j$, each $H_j$ one-sparse.

**Step 4 — Simulate each 1-sparse term.** A [[1-Sparse Hamiltonian Simulation via 2×2 Blocks|1-sparse Hermitian matrix]] is a direct sum of $2 \times 2$ blocks $\begin{pmatrix} H_{xx} & H_{xy} \\ H_{yx} & H_{yy} \end{pmatrix}$ (plus $1\times 1$ diagonal entries). Each block is diagonalized analytically. One query to $f$ finds the neighbor $y$ and the matrix elements, then $e^{-iH_j t}$ is applied as a controlled rotation on two basis states.

```
  d-sparse H
       │
       ▼
  Edge coloring (local, O(log* n) queries per color)
       │
       ▼
  6d² one-sparse terms:  H = H₁ + H₂ + ··· + H_{6d²}
       │
       ▼
  Each Hⱼ = direct sum of 2×2 blocks
       │
       ▼
  Simulate each block: 1 query → eigenvalues → controlled rotation
```

---

## Suzuki integrators and the time scaling

With $H = \sum_{j=1}^m H_j$ and each $H_j$ efficiently simulable, the paper uses the Suzuki recursive construction (see [[Order-Condition Cancellation in Product Formulas]] for why higher-order formulas cancel lower-order error terms):

$$
S_2(\lambda) = \prod_{j=1}^{m} e^{H_j \lambda/2} \cdot \prod_{j=m}^{1} e^{H_j \lambda/2}
$$

$$
S_{2k}(\lambda) = \bigl[S_{2k-2}(p_k \lambda)\bigr]^2 \cdot S_{2k-2}((1-4p_k)\lambda) \cdot \bigl[S_{2k-2}(p_k \lambda)\bigr]^2
$$

where $p_k = (4 - 4^{1/(2k-1)})^{-1}$.

The local error of $S_{2k}$ is $O(\lambda^{2k+1})$. Splitting total time $t$ into $r$ steps, the global error satisfies:

$$
\bigl\|e^{-iHt} - [S_{2k}(-it/r)]^r\bigr\| \leq \frac{2(2m \cdot 5^{k-1} \|H\| t)^{2k+1}}{(2k+1) \cdot 3 \cdot r^{2k}}.
$$

Setting this $\leq \epsilon$ and solving for $r$ gives the number of Trotter steps. Each step uses $2 \cdot 5^{k-1} \cdot m$ exponentials. Total exponential count:

$$
N_{\mathrm{exp}} \leq 2m \cdot 5^{2k} \cdot \frac{(m\tau)^{1+1/2k}}{\epsilon^{1/2k}}
$$

where $\tau = \|H\|t$.

The [[Suzuki Order as a Tunable Knob for Time Scaling|key trick]]: $k$ is a free parameter. Making $k$ larger improves the $\tau$-exponent ($1+1/2k \to 1$) but worsens the constant ($5^{2k}$ blows up). Optimizing gives nearly-linear-in-$\tau$ scaling with subexponential overhead:

$$
N_{\mathrm{exp}} \leq 4m^2 \tau \cdot \exp\!\bigl(2\sqrt{\ln 5 \cdot \ln(m\tau/\epsilon)}\bigr).
$$

---

## The lower bound

**Theorem 3:** For any $N$, there exists a 2-sparse Hamiltonian such that simulating it for time $\tau = \pi N/2$ to trace distance $1/4$ requires $\geq \tau/(2\pi)$ queries.

The construction: a 1D nearest-neighbor chain (a path graph) where evolving for time $t$ moves a wavepacket by $\sim t$ sites. To distinguish "moved" from "not moved," you need to query enough sites — and the number of sites traversed grows linearly with $t$.

```
 |ψ(0)⟩                    |ψ(t)⟩
    ●                          ●
    ├──┤──┤──┤──┤──┤──┤──┤──┤──┤
    0  1  2  3  4  5  6  7  8  9  ···

    Wavepacket moves ~t sites → need ~t queries to detect
```

**Corollary:** No [[product formula]]-based scheme (or any black-box scheme) can simulate sparse Hamiltonians in sublinear time. The near-linear scaling achieved above is the best possible up to subexponential factors.

---

## Where this sits historically

| Era | Paper | Sparsity dependence | Time scaling |
|---|---|---|---|
| 2003 | Aharonov–Ta-Shma | Polynomial in $d$ | Polynomial in $t$ |
| **2005** | **This paper** | $d^4$ | **Near-linear in $t$** |
| 2007 | Childs (quantum walk) | $d$ | Linear in $t$ |
| 2011 | [[Black-Box Hamiltonian Simulation and Unitary Implementation (Berry-Childs 2011) — Paper Notes\|Berry–Childs]] | $d$ | Linear in $t$, improved $\delta$-dependence |
| 2012 | [[LCU Origins (Childs-Wiebe 2012) — Paper Notes\|Childs–Wiebe (LCU)]] | $d^2$ | Near-optimal via Taylor series |
| 2015 | Berry–Childs–Cleve–Kothari–Somma (Taylor LCU) | $d^2$ | $O(d^2 \|H\|_{\max} t / \log(1/\epsilon))$ |
| 2016–19 | [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes\|Low–Chuang (QSP/qubitization)]] | Through [[Standard-Form Encoding (Prepare + Signal Oracle)\|block-encoding]] | Optimal: $O(\tau + \log(1/\epsilon))$ |

The sparsity dependence here ($d^4$ from $m = 6d^2$ with $m^2$ in the exponential count) was later improved by Childs's quantum-walk approach (2007) to linear in $d$. But the near-linear time scaling introduced here was the lasting contribution — the idea that you can [[Suzuki Order as a Tunable Knob for Time Scaling|trade integrator order for time exponent]].

---

## Reusable ideas

1. **[[Edge-Coloring Decomposition for Sparse Hamiltonians]]:** Reduce a $d$-sparse Hamiltonian to a sum of 1-sparse pieces using only local graph queries. The quadratic blowup ($6d^2$ terms) is the price for locality.

2. **[[Suzuki Order as a Tunable Knob for Time Scaling]]:** Treat the Suzuki integrator order $2k$ as a tunable parameter. Higher order → better time scaling, worse constant. Optimize $k$ to get near-linear total cost.

3. **[[1-Sparse Hamiltonian Simulation via 2×2 Blocks]]:** A 1-sparse Hermitian matrix decomposes into independent $2 \times 2$ blocks. One query gives you the block; one rotation simulates it. This is the base case that all product-formula sparse simulation builds on.

4. **Linear-in-$t$ lower bound:** Wavepacket propagation on a chain. Simple, clean, and it closes the door on sublinear scaling in the black-box model.

---

## References within this paper

- [[Adiabatic Quantum State Generation and Statistical Zero Knowledge (Aharonov-Ta-Shma 2003) — Paper Notes|Aharonov & Ta-Shma (2003)]] — first sparse [[Hamiltonian simulation]] via [[Sparse Hamiltonian Simulation via Coloring Decomposition|coloring decomposition]]; polynomial-in-$t$ scaling that this paper improves on
- [[Universal Quantum Simulators (Lloyd 1996) — Paper Notes|Lloyd (1996)]] — original proposal for simulating local Hamiltonians via [[Order-Condition Cancellation in Product Formulas|product formulas]] (Science 273, 1073)
- Suzuki (1991) — the recursive higher-order integrator construction used throughout (Phys. Lett. A 146, 319)
- Childs (2004, unpublished) — quantum walk approach to sparse simulation that later achieved linear-in-$d$ scaling

---

## Cross-links

### Paper notes
- [[Higher Order Decompositions of Ordered Operator Exponentials (Wiebe-Berry-Høyer-Sanders 2010) — Paper Notes]] — extends the [[Suzuki Order as a Tunable Knob for Time Scaling|Suzuki order optimization]] from this paper to time-dependent Hamiltonians, with rigorous non-analytic bounds
- [[Black-Box Hamiltonian Simulation and Unitary Implementation (Berry-Childs 2011) — Paper Notes]] — improved the sparsity dependence to linear in $d$
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]] — modern analysis of the [[Order-Condition Cancellation in Product Formulas|product-formula error]] used here
- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes]] — achieves the truly optimal scaling this paper approaches
- [[LCU Origins (Childs-Wiebe 2012) — Paper Notes]] — alternative to [[product formula]]s for sparse simulation
- [[Limitations on Simulation of Non-Sparse Hamiltonians (Childs-Kothari-Somma 2010) — Paper Notes]] — what happens when sparsity fails
- [[Faster Digital Quantum Simulation by Symmetry Protection (Tran-Su-Childs-Wiebe 2021) — Paper Notes]] — uses symmetry to improve the [[product formula]]s introduced here
- [[Adiabatic Quantum Computation is Equivalent to Standard Quantum Computation (Aharonov-van Dam-Kempe-Landau-Lloyd-Regev 2004) — Paper Notes]] — the adiabatic equivalence result that placed sparse simulation in BQP

### Trick cards
- [[Edge-Coloring Decomposition for Sparse Hamiltonians]]
- [[1-Sparse Hamiltonian Simulation via 2×2 Blocks]]
- [[Suzuki Order as a Tunable Knob for Time Scaling]]
- [[Order-Condition Cancellation in Product Formulas]]
- [[Trotter Commutator-Scaling Bound]]
- [[Selection and Improvement of Product Formulae for Best Performance of Quantum Simulation (Morales-Costa-Pantaleoni-Burgarth-Sanders-Berry 2025) — Paper Notes]] — optimizes the constant factors in high-order [[product formula]]s that this paper first applied to quantum simulation
