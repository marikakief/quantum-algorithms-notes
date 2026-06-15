# The Quantum Schur Transform I Efficient Qudit Circuits (Bacon-Chuang-Harrow 2006) — Paper Notes

> **Source:** Dave Bacon, Isaac L. Chuang, and Aram W. Harrow, *The Quantum Schur Transform: I. Efficient Qudit Circuits*, arXiv:quant-ph/0601001; SODA 2007
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/0601001) · [PDF](https://arxiv.org/pdf/quant-ph/0601001)
> **Tags:** #quantum-algorithm #Schur-transform #representation-theory #Clebsch-Gordan #symmetric-group #unitary-group #quantum-information

---

## The computational problem

Implement the **Schur transform** on $n$ qudits of local dimension $d$:

$$
(\mathbb C^d)^{\otimes n} \cong \bigoplus_{\lambda \in I_{d,n}} Q^d_\lambda \otimes P_\lambda,
$$

where $I_{d,n}$ is the set of partitions of $n$ into at most $d$ parts, $Q^d_\lambda$ is the polynomial irrep of $U(d)$, and $P_\lambda$ is the irrep of $S_n$ with the same Young diagram. The transform maps the computational basis

$$
|i_1,\ldots,i_n\rangle
$$

to a Schur basis register

$$
|\lambda\rangle |q\rangle |p\rangle,
$$

where $q \in Q^d_\lambda$ and $p \in P_\lambda$. The chosen basis is subgroup-adapted: $q$ is a Gelfand--Zetlin pattern for the tower $U(1) \subset \cdots \subset U(d)$, and $p$ is a Young--Yamanouchi path for $S_1 \subset \cdots \subset S_n$.

**Input:** $n$ qudit registers $|i_1,\ldots,i_n\rangle$, with each $i_k \in [d]$.

**Output:** registers $|\lambda\rangle |q\rangle |p\rangle$ in the Schur-Weyl decomposition.

**Cost measure:** elementary one- and two-qubit gates as a function of $n$, $d$, and precision $\varepsilon$.

---

## What the paper does

This is the original detailed qudit Schur-transform circuit. It gives a circuit of size

$$
n\,\operatorname{poly}\bigl(d,\log n,\log(1/\varepsilon)\bigr),
$$

with an optional extra $\operatorname{poly}(n)$ step to compress the $P_\lambda$ register to near-optimal size.

The construction is not the later high-dimensional trick from [[An Efficient High Dimensional Quantum Schur Transform (Krovi 2019) — Paper Notes|Krovi 2019]]. BCH still scales polynomially in $d$. Its real contribution is the circuit architecture: choose subgroup-adapted bases, build the Schur transform by cascading defining-irrep Clebsch--Gordan transforms, and implement each $U(d)$ Clebsch--Gordan step recursively using Wigner--Eckart and reduced Wigner coefficients.

**My assessment:** this is foundational infrastructure rather than a speedup paper. The $d$-dependence is not the final word, but the architecture became the baseline that later work improved.

---

## The algorithm / construction

### 1. Fix explicit Schur basis labels

The circuit needs basis labels that are both representation-theoretically natural and efficiently storable.

For $Q^d_\lambda$, use the Gelfand--Zetlin basis. A basis state is a chain

$$
q=(q_d=\lambda,q_{d-1},\ldots,q_1), \qquad q_1 \preceq q_2 \preceq \cdots \preceq q_d,
$$

where $q_{k-1}\preceq q_k$ is the interlacing condition for restricting $U(k)$ irreps to $U(k-1)$. Equivalently, $q$ is a semistandard Young tableau.

For $P_\lambda$, use the Young--Yamanouchi basis. A basis state is a chain

$$
p=(p_n=\lambda,p_{n-1},\ldots,p_1=(1)),
$$

where $p_{k-1}$ is obtained from $p_k$ by removing one box. Equivalently, this is a standard Young tableau.

These two choices line up with the recursive structure of the circuit.

### 2. Define the add-one-box Clebsch--Gordan transform

The only Clebsch--Gordan transform needed is tensoring a $U(d)$ irrep with the defining representation:

$$
Q^d_\mu \otimes Q^d_{(1)} \cong
\bigoplus_{j:\,\mu+e_j\in \mathbb Z^d_{++}} Q^d_{\mu+e_j}.
$$

The corresponding unitary $U_{\mathrm{CG}}$ acts on

$$
|\mu\rangle |q\rangle |i\rangle,
$$

where $|q\rangle\in Q^d_\mu$ and $|i\rangle\in Q^d_{(1)}$, and outputs a superposition over

$$
|j\rangle |\mu+e_j\rangle |q'\rangle,
$$

with $|q'\rangle\in Q^d_{\mu+e_j}$. The output label $j$ records which row received the new box, so the map remains unitary even when different input irreps feed into the same larger irrep label.

### 3. Cascade the Clebsch--Gordan transforms

Start with the first qudit as the irrep $Q^d_{(1)}$:

$$
|\lambda^{(1)}\rangle = |(1)\rangle, \qquad |q_1\rangle=|i_1\rangle.
$$

For $k=1,\ldots,n-1$, apply $U_{\mathrm{CG}}$ to

$$
|\lambda^{(k)}\rangle |q_k\rangle |i_{k+1}\rangle
$$

to obtain

$$
|j_k\rangle |\lambda^{(k+1)}\rangle |q_{k+1}\rangle,
\qquad \lambda^{(k+1)}=\lambda^{(k)}+e_{j_k}.
$$

After the final step, output

$$
|\lambda\rangle = |\lambda^{(n)}\rangle,
\qquad |q\rangle=|q_n\rangle,
\qquad |p\rangle=|j_1,\ldots,j_{n-1}\rangle.
$$

The sequence $j_1,\ldots,j_{n-1}$ is equivalent to the Young--Yamanouchi chain for $P_\lambda$: it records the row added at each stage, or in reverse the row removed under $S_k\downarrow S_{k-1}$.

Optionally, the $P_\lambda$ register can be compressed using the ranking map

$$
f_n(p_1,\ldots,p_n)=1+
\sum_{k=2}^{n}\sum_{\substack{\mu\in p_k-\square\\ \mu<p_{k-1}}}\dim P_\mu,
$$

which maps a Young--Yamanouchi path to an integer in $[\dim P_\lambda]$ in polynomial time.

### 4. Implement the $U(d)$ Clebsch--Gordan transform recursively

The hard part is implementing one $U(d)$ Clebsch--Gordan transform without expanding a matrix of dimension roughly $n^{O(d^2)}$.

Write a Gelfand--Zetlin basis vector as

$$
|q\rangle = |\mu'\rangle |q^{(d-2)}\rangle,
$$

where $\mu'\preceq \mu$ labels the $U(d-1)$ irrep inside $Q^d_\mu$.

The $U(d)$ CG transform is expressed through tensor operators

$$
T^{\mu,j}_i: Q^d_\mu \to Q^d_{\mu+e_j},
$$

where

$$
U^{[d]}_{\mathrm{CG}}|\mu\rangle|q\rangle|i\rangle
=|\mu\rangle\sum_j |\mu+e_j\rangle T^{\mu,j}_i|q\rangle.
$$

Restricting from $U(d)$ to $U(d-1)$ decomposes the action into maps between $Q^{d-1}_{\mu'}$ and $Q^{d-1}_{\mu'+e_{j'}}$. Wigner--Eckart then says that each such map factors into:

1. a lower-dimensional CG transform $U^{[d-1]}_{\mathrm{CG}}$, and
2. a reduced Wigner coefficient $\widehat T^{\mu,j,\mu',j'}$ independent of the lower-level basis vector.

Operationally:

1. If $i<d$, recursively apply $U^{[d-1]}_{\mathrm{CG}}$ to $|\mu'\rangle |q^{(d-2)}\rangle |i\rangle$, producing $|j'\rangle |\mu'+e_{j'}\rangle |q'^{(d-2)}\rangle$.
2. If $i=d$, skip the recursive CG call and relabel $i=d$ as $j'=0$.
3. Controlled on $\mu$ and $\mu'$ (or equivalently $\mu'+e_{j'}$), apply a $d\times d$ unitary $\widehat T^{[d]}_{\mu,\mu'}$ with matrix entries given by reduced Wigner coefficients.
4. Repack the resulting $U(d-1)$ data into a $U(d)$ Gelfand--Zetlin label $q'$.

The reduced Wigner coefficients are given explicitly via Biedenharn--Louck formulas. With

$$
\tilde \mu=\mu+\sum_{r=1}^d(d-r)e_r,
\qquad
\tilde \mu'=\mu'+\sum_{r=1}^{d-1}(d-1-r)e_r,
$$

the entries of $\widehat T^{[d]}_{\mu,\mu'}$ are products of differences of components of $\tilde\mu$ and $\tilde\mu'$, with a sign $S(j-j')$. The important algorithmic point is that all entries are computable in $\operatorname{poly}(d,\log n)$ arithmetic time and can be compiled into a $d$-dimensional unitary using $\operatorname{poly}(d,\log(1/\varepsilon))$ gates.

---

## Key results

**Schur transform circuit.** The Schur transform on $n$ qudits of dimension $d$ can be implemented to accuracy $\varepsilon$ in time

$$
n\,\operatorname{poly}\bigl(d,\log n,\log(1/\varepsilon)\bigr),
$$

with an optional additional $\operatorname{poly}(n)$ time to compress the $P_\lambda$ register to $\lceil \log \dim P_\lambda\rceil$ qubits.

**Clebsch--Gordan subroutine.** The add-one-box $U(d)$ Clebsch--Gordan transform can be implemented to accuracy $\varepsilon$ in time

$$
d^3\operatorname{poly}\bigl(\log n,\log(1/\varepsilon)\bigr)
$$

for Young diagrams with at most $n$ boxes.

**Naive constant-$d$ route.** If $d$ is fixed, the paper first observes that a direct matrix-element construction gives $\operatorname{poly}(n,\log(1/\varepsilon))$ gates, since

$$
\dim Q^d_\lambda \le (n+1)^{d^2}.
$$

The Wigner--Eckart recursion improves this to a form polynomial in $d$ and logarithmic in $n$ for a single CG transform.

---

## Comparison with prior work

| Work | What it gives | Scaling / limitation |
|---|---|---|
| Kaye--Mosca (2001) | efficient entanglement concentration circuit | special-purpose; not a full Schur transform |
| Bacon--Chuang--Harrow qubit version (2004) | Schur transform for $d=2$ | qubits only |
| **BCH qudit paper** | full qudit Schur transform via CG cascade + Wigner--Eckart recursion | $n\operatorname{poly}(d,\log n,\log(1/\varepsilon))$ |
| [[An Efficient High Dimensional Quantum Schur Transform (Krovi 2019) — Paper Notes|Krovi 2019]] | Schur transform via permutation modules and symmetric-group Fourier tools | $\operatorname{poly}(n,\log d,\log(1/\varepsilon))$; improves BCH in the $d$ dependence |

The later [[An Efficient High Dimensional Quantum Schur Transform (Krovi 2019) — Paper Notes|Krovi]] result is the better algorithm when $d$ is large. BCH is still the right note to read for the original CG-cascade viewpoint and the explicit Gelfand--Zetlin / Young--Yamanouchi bookkeeping.

---

## Limits / caveats

- The circuit is polynomial in $d$, not in $\log d$. For high-dimensional systems with $d\gg n$, [[An Efficient High Dimensional Quantum Schur Transform (Krovi 2019) — Paper Notes|Krovi 2019]] supersedes this scaling.
- The paper proves asymptotic circuit efficiency; it does not give optimized constants or a hardware-oriented implementation.
- The reduced Wigner step assumes that one can coherently compute the Biedenharn--Louck coefficients and synthesize the resulting controlled $d\times d$ unitaries. That is polynomial-time, but not lightweight.
- For applications needing only weak Schur sampling — measuring $\lambda$ — the full strong transform may be overkill. Phase-estimation routes, including ideas related to [[GPE for Multiplicity Space Decomposition]], can be cheaper in some regimes.
- The basis choices fix phase conventions only up to the conventions of the subgroup-adapted bases and the reduced Wigner coefficients. That is fine for many measurement applications, but coherent downstream use has to respect the same conventions.

---

## Reusable ideas

1. [[Clebsch-Gordan Cascade for Schur Transforms]] — build a Schur transform by adding one defining irrep at a time; the row-added history is the multiplicity register.
2. [[Wigner-Eckart Recursion for Unitary-Group Clebsch-Gordan Transforms]] — reduce a $U(d)$ add-one-box CG transform to a $U(d-1)$ CG transform plus a small reduced-Wigner unitary.
3. [[Subgroup-Adapted Basis Encoding for Schur-Weyl Registers]] — choose Gelfand--Zetlin and Young--Yamanouchi labels so representation branching becomes reversible register arithmetic.
4. [[Young-Path Ranking for Multiplicity Register Compression]] — compress a Young--Yamanouchi path into an integer by counting smaller branch paths using hook-length-style dimension formulas.

---

## References within this paper

- Bacon, Chuang, and Harrow (2004), *Efficient quantum circuits for quantum information theory* — the qubit Schur-transform predecessor.
- Kaye and Mosca (2001), *Quantum networks for concentrating entanglement* — efficient entanglement-concentration circuit; one of the motivating examples.
- Keyl and Werner (2001), *Estimating the spectrum of a density operator* — spectrum estimation via Schur sampling.
- Hayashi and Matsumoto (2002), *Universal distortion-free entanglement concentration* and related universal compression papers — applications requiring Schur-basis access.
- Gelfand and Zetlin (1950) — basis for $U(d)$ irreps.
- James and Kerber (1981) — symmetric-group representation theory and Young--Yamanouchi basis.
- Biedenharn and Louck (1968) — reduced Wigner coefficient formulas used in the recursive CG implementation.
- Beals (1997), *Quantum computation of Fourier transforms over symmetric groups* — related nonabelian Fourier-transform infrastructure; still pending in this Zoo batch.
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|Kitaev (1995)]] — phase estimation; relevant to part II and later Schur-sampling variants.
- Kuperberg (2003), *A subexponential-time quantum algorithm for the dihedral hidden subgroup problem* — cited as motivation for asking whether CG-series structure can yield algorithmic speedups beyond quantum information tasks.

---

## Cross-links

### Paper notes

- [[An Efficient High Dimensional Quantum Schur Transform (Krovi 2019) — Paper Notes]] — later high-dimensional Schur transform improving the $d$ dependence.
- [[Fast Quantum Algorithms for Approximating Some Irreducible Representations of Groups (Jordan 2009) — Paper Notes]] — uses Schur-transform access for selected $U(d)$ irreps.
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes]] — phase-estimation primitive related to weak Schur sampling and GPE.

### Trick cards

- [[Clebsch-Gordan Cascade for Schur Transforms]]
- [[Wigner-Eckart Recursion for Unitary-Group Clebsch-Gordan Transforms]]
- [[Subgroup-Adapted Basis Encoding for Schur-Weyl Registers]]
- [[Young-Path Ranking for Multiplicity Register Compression]]
- [[GPE for Multiplicity Space Decomposition]] — later, different route for multiplicity-space decomposition.
- [[QFT over Permutation Modules]] — later Krovi route that replaces the BCH $U(d)$ CG cascade.
