> **Source:** Edward Farhi, Jeffrey Goldstone, and Sam Gutmann, *A Quantum Approximate Optimization Algorithm*, arXiv:1411.4028 (2014)
> **Links:** [arXiv](https://arxiv.org/abs/1411.4028)
> **Tags:** #QAOA #variational #combinatorial-optimization #MaxCut #approximation #NISQ

---

## The computational problem

**Combinatorial optimisation:** Given $n$ bits and $m$ clauses, where each clause $C_\alpha$ is a constraint on a subset of bits, maximise the objective function:

$$C(z) = \sum_{\alpha=1}^{m} C_\alpha(z)$$

i.e., find a bit string $z$ satisfying as many clauses as possible. This covers MaxCut, MaxSAT, Max-$k$-XOR, and constraint satisfaction problems generally.

The paper focuses on **approximate optimisation** — finding $z$ with $C(z)$ close to the maximum, not necessarily achieving it exactly.

---

## What the paper does

Introduces the **Quantum Approximate Optimization Algorithm (QAOA)**, a parametrised quantum circuit for approximate combinatorial optimisation. The circuit alternates between a phase-separation operator (encoding the objective) and a mixing operator (single-qubit $X$ rotations), applied $p$ times with $2p$ tuneable angles. The approximation quality improves with $p$, and the algorithm converges to the exact optimum as $p \to \infty$.

For the showcase problem of MaxCut on 3-regular graphs at $p = 1$, they prove a worst-case approximation ratio of $\geq 0.6924$.

This paper launched an enormous research programme and became one of the most-cited papers in quantum computing. It also popularised the idea of hybrid quantum-classical variational algorithms for near-term devices. That said, **whether QAOA offers any practical speedup over classical algorithms remains an open question** — and for the specific problems analysed in this paper and its immediate follow-up, classical algorithms have since matched or beaten the QAOA results.

---

## The algorithm

### Operators

Start from the uniform superposition $|s\rangle = |+\rangle^{\otimes n}$. Define:

**Phase-separation unitary** (encodes the objective):
$$U(C, \gamma) = e^{-i\gamma C} = \prod_{\alpha=1}^{m} e^{-i\gamma C_\alpha}$$

All terms commute since $C$ is diagonal in the computational basis. Each factor has locality equal to the clause it encodes.

**Mixing unitary** (enables exploration):
$$U(B, \beta) = e^{-i\beta B} = \prod_{j=1}^{n} e^{-i\beta \sigma_j^x}$$

where $B = \sum_j \sigma_j^x$.

### The QAOA$_p$ state

For integer $p \geq 1$ and angles $\boldsymbol{\gamma} = (\gamma_1, \ldots, \gamma_p)$, $\boldsymbol{\beta} = (\beta_1, \ldots, \beta_p)$:

$$|\boldsymbol{\gamma}, \boldsymbol{\beta}\rangle = U(B, \beta_p) U(C, \gamma_p) \cdots U(B, \beta_1) U(C, \gamma_1) |s\rangle$$

The expected objective value is $F_p(\boldsymbol{\gamma}, \boldsymbol{\beta}) = \langle \boldsymbol{\gamma}, \boldsymbol{\beta}| C |\boldsymbol{\gamma}, \boldsymbol{\beta}\rangle$, and the algorithm optimises over the angles to maximise this.

### Circuit depth

Each layer costs at most $m + n$ gates (one per clause plus one per qubit for the mixer). Total depth: $O(p \cdot m)$.

### Angle selection

Two regimes:

- **Fixed $p$ (not growing with $n$):** For bounded-degree graphs, there are only finitely many distinct local subgraph types, so $F_p(\boldsymbol{\gamma}, \boldsymbol{\beta})$ can be computed classically in time independent of $n$ (see locality argument below). The optimal angles are found by classical optimisation and fed to the quantum computer. The quantum computer is only needed for *sampling* from $|\boldsymbol{\gamma}, \boldsymbol{\beta}\rangle$.

- **$p$ growing with $n$:** Classical angle optimisation becomes infeasible. The paper suggests gradient-based methods but doesn't resolve this case.

---

## The locality argument (the clever part)

For MaxCut on bounded-degree graphs at fixed $p$, the expectation value $F_p$ decomposes as a sum over edges:

$$F_p(\boldsymbol{\gamma}, \boldsymbol{\beta}) = \sum_{\langle jk \rangle} f_{g(j,k)}(\boldsymbol{\gamma}, \boldsymbol{\beta})$$

where $g(j,k)$ is the local subgraph within distance $p$ of edge $\langle jk \rangle$, and $f_g$ depends only on the isomorphism type of this subgraph.

For a graph with maximum degree $v$, each subgraph contains at most $2[(v-1)^{p+1} - 1]/[(v-1) - 1]$ qubits — independent of $n$. The number of distinct subgraph types is also $n$-independent. So the entire function $F_p(\boldsymbol{\gamma}, \boldsymbol{\beta})$ is determined by the *counts* of each subgraph type (easy to compute classically) times the *values* of each $f_g$ (small quantum or classical computation).

This is a nice structural insight: for fixed $p$ on bounded-degree problems, the classical preprocessing fully determines the optimal angles.

### Concentration

The variance of $C(z)$ when measuring $|\boldsymbol{\gamma}, \boldsymbol{\beta}\rangle$ is $O(m)$, so the distribution concentrates: repeating $O(m \log m)$ times gives a string with $C(z) \geq F_p - 1$ with high probability.

---

## Key results

**MaxCut on 3-regular graphs, $p = 1$:**

The approximation ratio is $\geq 0.6924$ in the worst case. This was computed by explicit evaluation of the $p = 1$ expectation over the three possible local subgraph types for 3-regular graphs.

**Monotonicity:** $M_p \geq M_{p-1}$ — more layers never hurt.

**Convergence:** $\lim_{p \to \infty} M_p = \max_z C(z)$ — QAOA converges to exact optimisation as $p \to \infty$. (This follows from the connection to the [[Quantum Computation by Adiabatic Evolution (Farhi-Goldstone-Gutmann-Sipser 2000) — Paper Notes|quantum adiabatic algorithm]] via Trotterisation.)

---

## The classical algorithms caught up

For the specific problems highlighted in the original QAOA papers, classical algorithms have since matched or surpassed the quantum results. This doesn't close the door on QAOA — it motivates looking at different problem families and higher depth — but it's important context:

### Max-E3LIN2 (bounded occurrence)

In the follow-up paper (arXiv:1412.6062), Farhi, Goldstone & Gutmann applied QAOA$_1$ to bounded-occurrence Max-E3LIN2 and achieved an approximation beating random assignment by $\Omega(1/D^{3/4})$ where $D$ bounds the number of constraints per variable. At the time, this was the best known algorithm for this problem.

Within a year, **Barak, Moitra, O'Donnell, Raghavendra, Regev, Steurer, Trevisan, Vijayaraghavan, Witmer & Wright (2015)** ([arXiv:1505.03424](https://arxiv.org/abs/1505.03424), APPROX/RANDOM 2015) gave a *classical* algorithm achieving $\Omega(1/\sqrt{D})$ — both qualitatively and quantitatively better. The quantum advantage on this particular problem didn't hold, though the QAOA framework itself opened the door to the broader research programme.

### MaxCut on triangle-free graphs

For MaxCut on $D$-regular triangle-free graphs, **Hastings (2019)** ([arXiv:1905.07047](https://arxiv.org/abs/1905.07047)) showed that a classical "threshold" algorithm with optimised parameters outperforms QAOA$_1$ on *all* triangle-free instances. **Marwaha (2021)** ([Quantum](https://quantum-journal.org/papers/q-2021-04-20-437/), [arXiv:2101.05513](https://arxiv.org/abs/2101.05513)) extended this: a local classical algorithm outperforms QAOA$_2$ on high-girth regular graphs.

### Current status

The Goemans-Williamson SDP-based algorithm achieves approximation ratio $\geq 0.878$ for MaxCut on general graphs (assuming UGC, this is optimal). QAOA$_1$'s $0.6924$ on 3-regular graphs is well below this. Whether QAOA at large $p$ can compete with or surpass the best classical algorithms for any natural problem family remains open.

---

## Subsequent developments

### Sampling hardness and quantum supremacy (Farhi & Harrow 2016)

Farhi & Harrow (arXiv:1602.07674) proved that even QAOA$_1$ — the shallowest possible version — produces output distributions that cannot be efficiently sampled classically, assuming the Polynomial Hierarchy doesn't collapse. The argument shows that a classical sampler for QAOA$_1$ output would imply PH collapse, even if one has an oracle for sampling from the output of a gapped stoquastic [[Quantum Computation by Adiabatic Evolution (Farhi-Goldstone-Gutmann-Sipser 2000) — Paper Notes|quantum adiabatic algorithm]].

This is a complexity-theoretic separation, not a practical one — it says QAOA produces *hard-to-simulate distributions*, regardless of whether those distributions are useful for optimisation. It positions QAOA as a candidate for demonstrating quantum computational supremacy, independent of its optimisation performance.

### Performance analysis and mechanism (Zhou, Wang, Choi, Pichler & Lukin 2020)

Zhou et al. (PRX 2020, arXiv:1812.01041, 1465 citations) provided the first in-depth study of QAOA performance, mechanism, and near-term implementation:
- Developed efficient parameter optimisation strategies (interpolation-based, avoiding brute-force grid search)
- Showed QAOA can exploit *non-adiabatic* mechanisms — the optimal angles don't always correspond to a smooth adiabatic path, meaning QAOA can find shortcuts that the adiabatic algorithm misses
- Benchmarked on MaxCut instances up to 22 qubits, with comparisons to the adiabatic algorithm and simulated annealing
- Studied the effect of realistic hardware noise on QAOA performance

### The Sherrington-Kirkpatrick model (Farhi, Goldstone, Gutmann & Zhou 2019–2022)

Farhi, Goldstone, Gutmann & Zhou (arXiv:1910.08187, Quantum 2022) analysed QAOA on the SK model (fully connected Ising spin glass) at infinite size. They developed a framework for computing QAOA performance on dense random instances and showed QAOA$_p$ at moderate $p$ finds energies approaching but not reaching the Parisi value. Whether QAOA achieves the Parisi value as $p \to \infty$ remains conjectured but unproven. It also remains open whether QAOA can compete with Montanari's classical algorithm for the SK Hamiltonian.

### High-depth QAOA (Basso, Farhi, Marwaha, Villalonga & Zhou 2021)

Basso et al. (arXiv:2110.14206) developed a more efficient recursive procedure for analysing QAOA at high depth on large-girth regular graphs and the SK model, pushing numerical analysis to $p = 20$. The approximation ratio improves with $p$, but convergence is slow.

### QAOA limitations at constant depth (Basso, Gamarnik, Mei & Zhou, FOCS 2022)

Proved performance limitations of constant-level QAOA on large sparse hypergraphs and spin glass models, connecting to the overlap gap property — a structural barrier to local algorithms. This is evidence that QAOA at fixed $p$ cannot beat certain classical algorithmic thresholds for these problems.

### Maximum Independent Set on Rydberg arrays (Ebadi et al., Science 2022)

Implemented QAOA-like variational optimisation for MIS on Rydberg atom arrays (up to 289 atoms). Notable as a physical implementation, though the quantum advantage question remains unresolved — classical solvers can handle these instance sizes.

### Near-symmetric optimisation problems (Montanaro & Zhou 2024)

Montanaro & Zhou (arXiv:2411.04979) proved that QAOA$_1$ achieves success probability $\Omega(1/\sqrt{n})$ (sometimes $\Omega(1)$) for finding exact solutions to near-symmetric combinatorial optimisation problems with planted solutions. They constructed instances where $O(1)$ quantum queries suffice vs $\Omega(n/\log n)$ classical queries — an *exponential separation* for planted problems. They benchmarked against state-of-the-art classical SAT solvers, finding instances where all known classical algorithms require exponential time. This is currently one of the strongest theoretical cases for QAOA speedup, though the problem families are somewhat synthetic.

### QAOA for local Hamiltonian problems (Kannan, King & Zhou 2024)

Extended QAOA beyond classical combinatorial optimisation to quantum local Hamiltonian problems (arXiv:2412.09221). This expands the algorithmic scope of the framework.

### Hardware implementations

Harrigan et al. (Nature Physics 2021, Google) implemented QAOA on a superconducting processor for MaxCut on non-planar graphs, demonstrating the algorithm runs on real hardware but without claiming quantum advantage over classical solvers.

---

## Has QAOA demonstrated any speedup?

**Not yet, for practical problems.** The honest summary as of early 2026:

- **No proven asymptotic speedup** over classical algorithms for any standard combinatorial optimisation problem.
- **Sampling hardness:** Farhi & Harrow (2016) showed QAOA output distributions are classically hard to sample (assuming PH doesn't collapse). This is a computational complexity result, not an optimisation speedup — the distributions are hard to *reproduce*, whether or not they're useful.
- **Near-symmetric planted problems:** Montanaro & Zhou (2024) proved exponential quantum-classical query separations for QAOA on constructed problem families with planted solutions. Whether these translate to practical advantage on natural problems is open.
- **LABS scaling evidence:** Shaydulin et al. (Science Advances 2024) observed better-than-classical scaling for QAOA on LABS in noiseless simulations up to 40 qubits. Whether this survives noise and scales further is unknown.
- **Near-term hardware limitations** remain severe. Even with error detection (e.g., iceberg codes), demonstrated improvements are incremental.
- **The variational parameter optimisation** is itself a hard problem for large $p$, with barren plateaus and local minima.

The question of whether QAOA offers practical quantum advantage for combinatorial optimisation is genuinely open. It hasn't been ruled out, but it hasn't been demonstrated either.

> **📋 TODO:** The subsequent developments section above is a sketch, not comprehensive coverage. QAOA has generated a large body of work that deserves fuller treatment, including:
> - Detailed comparison of QAOA performance on specific problem families (MaxCut variants, LABS, SK model, Max-$k$-XOR) with best known classical results
> - Warm-starting and recursive QAOA variants
> - Constrained optimisation with $XY$-mixers and other structured mixers (Hadfield et al.)
> - Overlap gap property and algorithmic barriers (Gamarnik et al.)
> - QAOA on hardware: noise effects, error mitigation, error detection results
> - Parameter transferability and concentration phenomena
> - Connections to quantum annealing and bang-bang control
> - Whether the LABS scaling advantage and Montanaro-Zhou near-symmetric results generalise to natural problem instances
> 
> Consider splitting into separate paper notes for the most significant follow-ups.

---

## Comparison with related approaches

| Approach | Type | Approximation for MaxCut (3-reg) | Notes |
|---|---|---|---|
| Goemans-Williamson (1995) | Classical SDP | $\geq 0.878$ (general graphs) | Best known classical; likely optimal under UGC |
| QAOA$_1$ (this paper) | Quantum variational | $\geq 0.6924$ | Fixed-depth, efficient classical preprocessing |
| QAOA$_2$ | Quantum variational | Better than $p=1$ but beaten by classical local algorithms on high-girth graphs | Marwaha (2021) |
| Classical threshold (Hastings 2019) | Classical local | Beats QAOA$_1$ on triangle-free instances | Simple, no quantum resources needed |
| Barak et al. (2015) | Classical | $\Omega(1/\sqrt{D})$ for Max-E3LIN2 | Beats QAOA$_1$'s $\Omega(1/D^{3/4})$ |
| [[Quantum Computation by Adiabatic Evolution (Farhi-Goldstone-Gutmann-Sipser 2000) — Paper Notes|Quantum adiabatic]] | Quantum continuous-time | Converges to exact; no proven speedup | QAOA is the Trotterised version |
| [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes|VQE]] (Peruzzo et al. 2014) | Quantum variational | Problem-dependent | Different ansatz, same variational paradigm |

---

## Limits / caveats

- **No proven speedup.** For every problem where QAOA$_p$ at small $p$ has been analysed, classical algorithms match or beat it. The conjecture that large $p$ might help is plausible but unproven.

- **Barren plateaus.** As the number of qubits grows, the cost function landscape can become exponentially flat, making gradient-based optimisation of the angles infeasible. This is a general problem for variational quantum algorithms.

- **$p$ scaling.** The convergence guarantee ($M_p \to \max$) requires $p \to \infty$, but the classical optimisation of $2p$ angles becomes intractable for large $p$. The fixed-$p$ regime has efficient preprocessing but limited approximation quality.

- **Hardware noise.** Current quantum hardware cannot run QAOA at the circuit depths needed for meaningful $p$ on interesting problem sizes. Error rates compound across layers.

---

## Reusable ideas

1. **[[Alternating Operator Ansatz]]:** The $U(C, \gamma) U(B, \beta)$ structure — alternate between a phase operator encoding the problem and a mixing operator enabling transitions. This is more general than QAOA and forms the basis for many variational quantum algorithms. Different mixing operators (e.g., $XY$-mixers preserving Hamming weight) give constrained optimisation variants.

2. **[[Locality-Based Classical Preprocessing for Variational Circuits]]:** For bounded-degree problems at fixed circuit depth $p$, the expectation value decomposes into contributions from local subgraphs of bounded size. The optimal variational parameters can be found classically in time independent of $n$, and the quantum computer is only needed for sampling. This idea extends beyond QAOA to any variational circuit with bounded-range interactions.

---

## References within this paper

- [[Quantum Computation by Adiabatic Evolution (Farhi-Goldstone-Gutmann-Sipser 2000) — Paper Notes|Farhi, Goldstone, Gutmann & Sipser (2000)]] — quantum adiabatic algorithm; QAOA is its Trotterised version
- Farhi, Goldstone & Gutmann (2002) — quantum adiabatic algorithm for satisfiability (arXiv:quant-ph/0001106)
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover (1996)]] — unstructured search; QAOA can reproduce Grover-like scaling for search
- Goemans & Williamson (1995) — SDP-based MaxCut approximation, the classical benchmark
- [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes|Peruzzo et al. (2014)]] — VQE, the other foundational variational quantum algorithm from the same year

---

## Cross-links

### Paper notes
- [[Quantum Computation by Adiabatic Evolution (Farhi-Goldstone-Gutmann-Sipser 2000) — Paper Notes]] — QAOA as Trotterised adiabatic evolution
- [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes]] — VQE, the parallel variational approach for eigenvalue problems
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]] — Grover search as a special case of the alternating operator structure
- [[Adiabatic Quantum Computation is Equivalent to Standard Quantum Computation (Aharonov-van Dam-Kempe-Landau-Lloyd-Regev 2004) — Paper Notes]] — adiabatic ↔ circuit equivalence, relevant to QAOA convergence

### Trick cards
- [[Alternating Operator Ansatz]] — the circuit structure generalised
- [[Locality-Based Classical Preprocessing for Variational Circuits]] — the fixed-$p$ subgraph decomposition
- [[Standard Amplitude Amplification]] — QAOA can be viewed as a structured generalisation
