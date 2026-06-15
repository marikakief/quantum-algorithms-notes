> **Source:** Craig Gidney, *Halving the cost of quantum addition*, arXiv:1709.06648, Quantum **2**, 74 (2018)
> **Links:** [arXiv](https://arxiv.org/abs/1709.06648) · [Journal](https://doi.org/10.22331/q-2018-06-18-74)
> **Tags:** #quantum-arithmetic #T-complexity #fault-tolerant #surface-code #Toffoli #adder #circuit-primitive

---

## The computational problem

Perform $n$-bit binary addition on a fault-tolerant quantum computer using the surface code, where runtime is dominated by the number of T gates (because $|T\rangle$ state distillation is the bottleneck). The goal is to minimise the T-count of the adder circuit.

## What the paper does

Reduces the T-count of an $n$-bit quantum ripple-carry adder from $8n + O(1)$ to $4n + O(1)$ — a factor-of-two improvement that had stood for over a decade. The construction replaces paired Toffoli gates (compute/uncompute) with **temporary logical-AND** operations that cost 4 T gates to compute and 0 T gates to erase. The paper also presents an $8n + O(1)$ controlled adder and an out-of-place adder whose inverse is T-free.

The main conceptual contribution is not the adder itself but the reframing of Jones's ancilla-and-fixup construction into the **temporary logical-AND** — a general-purpose circuit primitive applicable anywhere Toffoli gates appear in compute/uncompute pairs.

## The algorithm / construction

### Temporary logical-AND

The core primitive. To compute $|x\rangle|y\rangle|0\rangle \to |x\rangle|y\rangle|x \wedge y\rangle$:

**Computation (T-count 4, measurement-depth 1):**

```
x ───•─────────────── x
     │   T†
y ───•─────────────── y
     │   T†
|T⟩──⊕───T──•──H──S── x∧y
```

The circuit uses a $|T\rangle$ state as input, applies two $T^\dagger$ gates on the controls, a CNOT, a $T$ gate, a Hadamard, and an $S$ fixup. Total: 4 T gates (including the $|T\rangle$ state consumed as input).

**Uncomputation (T-count 0, measurement-depth 1):**

```
x ───•─────── x
     │
y ───•─────── y
     │   Z
x∧y ─H───•─── (measured and erased)
```

Measure the ancilla in the X basis (Hadamard then measure), apply a classically conditioned $Z$ correction to one of the controls. This uses only Clifford gates, so the T-count is zero.

The asymmetry between computation (4 T) and uncomputation (0 T) comes from the irreversibility of measurement. The uncomputation exploits measurement in a way the computation cannot.

### The adder

The adder is a variant of the Cuccaro ripple-carry adder, which uses $2n + O(1)$ Toffoli gates in compute/uncompute pairs to propagate carries. In Cuccaro's original construction, each Toffoli costs 4 T gates (using the matched-phase-error trick from Barenco et al. (1995)), giving $8n + O(1)$ T gates total.

Gidney replaces each compute/uncompute Toffoli pair with a temporary logical-AND: the compute half costs 4 T gates, and the uncompute half costs 0 T gates. Since the adder has $n - 1$ such pairs, the total T-count is $4(n-1) = 4n - 4$.

The building block for bit position $k$:

```
c_k ──•──•──•──•── c_k
i_k ──•──────•──── i_k
t_k ──•──⊕──────── (t+i)_k
c_{k+1}  ...       c_{k+1}
```

Each block uses one temporary logical-AND (T-count 4) and Clifford operations. The carry qubit $c_{k+1}$ stores the logical-AND of $c_k$ and $i_k$, propagated through the block.

**Resources for the $n$-bit adder:**
- T-count: $4n - 4$
- Ancilla qubits: $n - 1$ (for the temporary logical-AND outputs)
- Measurement depth: $2n - 2$ (same as Cuccaro)

### Controlled adder

A controlled adder (addition conditioned on a control qubit) with T-count $8n + O(1)$. Each building block uses two temporary logical-ANDs instead of one, incorporating the control qubit. This improves the previous best of $21n + O(1)$ from Muoz-Coreas and Thapliyal (2017).

### Out-of-place adder

An out-of-place adder computing $|a\rangle|b\rangle|0\rangle \to |a\rangle|b\rangle|a+b\rangle$ where the inverse uses **zero T gates**. This is useful in compute-use-uncompute patterns: the forward pass has a T-count, but the uncomputation is free.

### Opportunity cost of ancillae

The paper includes a careful analysis of the hidden cost of holding ancilla qubits — qubits used as ancillae are qubits not being used in T factories. Using the estimate that one $|T\rangle$ state costs $\sim 960$ spacetime volume units, the opportunity cost of an ancilla is approximately $\frac{1}{480}|T\rangle$ states per measurement-depth step.

Including opportunity cost under the paper's base storage model, the effective T-count of the Gidney adder is roughly $\frac{1}{480}n^2 + 4n$, vs $8n$ for Cuccaro, giving a crossover near $n=960$. With the memory-compressed idle-qubit assumption discussed in the paper, the crossover can move to about $n=5760$. Thus an RSA-4096-scale adder is favorable only under the latter storage assumption; the raw T-count advantage alone is not the whole resource comparison.

A **hybrid adder** is optimal: use temporary logical-ANDs for bit positions below a cutoff, and inline carry propagation (Cuccaro-style) above the cutoff.

**Raw T-count vs effective T-count.** The raw circuit count says temporary logical-ANDs halve the T gates in a ripple-carry adder. The effective surface-code cost also charges qubits held as temporary ANDs for the time they occupy hardware that could otherwise distill T states. This is why the optimal design can be hybrid even when the raw T-count of the all-temporary-AND adder is lower.

## Key results

**Temporary logical-AND:**
$$T_{\text{compute}} = 4, \quad T_{\text{erase}} = 0$$

**$n$-bit adder:**
$$T\text{-count} = 4n - 4, \quad \text{ancilla} = n - 1, \quad \text{meas-depth} = 2n - 2$$

**$n$-bit controlled adder:**
$$T\text{-count} = 8n + O(1)$$

**$n$-bit out-of-place adder (inverse):**
$$T\text{-count}_{\text{inverse}} = 0$$

**Multi-controlled NOT ($n$ controls):**
$$T\text{-count} = 4n - 4$$

**Application to Shor's algorithm (2048-bit RSA):** reduces the estimated $|T\rangle$ state cost from $2 \times 10^{12}$ to $8 \times 10^{11}$ and measurement-depth from 27 hours to 9 hours.

## Comparison with prior work

| Construction | T-count ($n$-bit add) | Ancilla | Measurement depth | Notes |
|---|---|---|---|---|
| Cuccaro (2004) with paired Toffolis | $8n + O(1)$ | 1 | $2n + O(1)$ | Matched phase errors |
| Jones (2013) per-Toffoli | $8n + O(1)$ | $n$ | $n + O(1)$ | Ancilla-and-fixup per gate |
| **Gidney (2018)** | $4n - 4$ | $n - 1$ | $2n - 2$ | Temporary logical-AND |
| Draper et al. (2004) log-depth | $O(n \log n)$ raw | $O(n)$ | $O(\log n)$ | Better effective T-count for large $n$ |

The factor-of-2 improvement applies to circuits exposing suitable compute/uncompute Toffoli pairs satisfying the phase-insensitivity condition, not to arbitrary Toffoli networks. Clean compute-phase-uncompute [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover]] predicates, multi-controlled NOT constructions, and many [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes|chemistry circuits]] are good examples when the paired structure is present.

## Limits / caveats

1. **Ancilla cost:** The adder uses $n - 1$ ancilla qubits, each held through $O(n)$ measurement-depth steps. For very large $n$ (above $\sim 960$–$5760$ depending on memory compression), the opportunity cost of these ancillae exceeds the T-gate savings. A hybrid approach is needed.

2. **Phase sensitivity:** The temporary logical-AND introduces a phase error on the controls during the interval the ancilla is held. Intermediate operations must be insensitive to this phase error. This condition is satisfied when the AND appears in a compute/uncompute pair, but not in general. See Figure 5 in the paper for the precise condition.

3. **Measurement requirement:** The zero-T uncomputation relies on mid-circuit measurement and classical feedforward. This is natural in the surface code (where measurement is the basic operation) but adds latency in other architectures.

4. **Lower bound proximity:** Howard and Campbell (2017) proved a single Toffoli requires at least 4 T gates. Whether $n$ Toffolis can be done in fewer than $4n$ T gates (for large $n$) remains open. The temporary logical-AND saturates this per-gate bound, but there may be room for improvement through inter-gate cancellations.

## Reusable ideas

1. [[Temporary Logical-AND]] — Compute the AND of two qubits into an ancilla for 4 T gates; erase it for 0 T gates using measurement-based uncomputation. Applicable anywhere Toffoli gates appear in compute/uncompute pairs.

2. [[Measurement-Asymmetric Uncomputation]] — Measurement breaks the symmetry between computation and uncomputation, making one direction cheaper than the other. The pattern: compute forward with T gates, uncompute by measuring and classically correcting.

3. [[Ancilla Opportunity Cost Analysis]] — Quantify the hidden cost of holding ancilla qubits by estimating how many $|T\rangle$ states those qubits could have produced if used in T factories instead. Determines the crossover point between ancilla-heavy and ancilla-light circuit variants.

## References within this paper

- [[Addition on a Quantum Computer (Draper 2000) — Paper Notes|Draper (2000)]] — the original QFT-based adder that eliminates carry qubits; Cuccaro et al. (2004) is a follow-up by the same Draper. [quant-ph/0008033](https://arxiv.org/abs/quant-ph/0008033)
- [[A New Quantum Ripple-Carry Addition Circuit (Cuccaro-Draper-Kutin-Moulton 2004) — Paper Notes|Cuccaro, Draper, Kutin, Moulton (2004)]] — the base ripple-carry adder that Gidney modifies. [quant-ph/0410184](https://arxiv.org/abs/quant-ph/0410184)
- Barenco et al. (1995) — paired Toffoli phase-error matching trick; elementary gate decompositions
- Jones (2013) — low-overhead fault-tolerant Toffoli via ancilla-and-fixup. The temporary logical-AND is a reframing of Jones's construction. [Phys. Rev. A **87**, 022328](https://doi.org/10.1103/physreva.87.022328)
- Howard and Campbell (2017) — proves a single Toffoli requires $\geq 4$ T gates. [Phys. Rev. Lett. **118**, 090501](https://doi.org/10.1103/PhysRevLett.118.090501)
- [[A Logarithmic-Depth Quantum Carry-Lookahead Adder (Draper-Kutin-Rains-Svore 2004) — Paper Notes|Draper, Kutin, Rains, Svore (2004)]] — logarithmic-depth carry-lookahead adder. [quant-ph/0406142](https://arxiv.org/abs/quant-ph/0406142)
- Kitaev, Shen, Vyalyi (2002) — phase gradient addition (phase kickback from adding into a Fourier state). See [[Phase Gradient State for Controlled Rotations]] and [[Phase Gradient Rotation Synthesis]].
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes|Babbush, Gidney et al. (2018)]] — chemistry algorithm where temporary logical-ANDs cut T-count by nearly 50%.
- Brent and Kung (1982) — classical parallel prefix adder; Gidney notes that a reversible implementation via temporary logical-ANDs gives T-count $12n$.
- Muoz-Coreas and Thapliyal (2017) — prior controlled adder with T-count $21n + O(1)$.
- Nielsen and Chuang (2009) — textbook Toffoli decomposition using 7 T gates.

## Cross-links

### Paper notes
- [[A Classical-Quantum Adder with Constant Workspace and Linear Gates (Gidney 2025) — Paper Notes]] — Gidney's follow-up: extends measurement-based techniques to classical-quantum addition with $O(1)$ ancillae and $O(n)$ Toffolis
- [[A New Quantum Ripple-Carry Addition Circuit (Cuccaro-Draper-Kutin-Moulton 2004) — Paper Notes]] — the base ripple-carry adder that Gidney modifies; provides the MAJ/UMA paired-Toffoli structure
- [[Addition on a Quantum Computer (Draper 2000) — Paper Notes]] — the original QFT-based quantum adder; Gidney's work improves on the carry-based lineage that branched off from Draper's later ripple-carry construction
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes]] — uses temporary logical-ANDs throughout; this paper is cited as [2]
- [[Improved Fault-Tolerant Quantum Simulation of Condensed-Phase Correlated Electrons via Trotterization (Kivlichan, Gidney, Babbush et al 2020) — Paper Notes]] — extends Gidney's adder into [[Hamming Weight Phasing for Equiangular Rotations]] for Trotter steps
- [[Applying Quantum Algorithms to Constraint Satisfaction Problems (Campbell-Khurana-Montanaro 2019) — Paper Notes]] — uses the temporary logical-AND to halve Grover oracle T-count
- [[Trading T Gates for Dirty Qubits in State Preparation and Unitary Synthesis (Low-Kliuchnikov-Schaeffer 2024) — Paper Notes]] — further T-count/ancilla tradeoffs building on this line of work
- [[Compilation of Fault-Tolerant Quantum Heuristics for Combinatorial Optimization (Sanders, Berry, Babbush et al 2020) — Paper Notes]] — uses phase gradient addition (enabled by Gidney's adder) for rotation synthesis
- [[Factoring using 2n+2 qubits with Toffoli based modular multiplication (Häner-Roetteler-Svore 2017) — Paper Notes]] — earlier Toffoli-based constant adder using dirty ancillae; the [[Temporary Logical-AND]] technique could be applied to its paired Toffoli gates
- [[A Log-Depth In-Place Quantum Fourier Transform (Kahanamoku-Meyer-Blue-Bergamaschi-Gidney-Chuang 2025) — Paper Notes]] — cites Gidney (2018) for phase gradient technique applicable to local QFT blocks
- [[Approximate Encoded Permutations and Piecewise Quantum Adders (Gidney 2019) — Paper Notes]] — Gidney's follow-up using approximate piecewise addition with oblivious carry runways; uses the Gidney (2018) ripple-carry adder on each piece

### Trick cards
- [[Temporary Logical-AND]] — the main primitive from this paper
- [[Carry Venting via X-Basis Measurement]] — extends measurement-based erasure from AND gates to carry qubits; introduced in [[A Classical-Quantum Adder with Constant Workspace and Linear Gates (Gidney 2025) — Paper Notes|Gidney (2025)]]
- [[In-Place Majority for Carry Propagation]] — the MAJ gate from the Cuccaro adder that Gidney optimises
- [[MAJ-UMA Decomposition for Reversible Adders]] — the paired gate structure whose Toffolis become temporary logical-ANDs
- [[Measurement-Asymmetric Uncomputation]] — the underlying principle
- [[Ancilla Opportunity Cost Analysis]] — the resource-theoretic framing
- [[Hamming Weight Phasing for Equiangular Rotations]] — downstream application of the adder
- [[Phase Gradient Rotation Synthesis]] — uses the adder for rotation via phase kickback
- [[Phase Gradient State for Controlled Rotations]] — the resource state that makes addition-based rotation work
- [[Unary Iteration]] — another Gidney primitive (from the 2018 chemistry paper) that uses temporary logical-ANDs
- [[Measurement-Based QROM Uncomputation]] — same measure-and-fixup principle applied to QROM
- [[Depth-Optimised Grover Oracle via Parallel Fan-Out]] — uses Gidney's "uncompute AND" trick
- [[Dirty Qubit Recycling for T-Gate Reduction]] — references Gidney's T-count/ancilla tradeoffs
- [[Sum of Tree Sums for Bit Counting]] — uses measurement-based uncomputation for the tree sums
