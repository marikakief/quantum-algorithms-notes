# A Reduction of Semigroup DLP to Classic DLP (Banin-Tsaban 2013) — Paper Notes

> **Source:** Matan Banin and Boaz Tsaban, *A reduction of semigroup DLP to classic DLP*, arXiv:1310.7903 (2013)
> **Links:** [arXiv](https://arxiv.org/abs/1310.7903) · [PDF](https://arxiv.org/pdf/1310.7903)
> **Tags:** #quantum-algorithms #semigroups #discrete-log #cryptanalysis

---

## The computational problem

The paper studies the discrete logarithm problem in a periodic semigroup. A semigroup $S$ has an associative product but need not have an identity or inverses. The assumptions are deliberately modest: elements have unique encodings, multiplication is polynomial-time, and equality of canonical representatives is efficient.

**Input:** a periodic semigroup $S$, an element $g\in S$, and a power $h=g^x$.

**Output:** any natural number $k$ such that
$$
g^k=h.
$$

For $g$, let $\ell,n$ be minimal such that
$$
g^{\ell+n}=g^\ell.
$$
Thus $\langle g\rangle$ is a finite tail followed by a cycle. The reduction assumes access to an ordinary cyclic-group DLP oracle for the group sitting in that cycle. The reduction itself is classical, but the oracle can be supplied quantumly by Shor's algorithm; the statement should not be read as a classical algorithm for cyclic-group DLP. Its cost is polynomial in the relevant representation lengths, the size of $g$, and the order of the cyclic group, plus oracle calls.

## What the paper does

Banin and Tsaban give a classical polynomial-time reduction from semigroup DLP to ordinary cyclic-group DLP inside the same semigroup. This is the companion result to [[Quantum Computation of Discrete Logarithms in Semigroups (Childs-Ivanyos 2013) — Paper Notes|Childs--Ivanyos]]: instead of using quantum period finding directly, they show that the lack of inverses can be bypassed classically once the group DLP oracle is available. The result removes an extra semigroup obstruction; it does not make the underlying cyclic-group DLP easier.

The upshot for the Zoo entry is simple: semigroup DLP is not a post-quantum escape hatch. [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor's discrete-log algorithm]] supplies the group-DLP oracle, so periodic semigroup DLP is polynomial-time quantum-solvable under the paper's encoding and multiplication assumptions.

## The algorithm / construction

### 1. Extract the cyclic group from the eventual cycle

Let $\ell,n$ be minimal with $g^{\ell+n}=g^\ell$. Let $t$ be minimal with
$$
tn>\ell.
$$
Lemma 2.1 defines
$$
G:=g^\ell\cdot\{g^k:0\leq k<n\}=\{g^{\ell+k}:0\leq k<n\}.
$$
Then $G$ is a cyclic group of order $n$. Its identity is
$$
e=g^{tn},
$$
and a generator is
$$
r=g^{tn+1}.
$$
Moreover,
$$
g^{sn}=g^{tn}\qquad\text{for all }s\geq t.
$$
This is the core observation behind [[Classical Reduction of Semigroup DLP to Cycle-Group DLP]]. It is close in spirit to [[Cyclic Group Extraction from a Semigroup Cycle]], but the presentation is purely classical and oracle-reduction based.

### 2. Recover the cycle length $n$ using a DLP oracle

Choose a large $N\gg \ell+n$; in practice the paper suggests doubling $N$ until the procedure works. The point of the large interval is to put the sampled exponent far beyond the tail and to wash out boundary bias, so that the induced distribution on the eventual cycle is close to uniform. Pick a random
$$
k\in\{\lceil N/2\rceil,\ldots,N\}
$$
and compute
$$
h=g^k.
$$
For large enough $N$, this is statistically close to uniform over $G$. With probability $\varphi(n)/n$, $h$ generates $G$, and
$$
\frac{\varphi(n)}{n}>\frac{1}{e^\gamma\log\log n+3/\log\log n}
$$
for $n>2$; for $n=1,2$ it is at least $1/2$.

Given a generator $h$ of a cyclic subgroup of $G$, the group order is recovered with DLP-oracle calls: choose random $k\in\{1,\ldots,N\}$, compute
$$
k' := \log_h(h^k),
$$
and take gcds of $O(1)$ values of $k-k'$. The distribution of $k'$ depends only on $k\bmod n$, so these differences expose the subgroup order. Repeating for
$$
O(\log\log n)
$$
random elements and taking the maximum, or more safely the least common multiple, recovers $|G|=n$ with high probability. This is [[DLP-Oracle Order Recovery from Random Exponent Differences]].

### 3. Find the identity boundary $t$

Once $n$ is known, find the minimal $t$ such that
$$
g^{tn}=(g^{tn})^2.
$$
In $G$, the identity is the unique idempotent, and Lemma 2.1 says $g^{sn}$ stabilises at $g^{tn}$ for $s\geq t$. The predicate
$$
P(s):=[g^{sn}\text{ is idempotent}]
$$
is therefore false before $t$ and true from $t$ onward. The algorithm first doubles $b$ until $g^{bn}$ is idempotent, then binary-searches between $b/2$ and $b$. This is [[Idempotent Boundary Search in a Cyclic Semigroup]].

The paper notes that the powers $g^{2^i}$ can be precomputed, so each power test is implemented with logarithmically many semigroup multiplications, or with one multiplication once the relevant powers have been assembled.

### 4. Solve the semigroup DLP

Let $h=g^x$ be the target. Test membership in $G$ using Lemma 2.4:
$$
g^x\in G\quad\Longleftrightarrow\quad g^n g^x=g^x.
$$

**Case A: $h\in G$.** Let
$$
r=g^{tn+1}.
$$
Use the DLP oracle in $G$ to find $x'$ with
$$
r^{x'}=h.
$$
Then
$$
h=r^{x'}=g^{x'(tn+1)},
$$
so
$$
k=x'(tn+1)
$$
is a valid semigroup logarithm.

**Case B: $h\notin G$.** Binary-search for the minimal $b$ such that
$$
g^{bn}h\in G.
$$
Then use the oracle to compute
$$
x' := \log_r(g^{bn}h),
$$
so
$$
g^{bn+x}=g^{x'(tn+1)}.
$$
Now binary-search for the maximal $c$ such that
$$
g^{x'(tn+1)-cn}\in G.
$$
The paper's monotonicity argument gives
$$
bn+x=x'(tn+1)-cn,
$$
and hence
$$
x=x'(tn+1)-cn-bn.
$$

### 5. Nonperiodic semigroups

Section 3 is explicitly speculative. If $g$ has infinite order, the semigroup $\langle g\rangle$ is isomorphic to $(\mathbb N,+)$, but the exponent map may be hard to invert. The authors propose a length-correlation heuristic: choose random $r\in\{\lceil P/2\rceil,\ldots,P\}$, set
$$
\tilde g=g^r,\qquad \tilde h=h^r,
$$
and binary-search for the largest $k$ such that
$$
\operatorname{len}(\tilde g^k)\leq \operatorname{len}(\tilde h).
$$
If $g^k=h$, return $k$. They report success for tested braid-group examples with a deliberately crude length function, but this is not a theorem. The black-box infinite cyclic case remains open in the paper.

## Key results

**Lemma 2.1.** Let $S$ be periodic, $g\in S$, and let $\ell,n$ be minimal with $g^{\ell+n}=g^\ell$. Let $t$ be minimal with $tn>\ell$. Then
$$
G=\{g^{\ell+k}:0\leq k<n\}
$$
is a cyclic group of order $n$, with identity $g^{tn}$ and generator $g^{tn+1}$. Also,
$$
g^{tn}=g^{sn}\qquad(s\geq t).
$$

**Reduction 2.2.** The cycle length $n=|G|$ can be found using a DLP oracle for cyclic subgroups of $G$ by random sampling from the cycle, recovering subgroup orders from gcds of exponent differences, and repeating for $O(\log\log n)$ samples.

**Lemma 2.4.** For each $x\in\mathbb N$,
$$
g^x\in G\quad\Longleftrightarrow\quad g^n g^x=g^x.
$$

**Reduction 2.5.** Given the DLP oracle for $G$, a discrete logarithm of any input power $g^x$ is computable in polynomial time using the group-DLP call plus the two binary searches above.

**Consequence.** Periodic semigroup DLP reduces in polynomial time to ordinary DLP in a cyclic subgroup of the same semigroup. Therefore it has a polynomial-time quantum algorithm via [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor]], and it has subexponential classical algorithms whenever the corresponding cyclic-group DLPs do.

## Comparison with prior work

| Work | Method | Scope | Assessment |
|---|---|---|---|
| Myasnikov--Ushakov (2013) | Embeds the proposed matrix group-ring semigroup into matrices over a finite field, then uses Jordan canonical form and finite-field DLP reductions | The specific matrix-semigroup cryptosystem of Kahrobaei--Koupparis--Shpilrain | Useful cryptanalytic attack, but representation-specific. |
| Banin--Tsaban (2013) | Classical reduction from periodic semigroup DLP to DLP in the cycle group $G\leq S$ | Any periodic semigroup with unique encodings and efficient multiplication | Cleaner and more general for ordinary one-generator semigroup DLP. |
| [[Quantum Computation of Discrete Logarithms in Semigroups (Childs-Ivanyos 2013) — Paper Notes|Childs--Ivanyos (2013)]] | Quantum period finding to get the tail/cycle structure, then group discrete log in the cycle | Finite semigroups; also shifted DLP and constructive membership | Stronger quantum picture; shows where semigroup problems become hard again. |
| [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor (1994)]] | Abelian period finding / group DLP | Groups | Supplies the oracle that turns Banin--Tsaban into a quantum algorithm. |

## Limits / caveats

- The reduction is for **periodic** semigroups. The infinite-order discussion is a heuristic, not a proved algorithm.
- The theorem assumes canonical encodings and polynomial-time multiplication. Nonunique or expensive normal forms can move the real cost outside the algebraic reduction.
- The DLP oracle is for cyclic groups appearing inside the semigroup. The result does not make group DLP easier; it says semigroup DLP adds no extra hardness beyond those cycle groups.
- The random order-recovery step is probabilistic and depends on sampling enough elements whose generated subgroup orders reveal $n$. The paper states $O(\log\log n)$ repetitions; taking lcm rather than maximum is the safer implementation detail.
- This only handles **one-generator** semigroup DLP. It does not solve shifted semigroup DLP or constructive membership; [[Quantum Computation of Discrete Logarithms in Semigroups (Childs-Ivanyos 2013) — Paper Notes|Childs--Ivanyos]] show those variants have different complexity behaviour.

## Reusable ideas

1. [[Classical Reduction of Semigroup DLP to Cycle-Group DLP]] — identify the cyclic group in the eventual cycle and reduce all one-generator semigroup logs to ordinary DLP there.
2. [[Idempotent Boundary Search in a Cyclic Semigroup]] — find the entry point into the cycle by using idempotence of $g^{sn}$ as a monotone predicate.
3. [[DLP-Oracle Order Recovery from Random Exponent Differences]] — use a DLP oracle on random powers to recover cyclic subgroup orders via gcds of exponent disagreements.

## References within this paper

- [[Quantum Computation of Discrete Logarithms in Semigroups (Childs-Ivanyos 2013) — Paper Notes|Childs--Ivanyos (2013)]] — independent quantum algorithm for semigroup DLP; also treats shifted DLP and constructive membership.
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor (1994)]] — not cited directly in the reference list, but it is the quantum group-DLP engine behind the consequence claimed in the text.
- Bach--Shallit (1996) — source for the Euler totient lower bound used to argue random cycle elements generate with inverse-polylog probability.
- Kahrobaei--Koupparis--Shpilrain (2013) — semigroup Diffie--Hellman proposal over matrices in finite group rings.
- Myasnikov--Ushakov (2013) — specialised quantum attack on that matrix group-ring semigroup.
- Menezes--Wu (1997) — reduces DLP in $\mathrm{GL}(n,q)$ to finite-field DLP, giving subexponential attacks for matrix groups over finite fields.
- Koblitz (1987) and Miller (1986) — elliptic-curve DLP context.
- McCurley (1990) — early survey where semigroup DLP already appears.
- Odoni--Varadharajan--Sanders (1984) — older matrix-ring public-key distribution proposal.

## Cross-links

### Paper notes

- [[Quantum Computation of Discrete Logarithms in Semigroups (Childs-Ivanyos 2013) — Paper Notes]]
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes]]
- [[Shor's Discrete Logarithm Quantum Algorithm for Elliptic Curves (Proos-Zalka 2003) — Paper Notes]]

### Trick cards

- [[Classical Reduction of Semigroup DLP to Cycle-Group DLP]]
- [[Idempotent Boundary Search in a Cyclic Semigroup]]
- [[DLP-Oracle Order Recovery from Random Exponent Differences]]
- [[Cyclic Group Extraction from a Semigroup Cycle]]
