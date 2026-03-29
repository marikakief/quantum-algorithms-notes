> **Source:** Oded Regev, *Quantum Computation and Lattice Problems*, FOCS 2002; arXiv:cs/0304005
> **Links:** [arXiv](https://arxiv.org/abs/cs/0304005) · [FOCS 2002](https://doi.org/10.1109/SFCS.2002.1181991)
> **Tags:** #lattice-problems #hidden-subgroup-problem #quantum-reduction #cryptography #dihedral-group

---

## The computational problem

**Unique Shortest Vector Problem ($f(n)$-unique-SVP):** Given a lattice basis $\langle \bar{b}_1, \ldots, \bar{b}_n \rangle$ in $\mathbb{R}^n$ with the promise that the shortest nonzero vector is shorter than all non-parallel vectors by a factor of at least $f(n)$, find the shortest vector.

**Dihedral Coset Problem (DCP):** Given $\text{poly}(\log N)$ registers, each in the state $\frac{1}{\sqrt{2}}(|0, x\rangle + |1, (x+d) \bmod N\rangle)$ for arbitrary $x$ and fixed unknown $d$, find $d$. A failure parameter $f$ allows each register to be a "bad" state $|b, x\rangle$ with probability at most $(\log N)^{-f}$.

**Average-Case Subset Sum:** Given $r \approx \log N$ integers $a_1, \ldots, a_r$ and a target $t$ modulo $N$, find a subset summing to $t \pmod{N}$.

---

## What the paper does

This is the first explicit connection between quantum computation and lattice problems. Regev shows two reductions that chain together:

1. A quantum reduction from $\Theta(n^{1/2 + 2f})$-unique-SVP to the Dihedral Coset Problem (with failure parameter $f$).
2. A quantum reduction from the DCP to average-case subset sum.

Combining these gives a quantum worst-case-to-average-case reduction: if you can solve a $1/\text{poly}(\log N)$ fraction of random subset sum instances, you can solve $\Theta(n^{2.5})$-unique-SVP quantumly. This was the first result connecting quantum algorithms to the hardness of lattice problems, which later became the foundation of post-quantum cryptography.

---

## The algorithm / construction

### Part 1: Unique-SVP → Two Point Problem → DCP

**The Two Point Problem** is an intermediate step: given registers in states $\frac{1}{\sqrt{2}}(|0, \bar{a}\rangle + |1, \bar{a}'\rangle)$ where $\bar{a}' - \bar{a}$ is fixed, find this difference. This reduces to DCP by encoding each $n$-dimensional vector as a single integer via $f(a_1, \ldots, a_n) = a_1 + a_2 \cdot 2M + \cdots + a_n (2M)^{n-1}$.

**From unique-SVP to the Two Point Problem** (the main construction):

1. Start with an LLL-reduced basis $\langle \bar{b}_1, \ldots, \bar{b}_n \rangle$.
2. Pick a prime $p > n^{2+2f}$ and guess parameters $l$ (length estimate for $\|\bar{u}\|$), $m$ (residue of a coordinate mod $p$), and $i_0$ (which coordinate).
3. Create a uniform superposition over lattice points $f(t, \bar{a}) = (a_1 p + tm)\bar{b}_1 + \sum_{i=2}^n a_i \bar{b}_i$ for $t \in \{0,1\}$ and $\bar{a} \in \{0, \ldots, M-1\}^n$ where $M = 2^{4n}$.
4. The partition function $g$ maps lattice points to cells. In the improved version (§3.3), cells are $n$-dimensional balls of radius $R = c_{\text{bal}} n^{1/2+2f} \cdot l$ instead of cubes, giving the $n^{1/2+2f}$ uniqueness gap (vs. $n^{1+2f}$ for cubes).
5. Measure $g \circ f$ to collapse the superposition. The uniqueness promise ensures at most one point per cell for each value of $t$. If both $(0, \bar{a})$ and $(1, \bar{a}')$ land in the same cell, their difference encodes the shortest vector.
6. This creates registers for the Two Point Problem hiding $\bar{u}$.

**Key improvement using balls:** The cube-based partition gives $n^{1+2f}$ uniqueness gap. Switching to balls with the Grover-Rudolph state preparation for uniform superposition over grid points inside an $n$-ball tightens this to $n^{1/2+2f}$, because the $\ell_1/\ell_2$ ratio is $\sqrt{n}$ for cubes but the ball intersection analysis (Claim 3.7) gives $O(\sqrt{n}\|\bar{d}\|/R)$ relative volume loss.

### Part 2: DCP → Average-Case Subset Sum

1. Apply QFT to the second register of each coset state. Measuring value $a$ collapses the qubit to $\frac{1}{\sqrt{2}}(|0\rangle + e^{2\pi i ad/N}|1\rangle)$.
2. Take $r \approx \log N$ registers. The measured values $a_1, \ldots, a_r$ are uniform in $\{0, \ldots, N-1\}$ — a random subset sum instance.
3. For each $r$-bit string $\bar{\alpha}$, the phase is $e^{2\pi i d \sum_i \alpha_i a_i / N}$.
4. Compute $\lfloor t_{\bar{\alpha}}/2 \rfloor$ where $t_{\bar{\alpha}} = \sum_i \alpha_i a_i$. Measuring collapses to a superposition of two strings whose sums differ by 1.
5. Use the subset sum oracle to identify both strings. Apply a unitary mapping them to $|0\rangle$ and $|1\rangle$ (Claim 4.6). Now the phase difference $e^{2\pi i qd/N}$ can be measured on a single qubit.
6. Repeat with phase doublings ($q = 1, 2, 4, \ldots$) using [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|Kitaev's iterative phase estimation]] technique to get exponentially accurate estimate of $d$.
7. The matching structure (Definition 4.4, Lemma 4.5) handles the fact that subset sum oracles only work on a fraction of inputs — there exist matchings $f$ such that enough pairs $(t, f(t))$ fall in the oracle's success set.

---

## Key results

$$\textbf{Theorem 1.1: } \text{DCP with failure parameter } f \implies \text{quantum algorithm for } \Theta(n^{1/2+2f})\text{-unique-SVP}$$

$$\textbf{Theorem 1.3: } \text{Subset sum oracle solving } \frac{1}{\text{poly}(\log N)} \text{ fraction} \implies \text{DCP with } f=1$$

$$\textbf{Corollary 1.5: } \text{Average-case subset sum} \implies \text{quantum } \Theta(n^{2.5})\text{-unique-SVP}$$

$$\textbf{Corollary 1.2: } \text{Dihedral HSP by coset sampling} \implies \text{quantum poly}(n)\text{-unique-SVP}$$

---

## Comparison with prior work

| Result | Type | Unique-SVP gap | Notes |
|---|---|---|---|
| Ajtai (1996) | Classical reduction | Large polynomial | First worst-to-average-case |
| Cai-Nerurkar (1997) | Classical reduction | Improved polynomial | — |
| **Regev (2002, this paper)** | **Quantum reduction** | **$\Theta(n^{2.5})$** | **First quantum–lattice connection** |
| Regev (2003/2005, LWE) | Classical reduction | $\Theta(n^{1.5})$ | Subsumes this paper's gap |

The quantum reduction achieves a better gap ($n^{2.5}$) than the classical reductions of Ajtai/Cai-Nerurkar, but the later LWE paper by Regev himself (2005) gives $n^{1.5}$ classically and makes this specific reduction less practically relevant. The enduring contribution is the conceptual bridge between quantum computation and lattice problems.

---

## Limits / caveats

1. **Conditional on dihedral HSP or subset sum.** Neither the dihedral HSP nor average-case subset sum with density 1 has a known efficient algorithm. Kuperberg's algorithm for dihedral HSP runs in subexponential $2^{O(\sqrt{\log N})}$ time, so the full chain doesn't give a polynomial-time algorithm.

2. **Density-1 subset sum only.** The subset sum instances produced have $r \approx \log N$ elements — density approximately 1. Standard lattice-based subset sum algorithms (LLL) work well at low density, and known NP-hardness reductions produce high-density instances, but density-1 is an awkward middle ground. Impagliazzo-Naor cryptographic applications can't be used.

3. **Superseded by LWE.** Regev's own 2005 LWE paper gives a better classical worst-to-average-case reduction with gap $n^{1.5}$, without needing quantum computation for the reduction itself (though the LWE-based cryptosystems are designed to be hard even for quantum computers).

4. **Unique-SVP only.** The result requires the uniqueness promise — the shortest vector must be separated from all others by a polynomial factor. Standard SVP (without the promise) is not addressed.

---

## Reusable ideas

1. [[Lattice Point Spacing for Pairwise Isolation]] — Subsample lattice points so that along the shortest vector direction, only pairs (not clusters) remain within each cell. This makes the measurement collapse to exactly two-point superpositions.

2. [[Phase Recovery via Subset Sum Oracle]] — After QFT on coset states, the measured values become a random subset sum instance. The oracle provides the inverse mapping from sums to subsets, enabling identification of basis states for phase measurement.

3. [[Ball vs Cube Partition for Tighter Lattice Guarantees]] — Using $n$-dimensional balls instead of cubes for spatial partitioning improves the uniqueness gap from $n^{1+2f}$ to $n^{1/2+2f}$, exploiting the better $\ell_2$ geometry of balls. The Grover-Rudolph technique prepares the uniform superposition over grid points inside the ball.

---

## References within this paper

- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor (1994)]] — polynomial-time factoring; motivation for quantum speedups
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|Kitaev (1995)]] — iterative phase estimation technique used in the DCP → $d$ recovery
- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes|Simon (1994)]] — Simon's algorithm as an HSP precursor
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover (1996)]] — square-root speedup reference
- [[Creating Superpositions That Correspond to Efficiently Integrable Probability Distributions (Grover-Rudolph 2002) — Paper Notes|Grover-Rudolph (2002)]] — state preparation for uniform superpositions over balls
- [[From Optimal Measurement to Efficient Quantum Algorithms for the HSP over Semidirect Product Groups (Bacon-Childs-van Dam 2005) — Paper Notes|Bacon-Childs-van Dam (2005)]] — related HSP work on semidirect products
- Kuperberg (2003) — subexponential dihedral HSP algorithm ($2^{O(\sqrt{\log N})}$); no vault note
- Ajtai (1996) — first classical worst-to-average-case reduction for lattice problems; no vault note
- Ajtai-Dwork (1997) — lattice-based cryptosystem based on unique-SVP hardness; no vault note
- Ettinger-Høyer (2000) — polynomial coset sampling for dihedral HSP, but no efficient classical post-processing; no vault note
- LLL (Lenstra-Lenstra-Lovász 1982) — lattice basis reduction, used throughout; no vault note

---

## Cross-links

### Paper notes
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes]]
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes]]
- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes]]
- [[Creating Superpositions That Correspond to Efficiently Integrable Probability Distributions (Grover-Rudolph 2002) — Paper Notes]]
- [[From Optimal Measurement to Efficient Quantum Algorithms for the HSP over Semidirect Product Groups (Bacon-Childs-van Dam 2005) — Paper Notes]]
- [[Quantum Algorithms for Solvable Groups (Watrous 2001) — Paper Notes]]

### Trick cards
- [[Lattice Point Spacing for Pairwise Isolation]]
- [[Phase Recovery via Subset Sum Oracle]]
- [[Ball vs Cube Partition for Tighter Lattice Guarantees]]
- [[Coset Sampling via Fourier Transform]]
