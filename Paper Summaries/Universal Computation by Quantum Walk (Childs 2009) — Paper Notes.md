> **Source:** Andrew M. Childs, *Universal computation by quantum walk*, Phys. Rev. Lett. **102**, 180501 (2009); arXiv:0806.1972
> **Links:** [arXiv](https://arxiv.org/abs/0806.1972) · [PRL](https://doi.org/10.1103/PhysRevLett.102.180501)
> **Tags:** #quantum-walk #universality #scattering #graph-computation #foundational

---

## What the paper does

Proves that continuous-time quantum walk on a sparse, unweighted graph of maximum degree 3 is **universal for quantum computation**. Any quantum circuit can be encoded entirely into the structure of a graph, where the walk dynamics implement the computation. This establishes quantum walk as a computational primitive on the same footing as the circuit model and adiabatic computation.

The construction encodes $n$-qubit computational basis states as $2^n$ parallel wires (semi-infinite lines), and implements quantum gates by attaching small graph widgets that scatter incoming wave packets. The key momentum is $k = -\pi/4$, where all widgets have perfect transmission with the desired gate phases.

---

## The computational problem

**Input:** An $m$-gate quantum circuit $U$ on $n$ qubits.

**Output:** A graph $G$ (with $\text{poly}(m, 2^n)$ vertices, maximum degree 3) such that a continuous-time quantum walk on $G$ — starting at a designated input vertex and evolving under the adjacency matrix Hamiltonian — produces the output of $U$ on measurement.

---

## The construction

### Representation

Each of the $2^n$ computational basis states corresponds to a separate wire (a long line of vertices). Gates are implemented by inserting **widgets** — small subgraphs connecting the wires — in sequence.

### Scattering theory on graphs

The core technical tool is scattering theory for particles on infinite graphs. Attach semi-infinite lines to the vertices of a finite graph $G$. An incoming wave of momentum $k$ on line $j$ produces:

$$\langle x, j' | \tilde{k}, \text{sc}_j^{\rightarrow} \rangle = T_{j,j'}(k)\, e^{ikx} \quad (j' \neq j)$$

The transmission coefficient $T_{j,j'}(k)$ determines how the wave is redistributed among output lines. By the method of stationary phase, for large distances the propagator is:

$$|\langle y, j' | e^{-iHt} | x, j \rangle| \sim \frac{|T_{j,j'}(k_\star)|}{\sqrt{2\pi |c(k_\star)|}}$$

where $k_\star$ satisfies $x + y + \ell_{j,j'}(k) = v(k)t$ with group velocity $v(k) = -2\sin k$ and effective length $\ell_{j,j'}(k) = \frac{d}{dk} \arg T_{j,j'}(k)$.

### Universal gate set via scattering

Three widgets suffice for universality, all operating at momentum $k = -\pi/4$:

| Widget | Function | Transmission at $k = -\pi/4$ |
|---|---|---|
| Wire crossing (Fig. 1a) | CNOT — swaps the $\|10\rangle$ and $\|11\rangle$ wires | Perfect, phase $e^{ik}$ |
| Phase gadget (Fig. 1b) | Phase gate $U_b = \text{diag}(1, e^{i\pi/4})$ on $\|1\rangle$ wire | Perfect, extra phase $e^{i\pi/4}$ |
| Basis-change gadget (Fig. 1c) | $U_c = -\frac{1}{\sqrt{2}}\begin{pmatrix} i & 1 \\ 1 & i \end{pmatrix}$ coupling two wires | Perfect, $\ell = 2$ |

$U_b$ and $U_c$ generate a dense subset of $\text{SU}(2)$ (since $U_b^2 U_c U_b^2$ is the Hadamard gate up to global phase). Combined with the CNOT widget, this gives a universal gate set.

### Momentum filtering

Away from $k = -\pi/4$, the gate widgets have imperfect transmission — they partially reflect. The construction adds **filter widgets** (Fig. 1d) that transmit only momenta near $k = -\pi/4$ (and $-3\pi/4$), suppressing all others exponentially. A **momentum separator** (Fig. 1e) then isolates $k = -\pi/4$ from $-3\pi/4$ by exploiting different effective lengths:
- $\ell^{(e)}(-\pi/4) \approx 0.686$
- $\ell^{(e)}(-3\pi/4) \approx 23.3$

This temporal separation, combined with $m_d = \log\Theta(m^2)$ filter stages, ensures only the desired momentum component reaches the gate widgets.

### Composition

When two widgets each have reflection $\|R\| \leq \delta$, their composition has:
- Reflection: $\|R_{12}\| \leq \delta_1 + (1 + \delta_1\delta_2)\delta_2$
- Forward transmission error: $\|T_{12} - T_1 T_2\| \leq \delta_1\delta_2(1 + \delta_1\delta_2)$

For momenta $|k + \pi/4| = O(1/m^2)$, each gate widget has $\|R\| = O(1/m^2)$, so $m$ widgets compose with total $\|R\| = O(1/m)$.

### Bound states

Bound states of the graph (decaying as $e^{-\kappa x}$) are handled by:
1. Starting the walk at distance $x = \Theta(m^4)$ from the first widget (makes weakly bound states negligible)
2. Choosing evolution time $t = \pi\lfloor(x + \ell)/\sqrt{2}\pi\rfloor$ so all bound states enter with approximately the same phase

---

## Key results

| Result | Statement |
|---|---|
| Universality | Any $m$-gate $n$-qubit circuit can be simulated by quantum walk on a graph with $\text{poly}(m, 2^n)$ vertices and maximum degree 3 |
| Graph properties | Sparse (degree ≤ 3), unweighted (all edges have weight 0 or 1) |
| Evolution time | $O(m^4)$ |
| Success probability | $\Omega(1/m^4)$ per run (boost by repetition) |
| Total cost | $\text{poly}(m)$ repetitions × $O(m^4)$ time per run |

---

## Comparison with prior work

| Construction | Degree | Weighted? | Key limitation |
|---|---|---|---|
| Feynman (1985) | Grows with circuit size | Yes | Not a proper quantum walk on a graph |
| [[Adiabatic Quantum Computation is Equivalent to Standard Quantum Computation (Aharonov-van Dam-Kempe-Landau-Lloyd-Regev 2004) — Paper Notes\|Aharonov et al. (2004)]] | — | — | Adiabatic, not walk-based |
| **This paper** | **3 (optimal)** | **No** | Success probability $\Omega(1/m^4)$ |

Degree 3 is optimal: degree-2 graphs are lines/cycles, which can't be universal.

---

## Limits / caveats

- **Exponential graph size.** The graph has $\Theta(2^n)$ wires, so it's exponentially large in $n$. This is necessary (the graph encodes the full Hilbert space), and the graph has a succinct description in terms of the circuit. But you can't literally build the graph — you simulate the walk on a standard quantum computer.
- **Low success probability.** The $\Omega(1/m^4)$ probability per run means you need $O(m^4)$ repetitions. The total time is $\text{poly}(m)$ but with a large polynomial.
- **Fixed momentum required.** The entire construction works at $k = -\pi/4$. Extending to other momenta or momentum distributions would require redesigned widgets.
- **Not immediately algorithmic.** This is a universality result, not an algorithm. It shows quantum walks *can* do anything, but doesn't give a new way to solve specific problems faster. The value is foundational and complexity-theoretic.

---

## Reusable ideas

1. [[Gate Implementation via Graph Scattering]] — implement quantum gates by designing small graph widgets with specific transmission coefficients at a chosen momentum. The S-matrix of the widget encodes the gate.
2. [[Momentum Filtering on Graphs]] — use repeated filter subgraphs to project onto a narrow momentum band, suppressing unwanted components exponentially. Analogous to spectral filtering but in the graph/scattering setting.

---

## References within this paper

- Feynman (1985, Optics News 11) — original Hamiltonian computer; this paper's construction improves on Feynman's by achieving degree 3 and unweighted edges
- [[Exponential Algorithmic Speedup by Quantum Walk (Childs-Cleve-Deotto-Farhi-Gutmann-Spielman 2003) — Paper Notes|Childs et al. (2003)]] — first exponential speedup by quantum walk; oracle-to-Hamiltonian via swap conjugation
- [[Spatial Search by Quantum Walk (Childs-Goldstone 2004) — Paper Notes|Childs-Goldstone (2004)]] — continuous-time quantum walk for spatial search
- Farhi-Gutmann (1998, PRA 58, 915) — introduced continuous-time quantum walks
- [[Any AND-OR Formula Can Be Evaluated in O(N^{1⁄2+o(1)}) Queries (Ambainis-Childs-Reichardt-Špalek-Zhang 2007) — Paper Notes|Ambainis-Childs-Reichardt-Špalek-Zhang (2007)]] — formula evaluation via related scattering ideas
- [[Adiabatic Quantum Computation is Equivalent to Standard Quantum Computation (Aharonov-van Dam-Kempe-Landau-Lloyd-Regev 2004) — Paper Notes|Aharonov et al. (2004)]] — universality of adiabatic computation (analogous result)
- Boykin-Mor-Pulver-Roychowdhury-Vatan (2000) — universality of the gate set $\{U_b, U_c, \text{CNOT}\}$

---

## Cross-links

### Paper notes
- [[Exponential Algorithmic Speedup by Quantum Walk (Childs-Cleve-Deotto-Farhi-Gutmann-Spielman 2003) — Paper Notes]] — first exponential walk speedup; this paper's universality result extends the power demonstrated there
- [[On the Relationship Between Continuous- and Discrete-Time Quantum Walk (Childs 2010) — Paper Notes]] — unifies continuous and discrete walks; the universality here applies to the continuous-time version
- [[Spatial Search by Quantum Walk (Childs-Goldstone 2004) — Paper Notes]] — continuous-time walk for a specific algorithmic task
- [[Discrete-Query Quantum Algorithm for NAND Trees (Childs-Cleve-Jordan-Yonge-Mallo 2009) — Paper Notes]] — scattering-based algorithm for formula evaluation
- [[Any AND-OR Formula Can Be Evaluated in O(N^{1⁄2+o(1)}) Queries (Ambainis-Childs-Reichardt-Špalek-Zhang 2007) — Paper Notes]] — scattering on graphs for formula evaluation
- [[Adiabatic Quantum Computation is Equivalent to Standard Quantum Computation (Aharonov-van Dam-Kempe-Landau-Lloyd-Regev 2004) — Paper Notes]] — analogous universality proof for adiabatic model
- [[Black-Box Hamiltonian Simulation and Unitary Implementation (Berry-Childs 2011) — Paper Notes]] — extends oracle-to-Hamiltonian ideas

### Trick cards
- [[Gate Implementation via Graph Scattering]]
- [[Momentum Filtering on Graphs]]
- [[Column-Subspace Reduction for Symmetric Graphs]] — related symmetry exploitation in quantum walks
- [[Oracle-to-Hamiltonian Simulation via Swap Conjugation]] — from the 2003 paper, used to implement walks on graphs
- [[Continuous-Time Quantum Walk Search]] — related algorithmic primitive
