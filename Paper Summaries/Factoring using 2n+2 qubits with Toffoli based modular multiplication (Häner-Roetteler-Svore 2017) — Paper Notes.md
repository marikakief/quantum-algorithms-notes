> **Source:** Thomas Häner, Martin Roetteler, Krysta M. Svore, *Factoring using 2n+2 qubits with Toffoli based modular multiplication*, arXiv:1611.07995, Quantum Information and Computation **17**(7&8), 2017
> **Links:** [arXiv](https://arxiv.org/abs/1611.07995) · [Journal](https://doi.org/10.26421/QIC17.7-8)
> **Tags:** #quantum-arithmetic #factoring #Toffoli #Shor #circuit-primitive #adder #dirty-qubits #modular-arithmetic

---

## The computational problem

Implement [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor's factoring algorithm]] for an $n$-bit integer $N$ using the fewest possible qubits, where the dominant subroutine is modular exponentiation:

$$|x\rangle|0\rangle \mapsto |x\rangle|a^x \bmod N\rangle$$

This requires $2n$ controlled modular multiplications, each built from $n$ controlled modular additions. The key design choice is what adder to use: [[Addition on a Quantum Computer (Draper 2000) — Paper Notes|QFT-based]] (Draper/Beauregard/Takahashi) or classical-reversible ([[A New Quantum Ripple-Carry Addition Circuit (Cuccaro-Draper-Kutin-Moulton 2004) — Paper Notes|Cuccaro]]-style). The cost measures are total qubit count, circuit size (gate count), circuit depth, and T-gate overhead under quantum error correction.

## What the paper does

Achieves the same $2n + 2$ qubit count as Takahashi et al. (2006) for Shor's algorithm, but replaces the QFT-based adder with a purely Toffoli-based modular multiplication circuit. The circuit size is $O(n^3 \log n)$ and depth is $O(n^3)$. Because the circuit uses only Toffoli and Clifford gates (no arbitrary rotations), it avoids the $\Theta(\log n)$ overhead from rotation synthesis that plagues QFT-based implementations, and it can be classically simulated for testing and debugging.

The main technical contribution is a new in-place constant adder $|a\rangle \mapsto |a + c\rangle$ using only **dirty ancilla qubits** (qubits in unknown states, borrowed from idle parts of the algorithm). This adder has size $O(n \log n)$ and depth $O(n)$, using between 1 and $n$ dirty ancillae.

## The algorithm / construction

### The constant adder (core innovation)

The adder computes $|a\rangle \mapsto |a + c\rangle$ where $c$ is a classically known constant. It works in three stages:

**Stage 1 — CARRY gate.** Compute the final carry bit $r_{n-1}$ of $a + c$ using $n - 1$ borrowed dirty qubits $g$. The idea: encode carry propagation via toggling of dirty qubits. For each bit position $i$:

- If $c_i = 1$: place a NOT on $a_i$, a CNOT (target $g_i$, control $a_i$), and a Toffoli (target $g_i$, controls $a_i$ and $g_{i-1}$).
- If $c_i = 0$: place only the Toffoli (target $g_i$, controls $a_i$ and $g_{i-1}$).

The Toffoli conditioned on $g_{i-1}$ is placed *before* the potential toggling of $g_{i-1}$, then again *after*, so the two cancel if both execute. The entire sequence runs forward to compute $r_{n-1}$, then backward (minus the output gate) to restore all dirty qubits. The Toffoli count for the CARRY gate is:

$$T_{\text{carry}}(n) = 4(n - 2) + 2$$

**Stage 2 — Divide-and-conquer recursion.** Split the $n$-bit register into high ($x_H$) and low ($x_L$) halves. Compute the carry of $x_L + c_L$ into a single qubit (using $x_H$ as dirty ancillae), then conditionally increment $x_H$ if there is a carry, then recursively add $c_L$ to $x_L$ and $c_H$ to $x_H$.

The conditional incrementer uses the construction from Gidney (2015, StackExchange): using $n$ borrowed dirty ancillae, subtract $g$, bit-flip $g$ (yielding $\bar{g} - 1 = g'$), subtract $g'$, then the net effect is $|x\rangle \mapsto |x + 1\rangle$. Toffoli count: $T_{\text{incr}}(n) = 2(2n - 1)$.

**Stage 3 — Dirty ancilla handling.** When the single available ancilla is dirty rather than clean, the incrementer runs twice with a conditional inversion in between (following the approach of [Gidney 2015]).

The recursion for the total Toffoli count of the $n$-bit addition:

$$T_{\text{add}}(n) = T_{\text{add}}\!\left(\lceil n/2 \rceil\right) + T_{\text{add}}\!\left(\lfloor n/2 \rfloor\right) + 8n - 8 = 8n(\log_2 n - 2) + O(1)$$

### Parallelisation

The carries for the low-half and high-half additions can be computed in parallel if $n/2$ additional dirty qubits are available. During modular multiplication in Shor's algorithm, $n$ qubits of the $x$-register are idle and can be borrowed, reducing the adder depth to $O(n)$.

### Modular multiplication

Following Beauregard (2003) and Takahashi et al. (2006), the modular multiplier is built from the adder:

1. **Modular addition** $|b\rangle \mapsto |(a + b) \bmod N\rangle$: compare $b$ to $N - a$ (using the CARRY gate on inverted bits), then either add $a$ or subtract $N - a$, then reset the comparison qubit with a second comparison.

2. **Modular multiplication** via repeated-addition-and-shift: $|x\rangle|0\rangle \mapsto |x\rangle|(ax) \bmod N\rangle$ using $n$ controlled modular additions of precomputed constants $(a \cdot 2^i) \bmod N$.

3. **Uncomputation** via [[Reversible Modular Exponentiation with Garbage Cleanup|Beauregard's swap-and-inverse trick]]: after computing $(ax) \bmod N$, swap the two $n$-qubit registers, then run the inverse multiplication by $a^{-1} \bmod N$. This cleans the $x$-register back to $|0\rangle$.

Total qubits: $2n + 1$ for the two registers plus the comparison qubit. An extra qubit for the semi-classical QFT control gives $2n + 2$.

### Controlled modular multiplication

The doubly-controlled comparator (conditioned on both the QFT control qubit and the current bit $x_i$) turns the final CNOT gates in the CARRY circuit into 3-qubit-controlled-NOT gates, implementable with 4 Toffoli gates using one of the idle dirty qubits (Barenco et al. 1995).

### Overall gate counts

The Toffoli count for one controlled modular multiplication:

$$T_{\text{mult}}(n) = 32n^2 \log_2 n + O(n^2)$$

For the full Shor's algorithm ($2n$ modular multiplications):

$$T_{\text{Shor}}(n) = 64n^3 \log_2 n + O(n^3)$$

This was verified by simulation in LIQUi$|\rangle$ on inputs up to 8,192-bit numbers.

## Key results

**Constant adder (in-place, classical constant $c$):**

$$\text{Size: } O(n \log n), \quad \text{Depth: } O(n), \quad \text{Ancillae: 1 dirty (serial) to } n \text{ dirty (parallel)}$$

**Shor's algorithm:**

$$\text{Qubits: } 2n + 2, \quad \text{Toffoli count: } 64n^3 \log_2 n + O(n^3), \quad \text{Depth: } O(n^3)$$

**Asymptotic improvement over Takahashi et al. (2006) under QEC:**

$$\text{Gate count: } O(n^3 \log n) \text{ vs. } O(n^3 \log^2 n) \quad \text{(factor } \log n \text{ saved from rotation synthesis)}$$
$$\text{Depth: } O(n^3) \text{ vs. } O(n^3 \log n)$$

## Comparison with prior work

| Implementation | Qubits | Gate count | Depth | Adder type | Rotation synthesis? |
|---|---|---|---|---|---|
| Beauregard (2003) | $2n + 3$ | $O(n^3 \log n)$ | $O(n^3)$ | QFT (Draper) | Yes |
| Takahashi et al. (2006) | $2n + 2$ | $O(n^3 \log^2 n)$ | $O(n^3 \log n)$ | QFT (Draper) | Yes |
| [[A New Quantum Ripple-Carry Addition Circuit (Cuccaro-Draper-Kutin-Moulton 2004) — Paper Notes|Cuccaro]] ripple-carry | $3n + O(1)$ | $O(n^3)$ | $O(n^3)$ | Classical | No |
| **Häner-Roetteler-Svore (2017)** | $2n + 2$ | $O(n^3 \log n)$ | $O(n^3)$ | Classical (Toffoli) | No |

The constant-adder comparison (Table 1 from the paper):

| Adder | Size | Depth | Ancillae |
|---|---|---|---|
| [[A New Quantum Ripple-Carry Addition Circuit (Cuccaro-Draper-Kutin-Moulton 2004) — Paper Notes|Cuccaro (2004)]] | $\Theta(n)$ | $\Theta(n)$ | $n + 1$ clean |
| Takahashi (2009) | $\Theta(n)$ | $\Theta(n)$ | $n$ clean |
| [[Addition on a Quantum Computer (Draper 2000) — Paper Notes|Draper (2000)]] QFT | $\Theta(n^2)$ | $\Theta(n)$ | 0 |
| **This paper** | $\Theta(n \log n)$ | $\Theta(n)$ | 1 dirty |

**Assessment:** This paper occupies an interesting niche. It matches the qubit count of Takahashi et al. while replacing all rotation gates with Toffolis — a strict improvement in the fault-tolerant regime where rotation synthesis is expensive. The constant-adder is the genuine contribution: using only dirty ancillae for a classical-constant addition was new. The overall factoring algorithm is a well-executed assembly of known techniques (Beauregard's uncompute trick, semi-classical QFT, repeated-addition-and-shift) with this new adder slotted in. Not a conceptual revolution, but a practically useful improvement for fault-tolerant implementations.

## Limits / caveats

1. **Size is $O(n \log n)$, not $O(n)$.** The constant adder is a $\log n$ factor larger than Cuccaro's or Takahashi's $O(n)$-size adders. This $\log n$ factor propagates into the full Shor circuit. The tradeoff: you avoid clean ancillae but pay in gate count.

2. **Open problem remains open.** The paper notes that a linear-size constant adder using only dirty ancillae would reduce the full circuit to $O(n^3)$ — matching the best gate count — without increasing qubit count to $3n + 2$. As of 2017, this was open. [[Halving the Cost of Quantum Addition (Gidney 2018) — Paper Notes|Gidney (2018)]] later addressed T-count from a different angle (measurement-based uncomputation) but did not solve this exact problem.

3. **T-gate count is high compared to later work.** Each Toffoli costs 7 T gates (or 4 in matched pairs). The $64n^3 \log_2 n$ Toffoli count translates to hundreds of billions of T gates for 2048-bit RSA. Later work by Gidney and Ekerå (2021) achieved factoring with roughly $3 \times 10^8$ Toffoli gates by using entirely different techniques (windowed arithmetic, coset representation).

4. **No measurement-based tricks.** The circuit is purely unitary. [[Halving the Cost of Quantum Addition (Gidney 2018) — Paper Notes|Gidney's temporary logical-AND]] would halve the T-count of paired Toffoli gates, but this paper predates that technique.

5. **Testability claim is for Toffoli networks only.** The binary-search fault localisation works because intermediate states of Toffoli networks are computational basis states. It does not extend to arbitrary quantum circuits, and the semi-classical QFT at the end of Shor's algorithm still involves rotations.

## Reusable ideas

1. [[Dirty Ancilla Constant Addition]] — Add a classically known constant to a quantum register using only dirty (borrowed) ancillae, via a divide-and-conquer carry computation. Achieves $O(n \log n)$ size, $O(n)$ depth, 1 dirty qubit minimum.

2. [[Carry Computation via Dirty Qubit Toggling]] — Propagate carry information through dirty qubits by encoding carries as toggles. The key insight: you cannot *set* a dirty qubit to a value, but you can *toggle* it and later un-toggle it, using pairs of Toffoli gates that cancel when both execute.

3. [[Binary Search Fault Localisation for Toffoli Networks]] — Test a Toffoli-only quantum circuit by running it on classical test vectors (since Toffoli networks map basis states to basis states). If an error is detected, subdivide the circuit and recursively test each half. This localises faults in $O(\log G)$ rounds for a $G$-gate circuit.

## References within this paper

- Takahashi and Kunihiro (2006) — the $2n + 2$ qubit Shor implementation using QFT addition that this paper matches in qubit count. QIC 6(2):184–192.
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor (1994)]] — the factoring algorithm itself. FOCS 1994.
- Beauregard (2003) — circuit for Shor's algorithm using $2n + 3$ qubits; introduced the swap-and-inverse uncomputation trick for modular multiplication. QIC 3(2):175–185.
- [[Addition on a Quantum Computer (Draper 2000) — Paper Notes|Draper (2000)]] — QFT-based adder. arXiv:quant-ph/0008033.
- [[A New Quantum Ripple-Carry Addition Circuit (Cuccaro-Draper-Kutin-Moulton 2004) — Paper Notes|Cuccaro, Draper, Kutin, Moulton (2004)]] — single-ancilla ripple-carry adder. arXiv:quant-ph/0410184.
- Takahashi, Tani, Kunihiro (2009) — addition circuits and unbounded fan-out. arXiv:0910.2530.
- Kliuchnikov, Maslov, Mosca (2016) — efficient Clifford+T synthesis. IEEE Trans. Computers 65(1):161–172.
- Selinger (2015) — Clifford+T approximation. QIC 15(1-2):159–180.
- Bocharov, Roetteler, Svore (2015a) — repeat-until-success circuit synthesis. PRL 114:080502.
- Bocharov, Roetteler, Svore (2015b) — probabilistic circuit synthesis with fallback. PRA 91:052317.
- Barenco et al. (1995) — elementary quantum gates; the multiply-controlled-NOT from one dirty ancilla that this paper's CARRY gate generalises. PRA 52(5):3457.
- Griffiths and Niu (1996) — semi-classical QFT. PRL 76(17):3228.
- Gidney (2015) — StackExchange post on constructing large controlled-NOTs from Toffolis without clean workspace. Used for the dirty-qubit incrementer.
- Wecker and Svore (2014) — LIQUi$|\rangle$ quantum simulator. arXiv:1402.4467.

## Cross-links

### Paper notes
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes]] — the algorithm being implemented
- [[Addition on a Quantum Computer (Draper 2000) — Paper Notes]] — the QFT adder that Beauregard and Takahashi used; this paper replaces it with a Toffoli-based alternative
- [[A New Quantum Ripple-Carry Addition Circuit (Cuccaro-Draper-Kutin-Moulton 2004) — Paper Notes]] — the classical-style adder that achieves $3n + O(1)$ qubits; this paper's adder targets fewer qubits at the cost of $O(n \log n)$ gates
- [[Halving the Cost of Quantum Addition (Gidney 2018) — Paper Notes]] — later T-count optimisation; the [[Temporary Logical-AND]] technique could be applied to this paper's Toffoli pairs
- [[A Classical-Quantum Adder with Constant Workspace and Linear Gates (Gidney 2025) — Paper Notes]] — uses this paper's carry-xor construction as a key building block; achieves $O(n)$ CQ addition, superseding the $O(n \log n)$ divide-and-conquer adder
- [[Improved Reversible and Quantum Circuits for Karatsuba-Based Integer Multiplication (Parent-Roetteler-Mosca 2017) — Paper Notes]] — another Roetteler paper on reversible arithmetic, using [[Pebble Game Space Reduction for Recursive Circuits|pebble games]] for space reduction in multiplication
- [[Optimizing Quantum Circuits for Arithmetic (Häner-Roetteler-Svore 2018) — Paper Notes]] — same authors' follow-up; reuses the CARRY circuit and constant adder for function evaluation (piecewise polynomials, Newton iteration)
- [[Quantum Computing Enhanced Computational Catalysis (von Burg, Low, Häner, Steiger, Reiher, Roetteler, Troyer 2020) — Paper Notes]] — later work by Häner and Roetteler on chemistry resource estimation
- [[Elucidating Reaction Mechanisms on Quantum Computers (Reiher, Wiebe, Svore, Wecker, Troyer 2017) — Paper Notes]] — Svore's FeMoCo benchmark paper from the same year

### Trick cards
- [[Dirty Ancilla Constant Addition]] — the main new technique from this paper
- [[Carry Computation via Dirty Qubit Toggling]] — the carry propagation mechanism
- [[Binary Search Fault Localisation for Toffoli Networks]] — the testability argument
- [[Reversible Modular Exponentiation with Garbage Cleanup]] — Beauregard's uncomputation trick used here
- [[MAJ-UMA Decomposition for Reversible Adders]] — the alternative carry propagation approach from Cuccaro
- [[Dirty Qubit Recycling for T-Gate Reduction]] — the general principle of borrowing idle qubits
- [[Temporary Logical-AND]] — later technique that could optimise this paper's Toffoli pairs
- [[Arithmetic in the Fourier Basis]] — the competing approach that this paper avoids
- [[Order-Finding via QFT and Continued Fractions]] — the outer loop of Shor's algorithm
