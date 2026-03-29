> **Source:** Oded Regev, *A subexponential time algorithm for the dihedral hidden subgroup problem with polynomial space*, arXiv:quant-ph/0406151, 2004
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/0406151)
> **Tags:** #hidden-subgroup-problem #dihedral-group #quantum-sieve #subexponential #space-efficient

---

## The computational problem

Same as [[A Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2005) — Paper Notes|Kuperberg (2005)]]: the **dihedral hidden subgroup problem (DHSP)**. Given an oracle $f: D_N \to R$ that hides a reflection subgroup $H = \{(0,0), (1,d)\}$ of $D_N$, find $d$.

Equivalently: given labelled qubits $|\psi_y\rangle = |0\rangle + e^{2\pi i dy/N}|1\rangle$ with $y$ uniformly random and known, find $d$ (or at least its least significant bit, from which the full $d$ can be recovered by recursion).

## What the paper does

Regev modifies [[A Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2005) — Paper Notes|Kuperberg's algorithm]] to achieve **polynomial space** at the cost of a slightly larger (but still subexponential) running time: $2^{O(\sqrt{\log N \cdot \log \log N})}$ vs Kuperberg's $2^{O(\sqrt{\log N})}$.

The key difference: Kuperberg's sieve stores $2^{O(\sqrt{n})}$ qubits simultaneously to wait for label collisions. Regev replaces this with a **multi-qubit combination** that processes $l + 4$ qubits at once and zeros out $l$ bits of the label in one shot, using only $O(l)$ qubits at a time.

## The algorithm

### Classical abstraction

Regev presents the algorithm through a clean classical abstraction. We have "objects" with labels in $\mathbb{Z}/N$ (where $N = 2^n$). The machine produces objects with uniformly random labels. The goal: produce an object with label $2^{n-1}$. Two objects with labels $a, b$ can be combined to yield one with label $a - b$ (succeeding with probability $1/2$).

### Kuperberg's pipeline (recap)

A chain of $k = O(\sqrt{n})$ routines, each clearing $k$ bits from labels. Each routine maintains a *pile* of $\sim 2^k$ objects waiting for collisions. This is where the space comes from: $k$ routines $\times$ $2^k$ objects each $= 2^{O(\sqrt{n})}$ total space.

### Regev's modification: multi-qubit combination

Instead of waiting for pairwise collisions, Regev uses a **batch combination** that processes $l + 4$ qubits at once to zero out $l$ bits.

Given $l + 4$ qubits $|\psi_{y_j}\rangle$ with uniformly random labels $y_1, \ldots, y_{l+4}$:

1. Tensor all qubits together:
$$\sum_{\vec{b} \in \{0,1\}^{l+4}} e^{2\pi i d \langle \vec{b}, \vec{y} \rangle / N} |\vec{b}\rangle$$
2. Compute $\langle \vec{b}, \vec{y} \rangle \bmod 2^l$ in an ancilla register (using the known $y_j$ values)
3. Measure the ancilla, obtaining $z \in \{0, \ldots, 2^l - 1\}$
4. Classically enumerate all $\vec{b} \in \{0,1\}^{l+4}$ with $\langle \vec{b}, \vec{y} \rangle \equiv z \pmod{2^l}$. Let $m$ be the count. If $m \notin \{2, 3, \ldots, 32\}$, fail.
5. Project onto the subspace spanned by the first two solutions $|\vec{b}_1\rangle, |\vec{b}_2\rangle$, obtaining:
$$e^{2\pi i d \langle \vec{b}_1, \vec{y} \rangle / N} |\vec{b}_1\rangle + e^{2\pi i d \langle \vec{b}_2, \vec{y} \rangle / N} |\vec{b}_2\rangle$$
6. Relabel to get the single-qubit state $|\psi_w\rangle$ with $w = \langle \vec{b}_2 - \vec{b}_1, \vec{y} \rangle$

Since $\langle \vec{b}_1, \vec{y} \rangle \equiv \langle \vec{b}_2, \vec{y} \rangle \pmod{2^l}$, the label $w$ has its $l$ least significant bits equal to zero.

**Why $m \in \{2, \ldots, 32\}$ with constant probability:** For each $\vec{b} \neq 0$, the indicator $\mathbf{1}[\langle \vec{b}, \vec{y} \rangle \equiv z]$ has expectation $2^{-l}$ and these are pairwise independent. The total count has expectation $\approx 16$ (from $2^{l+4} - 1$ non-zero strings) and variance $\approx 16$. Chebyshev's inequality gives constant probability of landing in $\{2, \ldots, 32\}$.

### Pipeline with no piles

The pipeline has $k = O(\sqrt{n/\log n})$ routines, each using the batch combination to clear $l = O(\sqrt{n \log n})$ bits. Each routine needs only $l + 4$ input objects per attempt — no pile storage. A routine simply waits for enough inputs from the previous stage, processes them, and either outputs one object (success) or discards and tries again.

Total input to the pipeline: $l^{O(k)} = 2^{O(\sqrt{n \log n})}$ oracle calls.

Each batch combination step takes $2^{O(l)} = 2^{O(\sqrt{n \log n})}$ classical time (brute-force enumeration of $\vec{b}$).

## Key results

**Theorem.** There is a quantum algorithm for the dihedral hidden subgroup problem with:
- Time complexity: $2^{O(\sqrt{\log N \cdot \log \log N})}$
- Space complexity: $\text{poly}(\log N)$
- Query complexity: $2^{O(\sqrt{\log N \cdot \log \log N})}$

## Comparison with Kuperberg

| | Kuperberg | Regev |
|---|---|---|
| Time | $2^{O(\sqrt{n})}$ | $2^{O(\sqrt{n \log n})}$ |
| Space | $2^{O(\sqrt{n})}$ | $\text{poly}(n)$ |
| Queries | $2^{O(\sqrt{n})}$ | $2^{O(\sqrt{n \log n})}$ |
| Combination | Pairwise ($k + \ell \to k \pm \ell$) | Batch ($l + 4$ qubits $\to$ 1 qubit, clearing $l$ bits) |
| Pile needed? | Yes ($2^{O(\sqrt{n})}$ qubits) | No (constant qubits per attempt) |

Where $n = \log N$.

The space reduction is significant for near-term considerations: polynomial-space algorithms are much more realistic than superpolynomial-space ones, even if both are subexponential in time.

## Limits / caveats

- The $\sqrt{n \log n}$ exponent is slightly worse than Kuperberg's $\sqrt{n}$. The $\log n$ factor comes from choosing $l = O(\sqrt{n \log n})$ to balance the pipeline parameters.
- The batch combination requires $2^{O(l)}$ classical brute-force enumeration — this is where most of the time goes. It's polynomial in the quantum resources but exponential (subexponentially so) in the classical work.
- Like Kuperberg's algorithm, this is still subexponential, not polynomial. The dihedral HSP in polynomial time remains open.
- The batch combination destroys $l + 3$ qubits per successful attempt. The attrition rate is similar to Kuperberg's ($\sim 4\times$ loss per pairwise step, vs constant-probability success per batch).
- The brute-force enumeration of $\vec{b}$ could potentially be improved (e.g., using lattice-sieving techniques on the constraint $\langle \vec{b}, \vec{y} \rangle \equiv z$), but this isn't explored in the paper.

## Reusable ideas

1. [[Batch Qubit Combination via Modular Inner Products]] — combining $l + 4$ labelled qubits to clear $l$ label bits in one shot, trading per-step classical computation for elimination of qubit storage

## References within this paper

- [[A Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2005) — Paper Notes|Kuperberg (2005)]] — the original subexponential DHSP algorithm that this paper improves in space
- Ettinger-Høyer (1999) — polynomial query complexity for DHSP
- Blum-Kalai-Wasserman (2003) — classical sieve inspiration
- Regev (2002/2004) — connection between dihedral HSP and lattice problems (same author's earlier work)

## Cross-links

### Paper notes
- [[A Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2005) — Paper Notes]] — the original algorithm that Regev modifies; same problem, same overall approach, different space/time tradeoff
- [[From Optimal Measurement to Efficient Quantum Algorithms for the HSP over Semidirect Product Groups (Bacon-Childs-van Dam 2005) — Paper Notes]] — complementary approach to nonabelian HSP via PGM
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes]] — the abelian HSP foundation
- [[Polynomial-Time Quantum Algorithms for Pell's Equation and the Principal Ideal Problem (Hallgren 2002) — Paper Notes]] — another extension of period finding; different group-theoretic setting but similar "beyond standard abelian HSP" flavour

### Trick cards
- [[Batch Qubit Combination via Modular Inner Products]]
- [[Quantum Sieve for Labelled Qubits]]
- [[Coset State to Labelled Qubit Reduction]]
- [[Coset Sampling via Fourier Transform]]
