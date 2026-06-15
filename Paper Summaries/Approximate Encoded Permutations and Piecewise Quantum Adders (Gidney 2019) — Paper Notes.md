> **Source:** Craig Gidney, *Approximate encoded permutations and piecewise quantum adders*, arXiv:1905.08488 (2019)
> **Links:** [arXiv](https://arxiv.org/abs/1905.08488)
> **Tags:** #quantum-arithmetic #adder #approximate #fault-tolerant #surface-code #Shor #modular-arithmetic #circuit-primitive

---

## The computational problem

Perform a long sequence of $n$-bit additions on a fault-tolerant quantum computer (as arises in [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor's algorithm]], where $\sim n^2$ additions into one register are needed). The cost measure is **spacetime volume** — the product of qubit count and measurement depth — which determines runtime on the surface code. Exact adders have depth $\Omega(\log n)$ (carry-lookahead) or $\Omega(n)$ (ripple-carry); the question is whether approximate circuits can do better.

## What the paper does

Introduces a formal framework for **approximate encoded permutations** — reversible classical circuits that operate on an enlarged encoding space where a small fraction of encodings may produce wrong answers — and proves that deviation $\varepsilon$ in the encoding implies trace distance at most $2\sqrt{\varepsilon}$ in the quantum output. Uses this framework to formalise two constructions:

1. **Coset representation of modular integers** (due to Zalka 2006): encode $r \bmod N$ as a uniform superposition $\sum_j |r + jN\rangle$, so that modular addition reduces to cheaper non-modular addition. Deviation $\leq 2^{-m}$ with $m$ padding qubits.

2. **Oblivious carry runways**: insert $m$-qubit carry absorbers at evenly spaced positions in a register, initialised to $|+\rangle^{\otimes m}$ then subtracted from the neighbouring high bits. This splits one long addition into independent piecewise additions on short segments, each of length $s + m$, executable in parallel. Deviation $\leq 2^{-m}$ per runway.

Combining both gives modular additions in depth $O(s + m)$ with total deviation $\leq (r + 1) \cdot 2^{-m}$ for $r$ runways. The asymptotic **$O(\log \log n)$ depth** claim requires all of the following in the same construction: $s = O(\log n)$ piece size, $m = O(\log n)$ padding large enough for polynomially many additions and the chosen error budget, and carry-lookahead adders on each $O(\log n)$-length piece. The constant factors make this irrelevant in practice.

The practical result: for register sizes relevant to Shor's algorithm ($n \approx 2000$–$8000$), piecewise ripple-carry adders with runways spaced every 256–512 bits achieve lower spacetime volume than both exact ripple-carry ([[Halving the Cost of Quantum Addition (Gidney 2018) — Paper Notes|Gidney 2018]]) and exact carry-lookahead ([[A Logarithmic-Depth Quantum Carry-Lookahead Adder (Draper-Kutin-Rains-Svore 2004) — Paper Notes|Draper-Kutin-Rains-Svore 2004]]) adders under the paper's surface-code model, CCZ factory, and reaction-time assumptions. The advantage is particularly large for modular addition, where the coset representation avoids the multiple comparisons and conditional subtractions that exact modular adders require.

## The algorithm / construction

### Approximate encoded permutations

A tuple $(G, u, E, v, C, L, f)$ where:
- $G$ is the set of unencoded values, $u : G \to G$ the desired permutation
- $E$ is a larger encoded space, $v : E \to E$ a cheap permutation
- $C$ is a set of coset values, $f : G \times C \to E$ a reversible encoder
- $L$ is a leakage set for boundary effects

Each input $g$ has $|C|$ possible encodings. The **deviation** is the maximum fraction of coset values $c$ such that $v(f(g, c))$ is not a valid encoding of $u(g)$:

$$\text{Dev}(P) = \max_{g \in G} \frac{|\text{Deviated}_g(P)|}{|C|}$$

**Main theorem (Theorem 2.7):** If a quantum circuit prepares a uniform superposition over $C$, applies $f^{-1} \circ v \circ f$, then discards the coset register, the trace distance from the ideal output is at most $2\sqrt{\varepsilon}$ where $\varepsilon = \text{Dev}(P)$.

**Composition rules:**
- Deviation is subadditive under composition: $\text{Dev}(P_0 \circ P_1) \leq \text{Dev}(P_0) + \text{Dev}(P_1)$
- Deviation is subadditive under concatenation (nesting): $\text{Dev}(P_0 * P_1) \leq \text{Dev}(P_0) + \text{Dev}(P_1)$

These rules let you bound the total error of a long sequence of approximate operations via a union bound.

### Coset representation of modular integers

Encode $r \bmod N$ with $m$ padding bits as:

$$|\text{Coset}_m(r)\rangle = \frac{1}{\sqrt{2^m}} \sum_{j=0}^{2^m - 1} |r + jN\rangle$$

This state is approximately an eigenvector (eigenvalue 1) of the "add $N$" operation — adding $kN$ to the register barely changes the state. Therefore modular addition $(r + k) \bmod N$ can be approximated by ordinary (non-modular) addition of $k$ into the encoded register, without explicit comparison against $N$ or conditional subtraction.

**Encoding circuit:** Start with $|r\rangle$ on $n = \lceil \log_2 N \rceil$ qubits and $m$ ancilla qubits in $|+\rangle$. For each ancilla qubit $j$ (from 0 to $m-1$): add $2^j N$ to the register conditioned on the ancilla, then negate amplitudes where the register value $\geq 2^j N$, then Hadamard and measure the ancilla to collapse to the coset superposition. Total Toffoli count: $O(nm)$.

**Deviation bound (Theorem 3.2):** $\text{Dev}(\text{COM}_{k,N,m}) \leq 2^{-m}$. Only the single coset value $c = 2^m - 1$ can deviate (the one at the boundary where adding $k$ causes overflow past $2^m N$).

### Oblivious carry runways

**Problem:** In a classical register, inserting a carry runway (extra padding bits at position $p$) lets the low and high halves be added independently. But in a quantum register, the carry runway's state becomes entangled with the computation history — it leaks which-path information about the sequence of additions performed.

**Solution:** Initialise the runway qubits to $|+\rangle^{\otimes m}$, then subtract the runway value from the high part of the register. The resulting state is approximately an eigenvector of the "carry into runway / borrow from high half" operation.

The runway is part of the encoded representation. It is not a classical carry register that records whether a particular addition overflowed; treating it that way would leak which-path information across a sequence of additions.

Formally: for a register of $n$ bits with a runway of length $m$ at position $p$:

$$f((g, c)) = ((g \bmod 2^p) + 2^p c, \;\; (\lfloor g / 2^p \rfloor - c) \bmod 2^{n-p})$$

Addition of $k$ in the encoded space becomes **piecewise addition**: add $k \bmod 2^p$ to the low piece (length $p + m$) and $\lfloor k / 2^p \rfloor$ to the high piece (length $n - p$), independently and in parallel.

**Deviation bound (Theorem 4.2):** $\text{Dev}(\text{RUN}_{k,p,m,n}) \leq 2^{-m}$. Again, only $c = 2^m - 1$ can deviate (runway overflow).

### Combined construction

With $r$ runways spaced every $s$ bits and coset padding $m$, performing $k$ additions:

- **Measurement depth:** $\sim 2(s + m) \cdot k$ (each piece is a short ripple-carry addition running in parallel)
- **Toffoli count:** $2(n + mr) \cdot k$ (total across all pieces and all additions)
- **Trace distance from ideal:** $\leq 2\sqrt{k \cdot (r+1) \cdot 2^{-m}}$

Here $k$ is the number of additions in one encoded computation. The factor $k(r+1)$ is a union-bound style use of the deviation subadditivity rules across the additions, runways, and coset boundary.

### Asymptotic depth $O(\log \log n)$

Set runway spacing $s = O(\log n)$ and padding $m = O(\log n)$ (sufficient for polynomial-in-$n$ many additions at inverse-polynomial error). Each piece has length $O(\log n)$. Use a [[Carry-Lookahead Tree for Logarithmic-Depth Addition|carry-lookahead adder]] on each piece: depth $O(\log \log n)$. This is exponentially better than exact carry-lookahead, though the constant factors are too large to matter in practice.

## Key results

**Approximate encoded permutation error bound:**
$$\text{Dev}(P) \leq \varepsilon \implies \text{trace distance} \leq 2\sqrt{\varepsilon}$$

**Coset representation deviation:**
$$\text{Dev}(\text{COM}_{k,N,m}) \leq 2^{-m}$$

**Oblivious carry runway deviation:**
$$\text{Dev}(\text{RUN}_{k,p,m,n}) \leq 2^{-m}$$

**Combined ($r$ runways, $k$ additions):**
$$\text{trace distance} \leq 2\sqrt{k(r+1) \cdot 2^{-m}}$$

**Asymptotic depth of approximate addition:** $O(\log \log n)$ (impractical constant factors).

**Practical result:** For Shor's algorithm at $n \approx 2000$–$8000$ bits with $k = n^2$ additions at 1% trace distance, piecewise adders with 256- or 512-bit spacing achieve the lowest spacetime volume among all compared constructions, assuming the CCZ factory from Gidney (2019b) and 10 $\mu$s reaction time.

## Comparison with prior work

| Construction | Depth per addition | Toffoli count per addition | Ancilla | Modular overhead | Approximate? |
|---|---|---|---|---|---|
| [[A New Quantum Ripple-Carry Addition Circuit (Cuccaro-Draper-Kutin-Moulton 2004) — Paper Notes\|Cuccaro (2004)]] | $O(n)$ | $O(n)$ | 1 | $\sim 5\times$ (comparisons + conditional subtractions) | No |
| [[A Logarithmic-Depth Quantum Carry-Lookahead Adder (Draper-Kutin-Rains-Svore 2004) — Paper Notes\|DKRS (2004)]] | $O(\log n)$ | $O(n)$ | $O(n)$ | $\sim 5\times$ | No |
| [[Halving the Cost of Quantum Addition (Gidney 2018) — Paper Notes\|Gidney (2018)]] | $O(n)$ | $4n + O(1)$ T gates | $n - 1$ | $\sim 5\times$ | No |
| **Gidney (2019), piecewise** | $O(s + m)$ | $O(n + mr)$ | $O(mr)$ | $1\times$ (coset) | Yes ($2\sqrt{\varepsilon}$) |

The coset representation is the decisive advantage for modular arithmetic: exact modular adders spend $\sim 5$ non-modular additions per modular addition (add, compare, conditionally subtract, compare again, fix), while the coset approach performs one non-modular addition per modular addition.

**Assessment:** This paper's main intellectual contribution is the approximate encoded permutation framework — a clean formalisation of Zalka's "algorithmic error" idea with composable error bounds. The practical piecewise adder is a straightforward application of this framework, but its impact is significant: it demonstrates that for realistic Shor's algorithm parameters, the overhead of $O(\log n)$-depth exact adders (in ancillae and volume) is worse than the overhead of cheap approximate adders. The $O(\log \log n)$ depth result is a theoretical curiosity. The coset representation (Zalka's idea, formalised here) is the real workhorse for modular arithmetic in all subsequent Shor resource estimates.

## Limits / caveats

1. **Approximate, not exact.** The trace distance bound $2\sqrt{k(r+1)/2^m}$ grows with the number of additions $k$. For $k = n^2$ additions (Shor's algorithm), the padding $m$ must be $\Omega(\log n)$ to keep the error bounded. This is fine in practice but means the error budget must be tracked alongside other sources (gate synthesis, distillation).

2. **Assumes amortised cost model.** The spacetime volume comparison assumes many additions into the same register (so the encoding/decoding cost is amortised). For a single addition, the overhead of initialising carry runways and coset padding makes this worse than an exact adder.

3. **Coset encoding has $O(nm)$ Toffoli overhead.** The encoding circuit that creates the coset representation uses $O(nm)$ Toffoli gates (one comparison-conditioned addition per padding bit). This is a one-time cost, amortised over $k$ additions.

4. **$O(\log \log n)$ depth is impractical.** The constant factors from using carry-lookahead on $O(\log n)$-length pieces, combined with the ancilla overhead of carry-lookahead, make this asymptotic result irrelevant at any foreseeable problem size.

5. **Runway qubits are not free.** Each runway adds $m$ qubits. With $r = \lceil n/s - 1 \rceil$ runways, the total qubit overhead is $mr$. The paper's volume estimates account for this, but the raw qubit count is higher than for ripple-carry or carry-lookahead.

6. **Different from optimistic circuits.** Both this framework and [[Optimistic Quantum Circuits]] tolerate rare bad cases, but the accounting is different. Approximate encoded permutations bound deviation over random coset/padding encodings and compose it by subadditivity; optimistic circuits bound Frobenius-average circuit error over Hilbert space and need a separate distributional or reduction argument for a given algorithm.

## Reusable ideas

1. [[Approximate Encoded Permutation Framework]] — Formalise approximate quantum circuits as reversible permutations on an enlarged encoding space where a bounded fraction of encodings may err. Deviation is subadditive under composition and concatenation; deviation $\varepsilon$ implies trace distance $\leq 2\sqrt{\varepsilon}$.

2. [[Oblivious Carry Runway]] — Insert carry-absorbing padding qubits (initialised to $|+\rangle$, then subtracted from the neighbouring high bits) at evenly spaced positions in a register to split one long addition into independent piecewise additions on short segments. Deviation $\leq 2^{-m}$ per runway of length $m$.

3. [[Coset Representation for Approximate Modular Arithmetic]] — Encode $r \bmod N$ as a uniform superposition $\sum_j |r + jN\rangle$ with $m$ padding qubits. Modular addition reduces to non-modular addition in the encoded register, avoiding explicit comparison and conditional subtraction. Deviation $\leq 2^{-m}$.

## References within this paper

- [[A New Quantum Ripple-Carry Addition Circuit (Cuccaro-Draper-Kutin-Moulton 2004) — Paper Notes|Cuccaro, Draper, Kutin, Moulton (2004)]] — the ripple-carry adder used on each piece in the piecewise construction. [quant-ph/0410184](https://arxiv.org/abs/quant-ph/0410184)
- [[A Logarithmic-Depth Quantum Carry-Lookahead Adder (Draper-Kutin-Rains-Svore 2004) — Paper Notes|Draper, Kutin, Rains, Svore (2004)]] — the $O(\log n)$-depth adder that the approximate adder beats in spacetime volume at practical sizes. [quant-ph/0406142](https://arxiv.org/abs/quant-ph/0406142)
- [[Halving the Cost of Quantum Addition (Gidney 2018) — Paper Notes|Gidney (2018)]] — the T-count-optimised ripple-carry adder; compared against in the volume estimates. [arXiv:1709.06648](https://arxiv.org/abs/1709.06648)
- Gidney (2019b) — "Efficient magic state factories with a catalyzed $|CCZ\rangle$ to $2|T\rangle$ transformation"; provides the CCZ factory model used in spacetime estimates. [Quantum **3**, 135 (2019)](https://doi.org/10.22331/q-2019-04-30-135)
- Gidney (2019c) — "Asymptotically efficient quantum Karatsuba multiplication"; mentions that padding registers in Karatsuba can be replaced by oblivious carry runways to avoid uncomputation. [arXiv:1904.07356](https://arxiv.org/abs/1904.07356)
- Fowler (2012) — "Time-optimal quantum computation"; the time-optimal surface code execution model assumed in the volume estimates. [arXiv:1210.4626](https://arxiv.org/abs/1210.4626)
- Zalka (1998) — "Fast versions of Shor's quantum factoring algorithm"; originator of the "algorithmic error" idea that this paper formalises. [quant-ph/9806084](https://arxiv.org/abs/quant-ph/9806084)
- Zalka (2006) — "Shor's algorithm with fewer (pure) qubits"; defines the coset representation of modular integers. [quant-ph/0601097](https://arxiv.org/abs/quant-ph/0601097)

## Cross-links

### Paper notes
- [[Halving the Cost of Quantum Addition (Gidney 2018) — Paper Notes]] — Gidney's earlier adder; the ripple-carry adder used within each piece of the piecewise construction, and the main exact-adder comparison point
- [[A New Quantum Ripple-Carry Addition Circuit (Cuccaro-Draper-Kutin-Moulton 2004) — Paper Notes]] — the base ripple-carry adder used on each piece
- [[A Logarithmic-Depth Quantum Carry-Lookahead Adder (Draper-Kutin-Rains-Svore 2004) — Paper Notes]] — the exact $O(\log n)$-depth adder that approximate piecewise adders beat in volume at practical sizes; also usable on each piece for the $O(\log \log n)$ asymptotic depth
- [[Addition on a Quantum Computer (Draper 2000) — Paper Notes]] — the QFT-based adder; an alternative approach to avoiding carry overhead
- [[A Classical-Quantum Adder with Constant Workspace and Linear Gates (Gidney 2025) — Paper Notes]] — Gidney's later work on exact CQ addition; a different line from the approximate approach here
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes]] — primary application: the $n^2$ additions in modular exponentiation are what motivate piecewise approximate adders
- [[Factoring using 2n+2 qubits with Toffoli based modular multiplication (Häner-Roetteler-Svore 2017) — Paper Notes]] — another approach to low-qubit Shor arithmetic; uses exact modular multiplication without the coset representation
- [[Improved Reversible and Quantum Circuits for Karatsuba-Based Integer Multiplication (Parent-Roetteler-Mosca 2017) — Paper Notes]] — the Karatsuba multiplier whose padding registers can be replaced by oblivious carry runways (noted in this paper's conclusion)
- [[A Log-Depth In-Place Quantum Fourier Transform (Kahanamoku-Meyer-Blue-Bergamaschi-Gidney-Chuang 2025) — Paper Notes]] — another Gidney co-authored circuit that achieves depth reduction through approximate techniques (optimistic circuits)

### Trick cards
- [[Approximate Encoded Permutation Framework]] — the main conceptual contribution from this paper
- [[Oblivious Carry Runway]] — the carry-absorbing technique for piecewise addition
- [[Coset Representation for Approximate Modular Arithmetic]] — Zalka's idea, formalised here
- [[Carry-Lookahead Tree for Logarithmic-Depth Addition]] — the exact $O(\log n)$-depth adder that can be used on each piece
- [[Temporary Logical-AND]] — Gidney's earlier T-count primitive; the ripple-carry adder on each piece uses this
- [[Measurement-Asymmetric Uncomputation]] — the underlying measurement-based technique used in [[Halving the Cost of Quantum Addition (Gidney 2018) — Paper Notes|Gidney (2018)]]
- [[Ancilla Opportunity Cost Analysis]] — relevant to the spacetime volume comparison
- [[Optimistic Quantum Circuits]] — a later approximate-circuit technique from Gidney (with different error model)
