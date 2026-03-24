> **Source:** Scott Aaronson, Daniel Gottesman, *Improved Simulation of Stabilizer Circuits*, arXiv:quant-ph/0406196, Phys. Rev. A 70, 052328 (2004)
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/0406196) · [Journal](https://doi.org/10.1103/PhysRevA.70.052328)
> **Tags:** #classical-simulation #stabilizer #clifford #complexity #parityl #tableau

---

## The computational problem

**Input:** A stabilizer circuit $C$ — a sequence of CNOT, Hadamard ($H$), and Phase ($S$) gates, plus single-qubit measurements in the computational basis — acting on $n$ qubits initialised in $|0\rangle^{\otimes n}$.

**Task (decision version):** Does qubit 1 output $|1\rangle$ with certainty after applying $C$ to $|0\rangle^{\otimes n}$? (Measurement outcomes for stabilizer states are either deterministic or uniformly random — no intermediate probabilities.)

**Simulation task:** Produce correct measurement outcomes and maintain an internal representation of the current stabilizer state after each gate and measurement.

---

## What the paper does

The [[Gottesman-Knill theorem]] says stabilizer circuits are classically simulable in polynomial time. This paper asks: *how efficiently*, and *how hard, exactly*? It gives a faster simulation algorithm (O(n²) per measurement instead of O(n³)), shows the simulation problem is complete for ⊕L (so stabilizer circuits are probably weaker than full classical computation), and proves a canonical form theorem bounding circuit size at O(n²/log n) gates.

The improved algorithm is implemented in **CHP** (CNOT-Hadamard-Phase), a freely available program that handles thousands of qubits.

---

## The algorithm / construction

### Tableau representation

The state is stored as a $2n \times (2n+1)$ binary tableau, plus a scratch row. Rows $1\ldots n$ are **destabilizer generators**; rows $n+1\ldots 2n$ are **stabilizer generators**.

For each row $i$ and qubit $j$, two bits $(x_{ij}, z_{ij})$ encode the Pauli on qubit $j$:

| $(x,z)$ | Pauli |
|---|---|
| $(0,0)$ | $I$ |
| $(1,0)$ | $X$ |
| $(1,1)$ | $Y$ |
| $(0,1)$ | $Z$ |

Each row has a phase bit $r_i \in \{0,1\}$ for the overall sign ($0 \to +1$, $1 \to -1$).

**Initial tableau for $|0\rangle^{\otimes n}$:**
- Destabilizers (rows $1\ldots n$): $x_{ij} = \delta_{ij}$, $z_{ij} = 0$, $r_i = 0$ — these are $X_1, X_2, \ldots, X_n$.
- Stabilizers (rows $n+1\ldots 2n$): $x_{ij} = 0$, $z_{ij} = \delta_{(i-n)j}$, $r_i = 0$ — these are $Z_1, Z_2, \ldots, Z_n$.

The destabilizer/stabilizer invariant:
- $(i)$ Stabilizer rows generate $S(|\psi\rangle)$; all $2n$ rows generate the full Pauli group $\mathcal{P}_n$.
- $(ii)$ Destabilizer rows mutually commute.
- $(iii)$ Row $h$ anticommutes with row $h+n$.
- $(iv)$ Row $i$ commutes with row $h+n$ for $i \neq h$.

### rowsum$(h, i)$ subroutine

Replaces row $h$ by the Pauli product $R_h \leftarrow R_h \cdot R_i$, tracking the phase.

Define $g(x_1,z_1,x_2,z_2) \in \{-1,0,1\}$ as the exponent $e$ such that Pauli $(x_1,z_1)$ times $(x_2,z_2)$ contributes $i^e$ to the phase.

Compute:
$$S = \bigl(2r_h + 2r_i + \sum_{j=1}^n g(x_{ij}, z_{ij}, x_{hj}, z_{hj})\bigr) \bmod 4$$

Set $r_h = 0$ if $S = 0$, else $r_h = 1$ (if $S = 2$). $S$ is always 0 or 2 for valid stabilizer rows.

Then: $x_{hj} \leftarrow x_{hj} \oplus x_{ij}$, $z_{hj} \leftarrow z_{hj} \oplus z_{ij}$ for all $j$.

### Gate update rules (applied to all rows $i = 1\ldots 2n$)

**CNOT(control $a$ → target $b$):**
$$r_i \leftarrow r_i \oplus x_{ia} z_{ib}(x_{ib} \oplus z_{ia} \oplus 1)$$
$$x_{ib} \leftarrow x_{ib} \oplus x_{ia}, \quad z_{ia} \leftarrow z_{ia} \oplus z_{ib}$$

**Hadamard on qubit $a$:**
$$r_i \leftarrow r_i \oplus x_{ia} z_{ia}$$
$$\text{swap } x_{ia} \text{ and } z_{ia}$$

**Phase ($S$) on qubit $a$:**
$$r_i \leftarrow r_i \oplus x_{ia} z_{ia}, \quad z_{ia} \leftarrow z_{ia} \oplus x_{ia}$$

Each gate costs $O(n)$ time (one pass over the tableau row).

### Measurement of qubit $a$

**Case I (random outcome):** Some stabilizer row $p \in \{n+1\ldots 2n\}$ has $x_{pa} = 1$.
1. For all rows $i \neq p$ with $x_{ia} = 1$: call rowsum$(i, p)$.
2. Set destabilizer row $p-n$ := stabilizer row $p$.
3. Reset stabilizer row $p$ to all zeros, set $r_p$ uniformly in $\{0,1\}$, set $z_{pa} = 1$.
4. Return $r_p$ as the measurement outcome.

Cost: $O(n^2)$ (rowsum called $\leq 2n$ times, each $O(n)$).

**Case II (deterministic outcome):** No stabilizer row has $x_{pa} = 1$.
1. Initialise scratch row (row $2n+1$) to zeros.
2. For all destabilizer rows $i \in \{1\ldots n\}$ with $x_{ia} = 1$: call rowsum$(2n+1, i+n)$.
3. Return $r_{2n+1}$ as the outcome (no state update).

Cost: $O(n^2)$ worst case.

**Why this is faster than Gaussian elimination:** The original Gottesman-Knill analysis had to bring the tableau into reduced row echelon form ($O(n^3)$) to determine measurement outcomes. The destabilizer representation lets you locate the right pivot row directly, replacing elimination with at most $O(n)$ rowsum operations.

---

## Key results

**Theorem 1 (Simulation complexity):**  A stabilizer circuit on $n$ qubits with $m$ gates and measurements can be simulated using $O(n^2)$ bits of space and $O(n)$ time per unitary gate, $O(n^2)$ time per measurement.

**Theorem 2 (Complexity classification):**  The Gottesman-Knill decision problem (given a stabilizer circuit, does qubit 1 output $|1\rangle$ with certainty?) is complete for $\oplus$L (parity-L) under $\leq^{\log}_m$ reductions.

*Consequence:* Stabilizer circuits are contained in $\oplus$L $\subseteq$ P, and adding Hadamard and Phase to CNOT gives no computational gain beyond $\oplus$L. This strongly suggests stabilizer circuits are not classical-computation-universal.

**Theorem 3 (Canonical form):**  Any unitary $n$-qubit stabilizer circuit is equivalent to a circuit in the 11-round form:
$$H - C - P - C - P - C - H - P - C - P - C$$
where $H$, $C$, $P$ denote layers of (possibly parallel) Hadamard, CNOT, and Phase gates respectively.

**Corollary (Gate count bound):**  Using the Patel-Markov-Hayes CNOT minimisation, any stabilizer circuit has an equivalent circuit with $O(n^2/\log n)$ gates.

**Theorem 4 (Inner product):**  The inner product $\langle \psi | \phi \rangle$ between two $n$-qubit stabilizer states can be computed in $O(n^2)$ time and $O(n^2)$ space.

---

## Comparison with prior work

| Property | Gottesman-Knill (original) | This paper |
|---|---|---|
| Representation | $n$ stabilizer generators (no destabilizers) | $2n \times (2n+1)$ tableau (stabilizers + destabilizers) |
| Space | $O(n^2)$ bits | $O(n^2)$ bits (factor-2 larger) |
| Unitary gate cost | $O(n)$ | $O(n)$ (same) |
| Measurement cost | $O(n^3)$ (Gaussian elimination) | $O(n^2)$ (direct pivot using destabilizers) |
| Complexity class | not characterised | $\oplus$L-complete |
| Canonical form | not given | $O(n^2/\log n)$ gates |
| Inner product | not given | $O(n^2)$ |
| Implementation | none publicly available | CHP program (handles thousands of qubits) |

The key trade-off: a factor-of-2 increase in memory eliminates the need for Gaussian elimination in measurements. Good deal.

---

## Limits / caveats

- The $O(n^2)$ measurement cost is a worst-case bound. Random circuits exhibit a phase transition around $\Theta(n \log n)$ random unitary gates; below this, measurements are cheap; above, they hit the quadratic worst case.
- The canonical form is for *unitary* stabilizer circuits only — does not directly handle mid-circuit measurements.
- Extensions to mixed states and limited non-stabilizer gates are given but at exponential cost in the number of non-stabilizer operations:
  - $d$ non-stabilizer gates each acting on $\leq b$ qubits: $O(4^{2bd} n + n^2)$ time.
  - General tensor-product initial states with block size $b$ and $d$ total measurements: $O(m \cdot 2^{2b+2d})$ time where $m$ is the number of blocks.
- The paper does not address fault-tolerant computation or magic state distillation — those were later developments.
- The $O(n^2/\log n)$ canonical gate count bound assumes the Patel-Markov-Hayes CNOT lower bound is tight, which it is for generic circuits.

---

## Reusable ideas

1. [[Destabilizer Tableau for Stabilizer Simulation]] — storing destabilizer generators alongside stabilizers eliminates the need for Gaussian elimination in measurement, reducing cost from $O(n^3)$ to $O(n^2)$.
2. [[Rowsum Phase-Tracking in Pauli Group Arithmetic]] — the rowsum subroutine correctly tracks $\pm 1$ phases (including the intermediate $\pm i$ phases from Pauli products) using a single integer accumulation modulo 4.
3. [[Deferred Measurement Principle for Complexity Reduction]] — commuting all measurements to the end of a circuit while replacing them with CNOT-controlled operations, used here to prove ⊕L containment.
4. [[Stabilizer Canonical Form via Gate Type Sorting]] — any Clifford circuit can be put in a fixed 11-layer alternating form by commuting gate types through each other; combined with CNOT minimisation this gives nearly-optimal circuit sizes.

---

## References within this paper

Key papers cited:

- **Gottesman (1998)** — original stabilizer formalism and Gottesman-Knill theorem. The paper this one improves.
- **Patel-Markov-Hayes (2003)** — CNOT minimisation: optimal CNOT circuits use $O(n^2/\log n)$ gates; used for the canonical form corollary.
- **Aaronson (2004)** — complexity-theoretic background on $\oplus$L and its known relations to P and $\oplus$P.
- [[The Computational Complexity of Linear Optics (Aaronson-Arkhipov 2011) — Paper Notes|Aaronson-Arkhipov (2011)]] — later work by Aaronson using related classical-simulation-hardness ideas (BosonSampling).
- [[Classical Simulation of Commuting Quantum Computations Implies Collapse of the Polynomial Hierarchy (Bremner-Jozsa-Shepherd 2011) — Paper Notes|Bremner-Jozsa-Shepherd (2011)]] — IQP hardness; complementary line of work on which circuit classes are classically hard vs. easy.

---

## Cross-links

### Paper notes
- [[Classical Simulation of Commuting Quantum Computations Implies Collapse of the Polynomial Hierarchy (Bremner-Jozsa-Shepherd 2011) — Paper Notes]] — IQP circuits are classically hard (complementary result: stabilizer circuits are easy)
- [[The Computational Complexity of Linear Optics (Aaronson-Arkhipov 2011) — Paper Notes]] — same first author; hardness of classical simulation of linear optical circuits
- [[The Solovay-Kitaev Algorithm (Dawson-Nielsen 2005) — Paper Notes]] — Clifford gate compilation
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes]] — stabilizer language (Kitaev's stabilizer problem)
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes]] — Shor's circuit contains non-stabilizer gates; the T-gate is what makes it hard

### Trick cards
- [[Destabilizer Tableau for Stabilizer Simulation]]
- [[Rowsum Phase-Tracking in Pauli Group Arithmetic]]
- [[Deferred Measurement Principle for Complexity Reduction]]
- [[Stabilizer Canonical Form via Gate Type Sorting]]
