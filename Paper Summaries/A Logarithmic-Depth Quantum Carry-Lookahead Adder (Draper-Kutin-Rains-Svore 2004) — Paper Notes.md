
> **Source:** Thomas G. Draper, Samuel A. Kutin, Eric M. Rains, Krysta M. Svore, *A logarithmic-depth quantum carry-lookahead adder*, arXiv:quant-ph/0406142, Quantum Information and Computation **6**(4–5), 351–369 (2006)
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/0406142) · [Journal](https://doi.org/10.26421/QIC6.4-5-4)
> **Tags:** #quantum-arithmetic #adder #circuit-primitive #carry-lookahead #Toffoli #reversible-computation #depth-optimised

---

## The computational problem

Add two $n$-bit integers on a quantum computer, either out of place ($|a\rangle|b\rangle|0\rangle \to |a\rangle|b\rangle|a+b\rangle$) or in place ($|a\rangle|b\rangle \to |a\rangle|a+b\rangle$). The primary cost measure is **circuit depth** — the number of parallel time steps — rather than total gate count. Prior quantum adders ([[Addition on a Quantum Computer (Draper 2000) — Paper Notes|Draper (2000)]], [[A New Quantum Ripple-Carry Addition Circuit (Cuccaro-Draper-Kutin-Moulton 2004) — Paper Notes|Cuccaro-Draper-Kutin-Moulton (2004)]], Vedral-Barenco-Ekert (1996)) all have $\Omega(n)$ depth, because they propagate carries sequentially.

## What the paper does

Adapts the classical carry-lookahead technique to quantum circuits, giving the first $O(\log n)$-depth quantum adder. The out-of-place version uses $O(n)$ ancillae and $O(n)$ Toffoli gates, with depth $\lfloor \log n \rfloor + \lfloor \log(n)/3 \rfloor + 7$. The in-place version doubles depth and gate count. The paper also gives variants for modular addition ($\bmod 2^n$ and $\bmod 2^n - 1$), comparison, subtraction, and incoming carry.

The challenge is that quantum circuits face constraints absent in classical carry-lookahead: reversibility, explicit erasure of scratch space, no free fan-out, and bounded qubit count. The paper addresses all four cleanly.

## The algorithm / construction

### Carry status and the $\circledast$ operator

The classical carry-lookahead idea encodes each bit position's carry behaviour as one of three states: **kill** ($k$), **generate** ($g$), or **propagate** ($p$).

For single-bit positions:
- $a_i = b_i = 0$: carry is killed, $C[i, i+1] = k$
- $a_i = b_i = 1$: carry is generated, $C[i, i+1] = g$
- $a_i \neq b_i$: carry is propagated, $C[i, i+1] = p$

Interval merging via the associative operator $\circledast$:

$$C[i, j] = C[i, k] \circledast C[k, j] \quad \text{for any } i < k < j$$

Encode the three-valued status in two bits: $p[i,j] = 1$ when propagate, $g[i,j] = 1$ when generate (never both 1). The recurrences are:

$$p[i, j] = p[i, k] \wedge p[k, j]$$

$$g[i, j] = g[k, j] \oplus (g[i, k] \wedge p[k, j])$$

The carry bits are $c_j = g[0, j]$.

### Reversible carry computation (Section 3)

The circuit has four phases, each with $\sim \lfloor \log n \rfloor$ rounds:

1. **P-rounds** (compute propagate tree): In round $t$, compute $p[2^t m, 2^t m + 2^t]$ for all valid $m$ using one Toffoli per value. Stores results in ancillary space. Total: $n - w(n) - \lfloor \log n \rfloor$ Toffolis.

2. **G-rounds** (compute generate tree): In round $t$, update $G[j] = g[i, j]$ using one Toffoli combining $g[i, k]$ and $p[k, j]$ from round $t-1$. Total: $n - w(n)$ Toffolis.

3. **C-rounds** (propagate carry from root): Working from the top of the tree down, compute $c_j = g[0, j]$ for the remaining positions using previously computed carries and propagate values. Total: $n - \lfloor \log n \rfloor - 1$ Toffolis.

4. **P$^{-1}$-rounds** (erase propagate scratch): Repeat the P-round Toffolis in reverse to reset ancillae to zero.

Phases overlap: P-round $t+1$ runs in parallel with G-round $t$; C-round $t-1$ runs in parallel with P$^{-1}$-round $t$. This gives total depth:

$$\lfloor \log n \rfloor + \left\lfloor \frac{\log n}{3} \right\rfloor + 3$$

with a total of $4n - 3w(n) - 3\lfloor \log n \rfloor - 1$ Toffoli gates, where $w(n)$ is the Hamming weight of $n$.

### Out-of-place addition (Section 4.1)

1. Compute $g[i, i+1] = a_i \wedge b_i$ into output register $Z$ (one Toffoli per bit)
2. Compute $p[i, i+1] = a_i \oplus b_i$ into $B$ (CNOT layer)
3. Run the carry computation of Section 3 → $Z[i] = c_i$
4. Compute $s_i = a_i \oplus b_i \oplus c_i$ via CNOT layer
5. Restore $B$ to its original value

**Resources ($n \geq 7$):**

$$\text{Toffoli count} = 5n - 3w(n) - 3\lfloor \log n \rfloor - 1$$

$$\text{CNOT count} = 3n - 1$$

$$\text{Ancillae} = n - w(n) - \lfloor \log n \rfloor$$

$$\text{Depth} = \lfloor \log n \rfloor + \left\lfloor \frac{\log n}{3} \right\rfloor + 7$$

### In-place addition (Section 4.2)

After computing the carry string into $n - 1$ ancilla qubits and writing $s = a \oplus b \oplus c$ over $B$, the carries must be erased. The paper uses the two's-complement identity: if $s = a + b$, then adding $a$ to $s' = \overline{s}$ produces carry string $d = c$. So: complement $s$, re-run the carry computation in reverse, and the ancillae reset to zero.

**Resources ($n \geq 7$):**

$$\text{Toffoli count} = 10n - 3w(n) - 3w(n-1) - 3\lfloor \log n \rfloor - 3\lfloor \log(n-1) \rfloor - 7$$

$$\text{Ancillae} = 2n - w(n) - \lfloor \log n \rfloor - 1$$

$$\text{Depth} = \lfloor \log n \rfloor + \lfloor \log(n-1) \rfloor + \left\lfloor \frac{\log n}{3} \right\rfloor + \left\lfloor \frac{\log(n-1)}{3} \right\rfloor + 14$$

### Extensions (Section 5)

**Modular addition ($\bmod 2^n$):** Skip the high carry bit. Run the Section 3 circuit on the low-order $n - 1$ bits. Saves a few gates and one depth layer.

**Modular addition ($\bmod 2^n - 1$):** One's-complement arithmetic, where the carry wraps around: $c_0 = g[0, n]$. After the G-rounds produce the overflow bit $g[0, n]$, additional C-rounds propagate it down. Requires $n - 2$ ancillae and $5n - 6$ Toffolis out-of-place.

**Comparison:** Compute only the high bit of $a - b$. Complement $a$, run the carry tree forward to the high bit, then reverse. Depth $2\lfloor \log(n-1) \rfloor + 9$, using $6n - w(n-1) - 2\lfloor \log(n-1) \rfloor - 7$ Toffolis. The comparison costs *more* than out-of-place addition because the unused output bits serve as free scratch in the adder — the comparator doesn't get that benefit and must explicitly clean up.

**Incoming carry:** Treat $y$ as bit 0 of an $(n+1)$-bit addition. Saves 1–2 Toffolis.

**Subtraction:** Complement $a$ before and after addition. Same cost plus two NOT layers.

## Key results

**Out-of-place $n$-bit addition ($n = 2^k$):**

$$\text{Depth} = 2k + 2, \quad \text{Toffoli count} = 5n - 3k - 4, \quad \text{Ancillae} = n - k - 1$$

**In-place $n$-bit addition ($n = 2^k$):**

$$\text{Depth} = 4k + 3, \quad \text{Toffoli count} = 10n - 9k - 7, \quad \text{Ancillae} = 2n - k - 2$$

**Comparison ($n = 2^k$):**

$$\text{Depth} = 2k + 5, \quad \text{Toffoli count} = 6n - 3k - 5, \quad \text{Ancillae} = 2n - k - 2$$

## Comparison with prior work

| Adder | Depth | Toffoli count | Ancillae | Notes |
|---|---|---|---|---|
| Vedral-Barenco-Ekert (1996) | $3n - 1$ | $4n - 2$ | $n$ | First quantum adder |
| [[A New Quantum Ripple-Carry Addition Circuit (Cuccaro-Draper-Kutin-Moulton 2004) — Paper Notes\|Cuccaro-Draper-Kutin-Moulton (2004)]] | $2n - 1$ | $2n - 1$ | 1 (in-place) | Best gate count, single ancilla |
| [[Addition on a Quantum Computer (Draper 2000) — Paper Notes\|Draper (2000)]] QFT adder | $O(\log n)$ approx | $O(n \log n)$ rotations | 0 | Depth $O(\log n)$ but $O(n)$ T gates per rotation |
| **QCLA out-of-place** | $O(\log n)$ | $5n + O(\log n)$ | $n + O(\log n)$ | This paper |
| **QCLA in-place** | $O(\log n)$ | $10n + O(\log n)$ | $2n + O(\log n)$ | This paper |
| [[Halving the Cost of Quantum Addition (Gidney 2018) — Paper Notes\|Gidney (2018)]] | $2n - 2$ | $4n - 4$ T gates | $n - 1$ | Lowest T-count (linear depth) |

The QCLA adder is the only carry-based quantum adder with $O(\log n)$ depth. The Draper QFT adder also achieves $O(\log n)$ depth (with approximate QFT), but its rotation gates are expensive on fault-tolerant hardware. On surface codes, each rotation costs $O(\log(1/\varepsilon))$ T gates, making the effective T-count $O(n \log n \cdot \log(1/\varepsilon))$ vs. the QCLA's $O(n)$ Toffolis (= $O(n)$ T gates with [[Temporary Logical-AND|temporary logical-ANDs]]).

**Assessment:** This paper fills a genuine gap — $O(\log n)$ depth addition using only Toffoli gates. The construction is straightforward once you see how to make the classical carry-lookahead tree reversible, but the overlapping-phase scheduling and the two's-complement trick for in-place erasure are non-obvious. The ancilla cost ($\sim 2n$ for in-place) is the main downside; Gidney (2018) notes that at large $n$, the [[Ancilla Opportunity Cost Analysis|opportunity cost]] of holding these ancillae can exceed the depth savings.

## Limits / caveats

1. **Ancilla count:** The in-place adder uses $\sim 2n$ ancillae, vs. 1 for the Cuccaro adder. On hardware where qubits are scarce (or where idle qubits have high opportunity cost in T factories), the depth improvement may not compensate. [[Halving the Cost of Quantum Addition (Gidney 2018) — Paper Notes|Gidney (2018)]] estimates the crossover at $n \sim 960$ for surface codes.

2. **T-count not optimised:** The paper counts Toffolis, not T gates. Each Toffoli costs 7 T gates (textbook decomposition) or 4 T gates (via [[Temporary Logical-AND]]). Applying temporary logical-ANDs to the QCLA tree gives T-count $\sim 12n$ (noted by [[Halving the Cost of Quantum Addition (Gidney 2018) — Paper Notes|Gidney (2018)]] via Brent-Kung). This is 3× worse in T-count than the $4n$ T-count ripple-carry adder, so the QCLA wins only when *depth* is the binding constraint.

3. **Fan-out overhead:** Unlike classical carry-lookahead, quantum circuits cannot freely copy bits. Every time a propagate or generate value is used by multiple gates, the circuit must explicitly schedule them sequentially or use ancilla-based fan-out. The paper handles this through careful phase overlapping, but it's a source of constant-factor overhead absent in classical circuits.

4. **No modular multiplication circuit:** The paper gives only adders, comparators, and mod-$2^n$ / mod-$(2^n - 1)$ variants. Building a full modular multiplier for [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor's algorithm]] from QCLA adders is left to future work.

## Reusable ideas

1. [[Carry-Lookahead Tree for Logarithmic-Depth Addition]] — Encode each bit position as kill/generate/propagate, merge intervals via the associative $\circledast$ operator in a binary tree, then propagate carries back down. The four-phase structure (P-rounds, G-rounds, C-rounds, P$^{-1}$-rounds) with overlapping scheduling is the reusable pattern.

2. [[Two's-Complement Carry Erasure for In-Place Adders]] — Erase carry ancillae by complementing the sum and re-running the carry computation in reverse. Exploits the identity that $a + \overline{s}$ produces the same carry string as $a + b$ (where $s = a + b$), because $\overline{s} + s = -1$ in two's complement.

3. [[Phase Overlapping in Tree Circuits]] — Run independent tree phases in parallel: P-round $t+1$ overlaps with G-round $t$ (they use disjoint data), and C-round $t-1$ overlaps with P$^{-1}$-round $t$. This compresses the effective depth from $\sim 4\log n$ to $\sim \frac{4}{3}\log n + O(1)$.

## References within this paper

- Vedral, Barenco, Ekert (1996) — first quantum adder; $n$ ancillae, $4n + O(1)$ Toffolis, depth $6n - 2$. [quant-ph/9511018](https://arxiv.org/abs/quant-ph/9511018)
- [[A New Quantum Ripple-Carry Addition Circuit (Cuccaro-Draper-Kutin-Moulton 2004) — Paper Notes|Cuccaro, Draper, Kutin, Moulton (2004)]] — improved ripple-carry adder; same Draper. Cited as [1]. [quant-ph/0410184](https://arxiv.org/abs/quant-ph/0410184)
- [[Addition on a Quantum Computer (Draper 2000) — Paper Notes|Draper (2000)]] — QFT-based adder. Cited as [2]. [quant-ph/0008033](https://arxiv.org/abs/quant-ph/0008033)
- Hwang (1979) — *Computer Arithmetic: Principles, Architecture, and Design*; classical carry-lookahead textbook reference
- Mano (1979) — *Digital Logic and Computer Design*; another textbook source for carry-lookahead circuits
- Ofman (1963) — "On the algorithmic complexity of discrete functions"; early parallel prefix computation. Cited as [5].
- Staff of the Computation Laboratory (1949) — *Description of a Relay Calculator*; Harvard Mark I documentation, origin of carry-lookahead idea. Cited as [6].
- Weinberger and Smith (1956) — "A one-microsecond adder using one-megacycle circuitry"; early carry-lookahead hardware. Cited as [8].

## Cross-links

### Paper notes
- [[Addition on a Quantum Computer (Draper 2000) — Paper Notes]] — Draper's QFT-based adder; same author, complementary approach (Fourier basis vs. carry tree)
- [[A New Quantum Ripple-Carry Addition Circuit (Cuccaro-Draper-Kutin-Moulton 2004) — Paper Notes]] — by two of the same authors (Draper, Kutin); the ripple-carry alternative with 1 ancilla and $O(n)$ depth
- [[Halving the Cost of Quantum Addition (Gidney 2018) — Paper Notes]] — optimises the T-count of carry-based adders; notes that a Brent-Kung parallel prefix adder via temporary logical-ANDs gives $12n$ T gates, and that QCLA wins on depth but loses on T-count at large $n$
- [[A Classical-Quantum Adder with Constant Workspace and Linear Gates (Gidney 2025) — Paper Notes]] — further carry-based adder with $O(1)$ ancillae; takes a different approach to the ancilla problem
- [[Factoring using 2n+2 qubits with Toffoli based modular multiplication (Häner-Roetteler-Svore 2017) — Paper Notes]] — builds modular multiplication from adders for Shor's algorithm; the QCLA adder could reduce the depth of the modular multiplication subroutine
- [[Improved Reversible and Quantum Circuits for Karatsuba-Based Integer Multiplication (Parent-Roetteler-Mosca 2017) — Paper Notes]] — uses adder circuits as multiplication subroutines; depth-optimised multiplication benefits from QCLA
- [[A Log-Depth In-Place Quantum Fourier Transform (Kahanamoku-Meyer-Blue-Bergamaschi-Gidney-Chuang 2025) — Paper Notes]] — achieves $O(\log n)$-depth QFT; the QCLA adder provides the classical-circuit analogy for depth reduction via tree parallelism
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes]] — primary application: modular exponentiation requires many additions
- [[Approximate Encoded Permutations and Piecewise Quantum Adders (Gidney 2019) — Paper Notes]] — shows that approximate piecewise adders beat QCLA in spacetime volume at practical sizes; also notes that QCLA on $O(\log n)$-length pieces gives $O(\log \log n)$ depth

### Trick cards
- [[Carry-Lookahead Tree for Logarithmic-Depth Addition]] — the core construction from this paper
- [[Two's-Complement Carry Erasure for In-Place Adders]] — the erasure trick for in-place variant
- [[Phase Overlapping in Tree Circuits]] — the scheduling optimisation
- [[MAJ-UMA Decomposition for Reversible Adders]] — the ripple-carry alternative; this paper replaces the linear MAJ chain with a tree
- [[In-Place Majority for Carry Propagation]] — the MAJ gate; QCLA replaces the MAJ chain with a tree of propagate/generate merges
- [[Temporary Logical-AND]] — applicable to all Toffoli pairs in the QCLA tree; reduces T-count by half
- [[Ancilla Opportunity Cost Analysis]] — relevant to the depth-vs-ancilla tradeoff: QCLA uses $\sim 2n$ ancillae for $O(\log n)$ depth
- [[Arithmetic in the Fourier Basis]] — the competing approach from [[Addition on a Quantum Computer (Draper 2000) — Paper Notes|Draper (2000)]]
