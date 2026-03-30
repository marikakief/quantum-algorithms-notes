> **Source:** Greg Kuperberg, *A subexponential-time quantum algorithm for the dihedral hidden subgroup problem*, SIAM J. Comput. 35(1):170–188, 2005; arXiv:quant-ph/0302112
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/0302112) · [SIAM](https://doi.org/10.1137/S0097539703436345)
> **Tags:** #hidden-subgroup-problem #dihedral-group #quantum-sieve #subexponential #foundational

---

## The computational problem

**Dihedral hidden subgroup problem (DHSP):** Given an oracle $f: D_N \to S$ that hides a subgroup $H = \langle yx^s \rangle$ of the dihedral group $D_N$ (where $s$ is the "slope" of the hidden reflection), find $s$.

The dihedral group $D_N$ has $2N$ elements: $N$ rotations $x^0, \ldots, x^{N-1}$ and $N$ reflections $yx^0, \ldots, yx^{N-1}$. The DHSP is equivalent to:
- The **hidden shift problem** for abelian groups: given $f, g: A \to S$ with $f(a) = g(a + s)$, find $s$
- The **hidden reflection problem**: given $f: A \to S$ with $f(a) = f(s - a)$, find $s$

Classical query complexity is $\Omega(\sqrt{N})$. The DHSP has a direct connection to lattice problems — Regev showed that an efficient dihedral HSP algorithm implies a quantum algorithm for certain lattice problems (unique-SVP). This makes DHSP one of the most important open problems in quantum algorithms.

## What the paper does

Kuperberg gives the first subexponential-time quantum algorithm for DHSP, with time and query complexity $2^{O(\sqrt{\log N})}$. The algorithm works for all finitely generated abelian groups (not just cyclic/dihedral).

The standard approach to HSP — apply the character transform (noncommutative QFT) and measure — fails badly here. For the dihedral group, this yields a representation label $k$ (which is essentially random and useless) and a single qubit state $|\psi_k\rangle \propto |0\rangle + e^{2\pi i ks/N}|1\rangle$ that encodes $s$ as a phase. The problem: no single qubit gives enough information, and the labels $k$ are random.

Kuperberg's insight: don't try to extract information from individual qubits. Instead, use a **quantum sieve** that combines pairs of qubits to produce new qubits with "better" labels, eventually manufacturing the target state $|\psi_{N/2}\rangle$ whose Hadamard measurement directly reveals the parity of $s$.

## The algorithm

### Setup: coset states to labelled qubits

1. Prepare the mixed state $\rho_{D_N/H}$ by querying the oracle with the uniform superposition over $D_N$ and discarding the output
2. Apply the [[Quantum Fourier Transform Circuit|QFT]] on $\mathbb{Z}/N$ to the rotation register and measure, obtaining label $k \in \mathbb{Z}/N$ and qubit state $|\psi_k\rangle \propto |0\rangle + e^{2\pi i ks/N}|1\rangle$

Each oracle call produces one labelled qubit $(k, |\psi_k\rangle)$ with $k$ uniformly random.

### The sieve: combining qubits

Given $|\psi_k\rangle$ and $|\psi_\ell\rangle$:
1. Tensor them: $|\psi_k\rangle \otimes |\psi_\ell\rangle \propto |00\rangle + e^{2\pi i ks/N}|10\rangle + e^{2\pi i \ell s/N}|01\rangle + e^{2\pi i(k+\ell)s/N}|11\rangle$
2. Apply CNOT and measure the second qubit
3. The residual state is $|\psi_{k \pm \ell}\rangle$ — the sign depends on the measurement outcome (each with probability $1/2$)

This is **summand extraction**: two labelled qubits yield one qubit with label $k + \ell$ or $k - \ell$.

### Algorithm 1 (for $N = 2^n$)

Let $m = \lceil\sqrt{n-1}\rceil$.

1. Create a list $L_0$ of $O(8^m)$ labelled qubits from oracle calls
2. For $j = 0, \ldots, m-1$: pair qubits in $L_j$ whose labels $k, \ell$ agree in $m$ low bits (beyond trailing zeros). Extract $|\psi_{k \pm \ell}\rangle$; keep those where subtraction occurred (gaining $\sim m$ trailing zeros). This gives $L_{j+1}$ with $|L_{j+1}| \approx |L_j|/4$.
3. The final list $L_m$ contains copies of $|\psi_0\rangle$ and $|\psi_{2^{n-1}}\rangle$. Measure $|\psi_{2^{n-1}}\rangle$ in the $|\pm\rangle$ basis to get $s \bmod 2$.
4. Recurse: pass to the subgroup $F_{s \bmod 2} \cong D_{N/2}$ to find the next bit.

### Algorithm 3 (greedy sieve, for $N = r^n$)

Uses an objective function $\alpha(k)$ counting factors of $r$ in $k$. Greedily pairs qubits that minimise $\alpha$ and maximise $\alpha(k \pm \ell)$ of the result. Achieves $e^{O(\sqrt[3]{2\log^3 N})}$ for smooth $N$.

### The general case (Theorem 7.1)

For arbitrary finitely generated abelian groups $A$, the hidden shift problem is solved in $2^{O(\sqrt{n})}$ time where $n$ is the output length. The sieve operates on the multi-dimensional label space, guided by an objective function that tracks progress toward a target representation.

## Key results

**Theorem 1.1.** There is a quantum algorithm for DHSP with time and query complexity $2^{O(\sqrt{\log N})}$.

**Theorem 5.1.** For $N = r^n$ (smooth), the complexity is $e^{O(\sqrt[3]{2\log^3 N})}$.

**Theorem 7.1.** The hidden shift problem for arbitrary finitely generated abelian groups has complexity $2^{O(\sqrt{n})}$ where $n$ is the output length.

## Comparison with prior work

| Approach | Time | Space | Queries |
|---|---|---|---|
| Classical brute force | $O(N)$ | $O(\log N)$ | $O(\sqrt{N})$ |
| Ettinger-Høyer (1999) | $2^{O(n)}$ | $\text{poly}(n)$ | $O(n)$ |
| **Kuperberg (2005)** | $2^{O(\sqrt{n})}$ | $2^{O(\sqrt{n})}$ | $2^{O(\sqrt{n})}$ |
| [[A Subexponential Time Algorithm for the Dihedral Hidden Subgroup Problem with Polynomial Space (Regev 2004) — Paper Notes\|Regev (2004)]] | $2^{O(\sqrt{n \log n})}$ | $\text{poly}(n)$ | $2^{O(\sqrt{n \log n})}$ |

Where $n = \log N$.

Ettinger-Høyer showed that $O(\log N)$ queries suffice *information-theoretically* (the coset states contain enough information), but their extraction algorithm requires exponential computation. Kuperberg's sieve is the first to break the exponential barrier, though it remains subexponential rather than polynomial.

## Limits / caveats

- **Superpolynomial space:** The sieve requires storing $2^{O(\sqrt{\log N})}$ qubits simultaneously. This is the main limitation and motivated [[A Subexponential Time Algorithm for the Dihedral Hidden Subgroup Problem with Polynomial Space (Regev 2004) — Paper Notes|Regev's polynomial-space variant]].
- **Not polynomial time.** Still subexponential — so it doesn't fully solve the dihedral HSP or break lattice cryptography. The gap between $2^{O(\sqrt{n})}$ and $\text{poly}(n)$ remains a major open question.
- **Suboptimal for non-smooth $N$:** When $N$ isn't a prime power, the algorithm needs additional techniques (spliced approximation) and the complexity worsens slightly.
- The sieve is inherently probabilistic — it's analysed via Chernoff bounds on Bernoulli trials. The constant factors are good but not tight.
- Kuperberg's later paper (2013, arXiv:1307.2122) gives an improved sieve with $2^{O(\sqrt[3]{\log N})}$ complexity, but still subexponential.

## Reusable ideas

1. [[Quantum Sieve for Labelled Qubits]] — repeatedly pairing qubit states to drive their labels toward a target via summand extraction, analogous to classical sieve algorithms
2. [[Coset State to Labelled Qubit Reduction]] — reducing hidden subgroup coset states to individual labelled qubits via [[Coset Sampling via Fourier Transform|Fourier sampling]], extracting the representation-theoretic content

## References within this paper

- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes|Simon (1994)]] — Simon's algorithm; the case $G = (\mathbb{Z}/2)^n$
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor (1994)]] — Shor's algorithm; HSP for $G = \mathbb{Z}$
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|Kitaev (1995)]] — abelian HSP, character transform
- Ettinger-Høyer (1999) — showed polynomial query complexity for DHSP with exponential computation; first to isolate the labelled qubit structure
- Ettinger-Høyer-Knill (2004) — hidden subgroup states are almost orthogonal; information-theoretic analysis
- Blum-Kalai-Wasserman (2003) — classical sieve for learning parity with noise; inspiration for the quantum sieve
- Ajtai-Kumar-Sivakumar (2001) — classical sieve for shortest lattice vector
- [[A Subexponential Time Algorithm for the Dihedral Hidden Subgroup Problem with Polynomial Space (Regev 2004) — Paper Notes|Regev (2004)]] — polynomial-space modification of Kuperberg's algorithm
- Regev (2002/2004) — connection between dihedral HSP and lattice problems
- [[From Optimal Measurement to Efficient Quantum Algorithms for the HSP over Semidirect Product Groups (Bacon-Childs-van Dam 2005) — Paper Notes|Bacon-Childs-van Dam (2005)]] — PGM approach to nonabelian HSP; dihedral case as special case of semidirect products

## Cross-links

### Paper notes
- [[The Quantum Query Complexity of the Hidden Subgroup Problem Is Polynomial (Ettinger-Høyer-Knill 2004) — Paper Notes]] — proves the query complexity is polynomial for all groups; the computational complexity (which Kuperberg addresses) is the real barrier
- [[A Subexponential Time Algorithm for the Dihedral Hidden Subgroup Problem with Polynomial Space (Regev 2004) — Paper Notes]] — Regev's polynomial-space variant (same problem, different tradeoff)
- [[From Optimal Measurement to Efficient Quantum Algorithms for the HSP over Semidirect Product Groups (Bacon-Childs-van Dam 2005) — Paper Notes]] — PGM approach; dihedral case reduces to average-case subset sum
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes]] — the abelian HSP that Kuperberg generalises beyond
- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes]] — Simon's problem as the easiest case of HSP
- [[Polynomial-Time Quantum Algorithms for Pell's Equation and the Principal Ideal Problem (Hallgren 2002) — Paper Notes]] — another extension of quantum period finding beyond standard abelian HSP; complementary approach via continuous groups

### Trick cards
- [[Quantum Sieve for Labelled Qubits]]
- [[Coset State to Labelled Qubit Reduction]]
- [[Coset Sampling via Fourier Transform]]
- [[Quantum Fourier Transform Circuit]]
- [[Entangled Multi-Copy Measurements for Nonabelian HSP]]
