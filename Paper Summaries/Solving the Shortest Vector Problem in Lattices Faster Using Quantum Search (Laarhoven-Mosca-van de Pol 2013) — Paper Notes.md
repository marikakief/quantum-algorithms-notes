# Solving the Shortest Vector Problem in Lattices Faster Using Quantum Search (Laarhoven-Mosca-van de Pol 2013) — Paper Notes

> **Source:** Thijs Laarhoven, Michele Mosca, and Joop van de Pol, *Solving the Shortest Vector Problem in Lattices Faster Using Quantum Search*, arXiv:1301.6176, PQCrypto 2013
> **Links:** [arXiv](https://arxiv.org/abs/1301.6176) · [PDF](https://arxiv.org/pdf/1301.6176)
> **Tags:** #lattice-problems #shortest-vector-problem #quantum-search #post-quantum-cryptography #quantum-cryptanalysis

---

## The computational problem

**Shortest Vector Problem (SVP).** Given a basis $B = \{b_1,\ldots,b_n\}$ for a full-rank lattice

$$
L = \left\{\sum_{i=1}^n \lambda_i b_i : \lambda_i \in \mathbb{Z}\right\} \subset \mathbb{R}^n,
$$

find a nonzero vector $s \in L$ such that $\|s\| = \lambda_1(L)$, where

$$
\lambda_1(L) = \min_{v \in L \setminus \{0\}} \|v\|.
$$

The approximate problem $\mathrm{SVP}_\gamma$ asks for a nonzero $v \in L$ with $\|v\| \leq \gamma \lambda_1(L)$. This paper studies exact SVP solvers as stand-alone attacks and as BKZ subroutines for lattice-based cryptanalysis.

The cost measure is the exponential leading constant in dimension $n$: time and space are written as $2^{cn+o(n)}$. The quantum model assumes Grover-style indexed access to exponentially large lists stored in quantumly addressable classical memory (QRACM). This is not the same as putting the whole list in coherent quantum memory, but it is also much stronger than ordinary classical RAM.

---

## What the paper does

This is a clean "Groverise the bottleneck" paper for lattice sieving and saturation algorithms. Wherever the classical SVP solver repeatedly searches a long list of candidate centres or pairs, replace that search by [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover search]].

The best provable result is a quantum saturation algorithm for exact SVP in time $2^{1.799n+o(n)}$ and space $2^{1.286n+o(n)}$, beating the classical provable $2^{2n+o(n)}$ Voronoi-cell algorithm in time. The best heuristic result is $2^{0.312n+o(n)}$ time and $2^{0.208n+o(n)}$ space by applying quantum search to the Nguyen--Vidick sieve; the authors also expect about $2^{0.39n}$ time for quantum Gauss-sieve-style saturation in practice.

My assessment: the paper is conceptually simple, but it matters because the constants feed directly into post-quantum security estimates. The main caveat is the QRACM assumption; without fast coherent indexed access to huge classical lists, the advertised square-root list-search gain is not a hardware-neutral claim. The table values are time/space exponents, not complete area-time or architecture-level attack costs.

---

## The algorithm / construction

### Shared quantum-search replacement

The paper writes the common subroutine as

$$
x \leftarrow \operatorname{search}_{e \in L}(f(e)=1),
$$

meaning: find an element satisfying a predicate, or report that none exists with high probability. Classically this costs $O(|L|)$ predicate evaluations. Quantumly it costs $O(\sqrt{|L|})$ via [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover search]], assuming indexed access to the list.

The replacement is deliberately local: the outer lattice algorithm is unchanged. The heuristic assumptions used by the classical sieve therefore remain the same; only the list-search cost changes.

### 1. Nguyen--Vidick heuristic sieve with quantum centre search

The Nguyen--Vidick sieve starts with a long list $S$ of random lattice vectors of length at most $\|B\|$. Each sieving round takes the previous list $S_{\mathrm{prev}}$ and builds:

- a centre list $C$, initially $\{0\}$;
- a new shorter list $S$, initially empty.

For each $v \in S_{\mathrm{prev}}$:

1. Search $C$ for a centre $c$ satisfying $\|v-c\| \leq \gamma R$, where $R=\max_{u\in S_{\mathrm{prev}}}\|u\|$ and $2/3 < \gamma < 1$.
2. If such a centre exists, add $v-c$ to the new list $S$.
3. Otherwise, add $v$ to $C$.
4. Repeat until the current list is empty; return the shortest vector seen in the last nonempty list.

The paper uses the Nguyen--Vidick heuristic:

$$
S \cap C_n(\gamma R,R) \text{ is uniformly distributed in } C_n(\gamma R,R),
$$

where $C_n(r_1,r_2)=\{x\in\mathbb{R}^n:r_1\leq\|x\|\leq r_2\}$.

Classically, the dominant cost is searching $C$ for every $v$, giving roughly $\widetilde{O}(|S||C|)$. Quantumly this becomes

$$
\widetilde{O}(|S|\sqrt{|C|}).
$$

The final minimum search over $S_{\mathrm{prev}}$ is not a quantum bottleneck because it can be maintained incrementally or with sorted lists.

### 2. Wang--Liu--Tian--Bi two-centre sieve

Wang et al. use two centre lists $C_1,C_2$ and two geometric parameters $\gamma_1,\gamma_2$. The classical cost is

$$
\widetilde{O}(|S|(|C_1|+|C_2|)),
$$

while the quantum version costs

$$
\widetilde{O}\bigl(|S|(\sqrt{|C_1|}+\sqrt{|C_2|})\bigr).
$$

After re-optimising, the asymptotic quantum time and space match the simpler Nguyen--Vidick quantum sieve: $2^{0.312n+o(n)}$ time and $2^{0.208n+o(n)}$ space. The authors expect the Nguyen--Vidick version to have better constants.

### 3. Pujol--Stehlé provable saturation with quantum list and pair searches

The Pujol--Stehlé saturation algorithm is the paper's best provable result. It has three stages.

**Stage T: build a dummy reduction list.**

1. Set $\gamma=1-1/n$.
2. Sample $x$ uniformly from the Euclidean ball $B_n(0,\xi\mu)$.
3. Reduce $v'=x \bmod P(B)$ using the current list $T$: while there is $t\in T$ with $\|v'-t\|<\gamma\|v'\|$, set $v'\leftarrow v'-t$.
4. Let $v=v'-x$. If $\|v\|\geq R\mu$, add $v$ to $T$.

This list is mostly a proof device: it makes the later samples independent enough for the birthday-style analysis.

**Stage S: generate independent short vectors.**

Repeat a similar reduction process, but reduce against the fixed list $T$ and add every resulting $v$ to a fresh list $S$.

**Final stage: birthday search for a close pair.**

Search $S\times S$ for distinct $s_1,s_2$ with

$$
0 < \|s_1-s_2\| < \mu.
$$

Return $s_1-s_2$. Polynomially many calls handle the fact that a single run has constant success probability.

Quantum search is used in three places:

- searching $T$ while generating $T$;
- searching $T$ while generating $S$;
- searching $S\times S$ for the close pair.

The classical exponents in the three stages are replaced by their square-root-list counterparts. With the notation of Appendix B,

$$
\tilde t=\max\left(c_g+\frac{3c_t}{2},\; c_g+\frac{c_b}{2}+\frac{c_t}{2},\; c_g+\frac{c_b}{2}\right),
$$

and

$$
\tilde s=\max\left(c_t,\; c_g+\frac{c_b}{2}\right).
$$

Optimising gives $\xi\approx 0.9086$ and $R\approx 3.1376$.

### 4. Heuristic Micciancio--Voulgaris saturation / Gauss sieve

The practical saturation algorithm keeps a list $S$ of pairwise Gauss-reduced vectors. For each sampled vector $v$:

1. reduce $v$ by vectors already in $S$;
2. reduce existing vectors in $S$ by the new $v$ if possible;
3. add $v$ unless it vanishes;
4. stop after enough collisions.

Classically, the cost is of order $|S|^2$. Replacing the two list searches by quantum search suggests a cost of order

$$
|S|^{3/2},
$$

so the exponent drops by about $25\%$. Using the claimed practical classical scaling $2^{0.52n}$, the paper expects about $2^{0.39n}$ quantum time.

### Why enumeration and Voronoi do not get the same easy speedup

The appendix is useful. Enumeration is serial: the allowed values of $u_{i-1}$ depend on the chosen suffix $(u_i,\ldots,u_n)$, and modern pruning makes the region even less like a fixed search list. A brute-force Grover search over a predetermined box would lose the point of enumeration.

For the Micciancio--Voulgaris Voronoi-cell algorithm, some basic subroutines can be sped up, but the optimised $2^{2n+o(n)}$ algorithm uses a priority-queue enumeration trick. The search is not a plain independent list search, so the paper does not get a useful quantum improvement there.

---

## Key results

**Theorem 1 (Nguyen--Vidick sieve, quantum heuristic).** Assuming the Nguyen--Vidick uniformity heuristic, the quantum version of Algorithm 1 returns a shortest vector in time

$$
2^{0.312n+o(n)}
$$

and space

$$
2^{0.208n+o(n)}.
$$

More generally, if

$$
c_h=-\log_2(\gamma)-\frac12\log_2\left(1-\frac{\gamma^2}{4}\right),
$$

then the quantum time is $2^{(3c_h/2)n+o(n)}$ and the space is $2^{c_hn+o(n)}$; taking $\gamma\to1$ gives the displayed constants.

**Wang et al. quantum sieve.** Replacing the two centre searches by quantum search and re-optimising gives the same leading asymptotic cost as Nguyen--Vidick:

$$
T_Q = 2^{0.312n+o(n)},\qquad S_Q=2^{0.208n+o(n)}.
$$

**Theorem 2 (Pujol--Stehlé saturation, quantum provable).** Let $\xi\approx0.9086$ and $R\approx3.1376$. Using polynomially many calls to the quantum version of Algorithm 2, one can find a shortest vector with probability exponentially close to $1$ in time

$$
2^{1.799n+o(n)}
$$

and space

$$
2^{1.286n+o(n)}.
$$

**Parallel remark.** If the $S$ list is generated in parallel with exponentially many quantum processors, the time can be pushed to

$$
2^{1.470n+o(n)},
$$

with $\xi\approx1.0610$ and $R\approx4.5166$, but this requires exponentially many parallel quantum machines and is not the main resource estimate.

**Heuristic Gauss-sieve estimate.** Applying the same search replacement to the Micciancio--Voulgaris heuristic saturation algorithm changes the expected dominant cost from $|S|^2$ to $|S|^{3/2}$, leading to an estimated practical time near

$$
2^{0.39n}.
$$

---

## Comparison with prior work

| Algorithm family | Classical time | Classical space | Quantum time | Quantum space | Status |
|---|---:|---:|---:|---:|---|
| Enumeration | $2^{O(n\log n)}$ with Kannan preprocessing; practical variants $2^{O(n^2)}$ but fast at moderate $n$ | polynomial | no clean gain | — | serial/pruned search tree resists direct Groverisation |
| Pujol--Stehlé saturation | $2^{2.465n+o(n)}$ | $2^{1.233n+o(n)}$ | $2^{1.799n+o(n)}$ | $2^{1.286n+o(n)}$ | provable exact SVP |
| Micciancio--Voulgaris Voronoi | $2^{2n+o(n)}$ | $2^{n+o(n)}$ | no better bound here | — | best classical provable time remains hard to quantum-speed up directly |
| Micciancio--Voulgaris heuristic saturation | about $2^{0.52n}$ experimentally | likely $2^{0.208n}$ | about $2^{0.39n}$ expected | $2^{0.208n}$ | heuristic/practical |
| Nguyen--Vidick sieve | $2^{0.415n+o(n)}$ | $2^{0.208n+o(n)}$ | $2^{0.312n+o(n)}$ | $2^{0.208n+o(n)}$ | heuristic |
| Wang et al. sieve | $2^{0.384n+o(n)}$ | $2^{0.256n+o(n)}$ | $2^{0.312n+o(n)}$ | $2^{0.208n+o(n)}$ | heuristic; quantum constants likely worse than Nguyen--Vidick |
| Chailloux--Loyer (2021) | LSF sieve baseline $2^{0.2925n+o(n)}$ | $2^{0.2075n+o(n)}$ | $2^{0.2570n+o(n)}$ | QRAM $2^{0.0767n}$, quantum memory $2^{0.0495n}$ | later improves the LSF/Grover bottleneck using a local-filter Johnson walk; see [[Lattice Sieving via Quantum Random Walks (Chailloux-Loyer 2021) — Paper Notes]] |

Compared with [[Quantum Computation and Lattice Problems (Regev 2002) — Paper Notes|Regev (2002)]], this paper is not a hidden-subgroup reduction and does not threaten lattice cryptography with polynomial-time algorithms. It is a parameter-estimation paper: assume quantum search is available, then update the exponential constants for the best SVP attacks. The later [[Lattice Sieving via Quantum Random Walks (Chailloux-Loyer 2021) — Paper Notes|Chailloux--Loyer]] result shows that the LSF bucket-search step is not limited to plain Grover search.

---

## Limits / caveats

1. **QRACM is doing real work.** The list-search speedups require quantumly addressable classical memory for exponentially large lists. This is weaker than storing the list in coherent quantum memory, but still a strong architectural assumption.

2. **Heuristic results inherit classical heuristics.** The $2^{0.312n+o(n)}$ sieve bound assumes the Nguyen--Vidick uniformity heuristic. The paper does not prove that the heuristic list distributions hold.

3. **Space is still exponential.** The best heuristic quantum sieve keeps $2^{0.208n+o(n)}$ space; the best provable quantum saturation result uses $2^{1.286n+o(n)}$ space. Security estimates should account for memory layout, QRACM access, parallelism, and area-time tradeoffs rather than comparing only the time exponent.

4. **No direct speedup for the practical enumeration frontier.** The paper is honest about enumeration: the tree/pruning structure is not a flat list, so Grover search does not plug in cleanly.

5. **The provable quantum result improves time but not clearly practice.** The $2^{1.799n}$ result is asymptotic and exact, but practical lattice attacks at challenge sizes historically use enumeration or Gauss-sieve variants with better finite-dimensional behaviour.

---

## Reusable ideas

1. [[Groverised Centre Search in Lattice Sieves]] — replace each linear scan over sieve centres by Grover search, changing $\widetilde{O}(|S||C|)$ into $\widetilde{O}(|S|\sqrt{|C|})$ without changing the outer sieve.

2. [[Quantum Birthday Search in Saturation SVP]] — use Grover search both for reductions against a saturation list and for the final close-pair search over $S\times S$, preserving the birthday-paradox proof shape while lowering the list-search exponents.

3. [[QRACM Accounting for Quantum Search over Classical Lists]] — separate ordinary qubits, quantumly addressable classical memory, and query count when Groverising classical list algorithms; otherwise the resource estimate hides its main assumption.

---

## References within this paper

- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover (1996)]] and Boyer--Brassard--Høyer--Tapp (1998) — quantum search and tight bounds for unknown number of marked items.
- [[Quantum Computation and Lattice Problems (Regev 2002) — Paper Notes|Regev (2002)]] — earlier quantum/lattice connection through dihedral cosets and unique-SVP, conceptually adjacent but technically different.
- [[Constructing Elliptic Curve Isogenies in Quantum Subexponential Time (Childs-Jao-Soukharev 2014) — Paper Notes|Childs--Jao--Soukharev (2014)]] — cited as a quantum subexponential attack on ordinary elliptic-curve isogeny problems relevant to post-quantum parameter setting.
- Micciancio--Voulgaris (2010), *Faster Exponential Time Algorithms for the Shortest Vector Problem* — saturation/Gauss-sieve algorithms; no vault note yet.
- Micciancio--Voulgaris (2010), *A Deterministic Single Exponential Time Algorithm for Most Lattice Problems based on Voronoi Cell Computations* — $2^{2n+o(n)}$ Voronoi-cell SVP; no vault note yet.
- Nguyen--Vidick (2008) — practical heuristic sieve with $2^{0.415n+o(n)}$ time and $2^{0.208n+o(n)}$ space; no vault note yet.
- Wang--Liu--Tian--Bi (2011) — two-level heuristic sieve with $2^{0.384n+o(n)}$ classical time; no vault note yet.
- Pujol--Stehlé (2009) — provable saturation algorithm with $2^{2.465n+o(n)}$ time; no vault note yet.
- Schneider (2011) — Gauss-sieve analysis, including the $2^{0.57n-23.5}$ practical fit; no vault note yet.
- Ajtai--Kumar--Sivakumar (2001) — first $2^{O(n)}$ sieve for SVP; no vault note yet.
- Kannan (1983), Schnorr--Euchner (1994), Gama--Nguyen--Regev (2010) — enumeration and pruning lineage; no vault notes yet.

---

## Cross-links

### Paper notes

- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]]
- [[Quantum Computation and Lattice Problems (Regev 2002) — Paper Notes]]
- [[Constructing Elliptic Curve Isogenies in Quantum Subexponential Time (Childs-Jao-Soukharev 2014) — Paper Notes]]
- [[An Efficient Quantum Algorithm for Lattice Problems Achieving Subexponential Approximation Factor (Eldar-Hallgren 2022) — Paper Notes]] — another quantum-lattice algorithm, but based on phase-estimating approximate shift eigenvectors rather than Groverising list searches.

### Trick cards

- [[Groverised Centre Search in Lattice Sieves]]
- [[Quantum Birthday Search in Saturation SVP]]
- [[QRACM Accounting for Quantum Search over Classical Lists]]
- [[Standard Amplitude Amplification]]
