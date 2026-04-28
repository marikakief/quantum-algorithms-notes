> **Source:** Steven A. Cuccaro, Thomas G. Draper, Samuel A. Kutin, David Petrie Moulton, *A new quantum ripple-carry addition circuit*, arXiv:quant-ph/0410184 (2004)
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/0410184)
> **Tags:** #quantum-arithmetic #adder #circuit-primitive #ripple-carry #Toffoli #reversible-computation

---

## The computational problem

Add two $n$-bit integers $a$ and $b$ on a quantum computer, storing the result in place: $|a\rangle|b\rangle \to |a\rangle|a+b\rangle$, with one additional output qubit for the carry bit $s_n$. The cost measures are qubit count (especially ancillae), gate count (Toffoli and CNOT), and circuit depth.

The prior state of the art was the Vedral-Barenco-Ekert (VBE) adder, which uses $n - O(1)$ ancilla qubits, $4n + O(1)$ Toffoli gates, $4n + O(1)$ CNOTs, and has depth $6n - 2$.

## What the paper does

Constructs a ripple-carry adder that uses only **one ancilla qubit** instead of $n$, while also reducing gate count and depth compared to VBE. The circuit uses $2n - 1$ Toffoli gates, $5n - 3$ CNOTs, and $2n - 4$ negations, with depth $2n + 4$. This is a factor-of-3 depth improvement over VBE and eliminates the linear ancilla overhead.

The key idea is two new gates — MAJ (in-place majority) and UMA (unmajority-and-add) — that propagate carries using only the input registers plus a single ancilla, rather than writing each carry into a separate scratch qubit.

## The algorithm / construction

### Carry string and basic structure

Define the carry string recursively: $c_0 = 0$ and $c_{i+1} = \text{MAJ}(a_i, b_i, c_i) = a_i b_i \oplus a_i c_i \oplus b_i c_i$. Then $s_i = a_i \oplus b_i \oplus c_i$ for $i < n$, and $s_n = c_n$.

A reversible ripple-carry adder computes carries $c_1, c_2, \ldots, c_n$ in order (upward sweep), then erases them while computing sum bits (downward sweep).

### The MAJ gate

Computes the majority of three bits in place using two CNOTs and one Toffoli:

```
c_i ──•──────•── c_i ⊕ a_i
b_i ──⊕──•──── b_i ⊕ a_i
a_i ─────⊕──T── c_{i+1}  (= MAJ(a_i, b_i, c_i))
```

The first CNOT maps $c_i \to c_i \oplus a_i$, the second maps $b_i \to b_i \oplus a_i$, and the Toffoli computes $a_i \oplus (c_i \oplus a_i)(b_i \oplus a_i) = \text{MAJ}(a_i, b_i, c_i)$, storing $c_{i+1}$ in register $A_i$.

### The UMA gate

Reverses MAJ while also computing the sum bit $s_i$. Two versions exist:

- **2-CNOT version:** one Toffoli and two CNOTs (simpler, less parallelisable)
- **3-CNOT version:** three CNOTs and one Toffoli (better depth when interleaved)

After applying MAJ then UMA on the same triple, register $A_i$ is restored to $a_i$, register $B_i$ holds $s_i$, and the carry $c_i$ is available for the next stage.

### Basic adder

String together $n$ MAJ gates (upward sweep to propagate $c_0 \to c_1 \to \cdots \to c_n$), copy $c_n$ to the output qubit $Z$, then apply $n$ UMA gates in reverse (downward sweep to compute $s_{n-1}, \ldots, s_0$ and erase all carries). One ancilla qubit $X$ (initialised to 0) holds $c_0$.

### Optimised adder (Section 3)

Five optimisations reduce depth from $\sim 4n$ to $2n + 4$:

1. **Parallel initial/final CNOTs:** The first CNOT of every MAJ gate and the last CNOT of every UMA gate can each execute in a single time-slice (they act on disjoint qubits).

2. **MAJ Toffoli/CNOT commutation:** The Toffoli at the end of MAJ$_i$ commutes with the second CNOT of MAJ$_{i+1}$. Swapping them lets the Toffoli of gate $i$ run in parallel with the CNOT of gate $i+2$.

3. **UMA Toffoli/CNOT commutation:** Symmetric to optimisation 2, applied to the UMA sweep.

4. **Simplified $c_1$ computation:** Since $c_0 = 0$, we have $c_1 = a_0 \cdot b_0$, which is a single Toffoli — no full MAJ gate needed. The ancilla stores $c_1$ instead of $c_0$.

5. **Direct output write:** Instead of writing $c_n$ to $A_{n-1}$, copying to $Z$, and erasing, the circuit writes directly to $Z$ with one Toffoli and two CNOTs, saving gates and depth.

### Final gate counts (for $n \geq 2$)

| Resource | Count |
|---|---|
| Toffoli gates | $2n - 1$ |
| CNOT gates | $5n - 3$ |
| NOT gates | $2n - 4$ |
| Ancilla qubits | 1 |
| Circuit depth | $2n + 4$ |

The depth breaks down as $2n - 1$ Toffoli time-slices and 5 CNOT time-slices.

## Key results

**Main adder ($n$-bit, carry output in $Z$):**

$$\text{Toffoli: } 2n - 1, \quad \text{CNOT: } 5n - 3, \quad \text{depth: } 2n + 4, \quad \text{ancilla: } 1$$

**Addition modulo $2^n$ (no carry output):**

$$\text{Toffoli: } 2n - 3, \quad \text{CNOT: } 5n - 7, \quad \text{depth: } 2n + 2, \quad \text{ancilla: } 1$$

**With incoming carry (external $c_0$):**

$$\text{Toffoli: } 2n - 1, \quad \text{CNOT: } 5n + 1, \quad \text{depth: } 2n + 6, \quad \text{ancilla: } 0$$

**High bit only (comparator):**

$$\text{Toffoli: } 2n - 1, \quad \text{CNOT: } 4n - 3, \quad \text{depth: } 2n + 3, \quad \text{ancilla: } 1$$

**Controlled-rotation decomposition:** The Toffoli gates can be overlapped when decomposed into controlled rotations, giving controlled-unary depth $6n - 2$ (not $10n + O(1)$ as a naive decomposition would suggest).

## Comparison with prior work

| Construction | Toffoli | CNOT | Ancilla | Depth | Notes |
|---|---|---|---|---|---|
| VBE (1996) | $4n - 2$ | $4n - 2$ | $n$ | $6n - 2$ | Classical ripple-carry |
| [[Addition on a Quantum Computer (Draper 2000) — Paper Notes\|Draper (2000)]] QFT adder | — | — | 0 | $O(n^2)$ / $O(n \log n)$ | Controlled rotations, not Toffolis |
| **Cuccaro et al. (2004)** | $2n - 1$ | $5n - 3$ | **1** | $2n + 4$ | This paper |
| Draper-Kutin-Rains-Svore (2004) | $O(n)$ | $O(n)$ | $O(n)$ | $O(\log n)$ | Carry-lookahead |
| [[Halving the Cost of Quantum Addition (Gidney 2018) — Paper Notes\|Gidney (2018)]] | — | — | $n - 1$ | $2n - 2$ | $4n - 4$ T gates; modifies Cuccaro |

**Assessment:** This paper is a clean, well-engineered improvement over VBE — not a conceptual revolution, but a practically important circuit that became the standard ripple-carry adder in subsequent work. The single-ancilla property is the main win: in qubit-constrained settings (e.g., early fault-tolerant machines or as a subroutine in [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor's algorithm]]), saving $n - 1$ ancillae matters. The MAJ/UMA decomposition is elegant and became the basis for [[Halving the Cost of Quantum Addition (Gidney 2018) — Paper Notes|Gidney's T-count optimisation]].

## Limits / caveats

1. **Linear depth is inherent to ripple-carry.** The $\Omega(n)$ depth cannot be improved without switching to a different adder topology (e.g., carry-lookahead). For applications where depth is the bottleneck, the Draper-Kutin-Rains-Svore $O(\log n)$-depth adder is preferable — at the cost of $O(n)$ ancillae.

2. **T-count is $8n + O(1)$ under standard Toffoli decomposition.** Each of the $2n - 1$ Toffolis costs 4 T gates when using the matched-phase-error trick (Barenco et al. 1995), since the Toffolis come in compute/uncompute pairs. [[Halving the Cost of Quantum Addition (Gidney 2018) — Paper Notes|Gidney (2018)]] later cut this to $4n + O(1)$ by replacing paired Toffolis with [[Temporary Logical-AND]] operations.

3. **CNOT count is higher than VBE.** The $5n - 3$ CNOTs exceed VBE's $4n - 2$. For architectures where CNOTs are not free (e.g., trapped ions with limited connectivity), this tradeoff may not be favourable.

4. **Classical-to-quantum addition gains nothing.** Unlike the [[Addition on a Quantum Computer (Draper 2000) — Paper Notes|QFT adder]], where adding a classical constant removes the need for a quantum $b$-register, the Cuccaro adder still requires $n$ qubits to store the classical addend. The QFT adder is superior for classical-to-quantum addition.

## Reusable ideas

1. [[In-Place Majority for Carry Propagation]] — Compute $\text{MAJ}(a, b, c)$ in place using 2 CNOTs + 1 Toffoli, storing the carry in one of the input registers. Avoids dedicating a separate ancilla qubit per carry bit.

2. [[MAJ-UMA Decomposition for Reversible Adders]] — Decompose a reversible adder into matched pairs of MAJ (compute carry) and UMA (uncompute carry + compute sum) gates. The pairing structure enables depth optimisations via gate commutation and is the foundation for [[Temporary Logical-AND]]-based T-count reductions.

3. [[Gate Commutation for Ripple-Carry Depth Reduction]] — In a chain of identical gate blocks (like MAJ or UMA), the Toffoli at the end of block $i$ may commute with the CNOT at the start of block $i+1$. Swapping them allows the Toffoli of block $i$ to execute in parallel with the CNOT of block $i+2$, halving the effective depth of the chain.

## References within this paper

- Dore, Kutin (in preparation at time of writing) — a logarithmic-depth comparison circuit with one ancilla, using a zero-ancilla version of the Cuccaro adder
- [[Addition on a Quantum Computer (Draper 2000) — Paper Notes|Draper (2000)]] — the QFT-based adder; same Draper as in this paper. [quant-ph/0008033](https://arxiv.org/abs/quant-ph/0008033)
- [[A Logarithmic-Depth Quantum Carry-Lookahead Adder (Draper-Kutin-Rains-Svore 2004) — Paper Notes|Draper, Kutin, Rains, Svore (2004)]] — logarithmic-depth carry-lookahead adder using $2n$ ancillae; depth/ancilla tradeoff family with $n/k$ ancillae and $O(k + \log n)$ depth. [quant-ph/0406142](https://arxiv.org/abs/quant-ph/0406142)
- Vedral, Barenco, Ekert (1996) — first quantum adder circuit (VBE); $n$ ancillae, $4n + O(1)$ Toffolis, depth $6n - 2$. [quant-ph/9511018](https://arxiv.org/abs/quant-ph/9511018)

## Cross-links

### Paper notes
- [[Addition on a Quantum Computer (Draper 2000) — Paper Notes]] — Draper's QFT-based adder; this paper is by the same Draper and provides the alternative ripple-carry approach
- [[A Logarithmic-Depth Quantum Carry-Lookahead Adder (Draper-Kutin-Rains-Svore 2004) — Paper Notes]] — by two of the same authors (Draper, Kutin); achieves $O(\log n)$ depth at the cost of $O(n)$ ancillae, vs. the single-ancilla $O(n)$-depth approach here
- [[Halving the Cost of Quantum Addition (Gidney 2018) — Paper Notes]] — directly modifies the Cuccaro adder, replacing paired Toffolis with [[Temporary Logical-AND]] operations to halve the T-count
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes]] — primary application: the adder is a subroutine in modular exponentiation for Shor's algorithm
- [[Improved Reversible and Quantum Circuits for Karatsuba-Based Integer Multiplication (Parent-Roetteler-Mosca 2017) — Paper Notes]] — uses this adder as the addition primitive in a Karatsuba multiplier circuit
- [[Factoring using 2n+2 qubits with Toffoli based modular multiplication (Häner-Roetteler-Svore 2017) — Paper Notes]] — achieves $2n + 2$ qubit Shor's algorithm using a Toffoli-based constant adder instead of this adder; compares constant-addition costs in Table 1
- [[Approximate Encoded Permutations and Piecewise Quantum Adders (Gidney 2019) — Paper Notes]] — uses the Cuccaro adder on each piece of a piecewise approximate addition with oblivious carry runways

### Trick cards
- [[In-Place Majority for Carry Propagation]] — the MAJ gate from this paper
- [[MAJ-UMA Decomposition for Reversible Adders]] — the paired gate structure
- [[Gate Commutation for Ripple-Carry Depth Reduction]] — the depth optimisation technique
- [[Temporary Logical-AND]] — Gidney's later optimisation of the paired Toffoli structure introduced here
- [[Ancilla Opportunity Cost Analysis]] — relevant to the 1-ancilla vs. $n$-ancilla tradeoff
- [[Carry-Lookahead Tree for Logarithmic-Depth Addition]] — the $O(\log n)$-depth alternative that replaces the MAJ chain with a tree
- [[Arithmetic in the Fourier Basis]] — the competing approach from Draper (2000)
- [[Fourier State as Eigenstate of Addition]] — underlies the QFT adder alternative
