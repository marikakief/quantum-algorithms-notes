> **Source:** Zeph Landau, Yunchao Liu, *Learning quantum states prepared by shallow circuits in polynomial time*, arXiv:2410.23618, in *Proceedings of STOC 2025*
> **Links:** [arXiv](https://arxiv.org/abs/2410.23618) · [STOC DOI](https://doi.org/10.1145/3717823.3718311)
> **Tags:** #quantum-learning #state-tomography #shallow-circuits #locality #complexity-testing

---

## The computational problem

Given copies of an unknown $n$-qubit pure state

$$
|\psi\rangle = U|0^n\rangle
$$

where $U$ is an unknown shallow circuit on a $k$-dimensional lattice, output a classical description of a shallow circuit that prepares a state close to $|\psi\rangle$.

This is the state-learning version of the shallow-circuit programme, but it is not a trivial corollary of [[Learning shallow quantum circuits (Huang-Liu-Broughton-Kim-Anshu-Landau-McClean 2024) — Paper Notes|the 2024 circuit-learning paper]]. There the challenge was reconstructing a unitary from local Heisenberg data. Here the challenge is to reconstruct a **state preparation history** from local marginals alone.

## What the paper does

It gives a polynomial-time algorithm for learning shallow-circuit states on finite-dimensional lattices, and it extends to polylogarithmic circuit depth with quasipolynomial runtime.

The key new idea is a very clean reconstruction primitive: if you can find a local inversion that disentangles a region, then you may replace that region by $|0\rangle$ and undo the inversion without changing the global target state. Layer enough of these replacement operations in the right geometric pattern and the backward lightcone of the final output terminates on known reset qubits rather than the original unknown state.

My assessment: this is the stronger state-learning result. It is conceptually nicer than the band-slicing construction in the earlier shallow-circuit paper because it avoids the ugly consistency problem altogether. The covering-scheme / replacement-process combination feels reusable.

## The algorithm / construction

### 1. Build a covering scheme

The algorithm first constructs a layered geometric decomposition of the lattice: subsets $S_j^i$ grouped into $\ell = k+1$ layers with three properties:

1. each $d$-neighborhood $B(S_j^i,d)$ is small,
2. neighborhoods in the same layer are disjoint,
3. every sufficiently large local neighborhood of a qubit is contained in one of these sets.

This covering scheme is the geometric skeleton that lets local reconstruction become global.

### 2. Learn local reduced density matrices

From copies of $|\psi\rangle$, estimate the reduced density matrix of every local region $B(S_j^i,d)$. This uses standard local-tomography / shadow-style ingredients; the important point is that all relevant regions have constant size for constant depth and lattice dimension.

### 3. Find local inversions

For each region, search for a depth-$d$ local unitary $V_j^i$ that disentangles the core region $S_j^i$ from the rest of its neighborhood. In the ideal case,

$$
V_j^i |\psi\rangle = |0\rangle_{S_j^i} \otimes |\phi\rangle.
$$

The paper finds such inversions by brute-force search over an $\varepsilon$-net on the constant-size local region. That is not elegant computationally, but because the region size is bounded, it is still polynomial in $n$ for fixed depth and geometry.

### 4. Define the replacement process

Given a local inversion $V_A$ for region $A$, define the replacement channel

$$
\Phi_A(\rho)=V_A^\dagger \, R_A\!\left(V_A\rho V_A^\dagger\right) V_A,
$$

where $R_A$ resets the qubits in $A$ to $|0\rangle$.

If $V_A$ is an exact local inversion for the target state, then

$$
\Phi_A(|\psi\rangle\langle\psi|)=|\psi\rangle\langle\psi|.
$$

So this operation rewrites part of the preparation history without changing the target state.

### 5. Layer the replacements and read the backward lightcone

Apply all replacement processes layer by layer according to the covering scheme. The composed channel fixes $|\psi\rangle$, but now its causal structure contains many explicit reset operations to $|0\rangle$.

The final learned circuit is extracted from the **backward lightcone** of the physical output qubits through this layered replacement network. The covering properties guarantee that this backward lightcone eventually terminates on reset qubits rather than the initial unknown state.

That means one obtains an explicit shallow circuit $W$ on system plus ancillas that prepares a state whose system marginal approximates $|\psi\rangle$.

## Key results

### Main theorem

For constant depth $d$ and lattice dimension $k$, there is a polynomial-time algorithm that learns a shallow circuit preparing $|\psi\rangle$ up to trace distance $\varepsilon$ with high probability.

In the more explicit form quoted in the paper, the algorithm outputs a depth-$(2k+1)d$ circuit with ancillas and has sample complexity

$$
M = \frac{n^4 2^{O(c)}}{\varepsilon^4}\log(n/\delta)
$$

and runtime

$$
T = \frac{n^4 2^{O(c)}}{\varepsilon^4}\log(n/\delta) + \left(\frac{n k d c}{\varepsilon}\right)^{O(dc)},
$$

where $c$ is the size bound on the local regions coming from the covering scheme.

So the theorem is polynomial in $n$ for fixed $k,d$, and quasipolynomial when $d=\operatorname{polylog}(n)$.

### Complexity-testing application

The same machinery yields an efficient test distinguishing low-circuit-complexity states from states that remain far from every shallow-circuit state, again under a lattice-depth promise.

That is a nice bonus: the result is not just constructive tomography but also a complexity witness under promise-gap assumptions.

## Comparison with prior work

| Work | Object learned | Main technique | Main limitation |
|---|---|---|---|
| [[Efficient Quantum State Tomography (Cramer-Plenio-Flammia-Somma-Gross-Bartlett-Landon-Cardinal-Poulin-Liu 2010) — Paper Notes|Cramer et al. (2010)]] | 1D low-entanglement states | sequential disentangling / MPS structure | not general shallow lattice states |
| [[Learning shallow quantum circuits (Huang-Liu-Broughton-Kim-Anshu-Landau-McClean 2024) — Paper Notes|Huang et al. (2024)]] | shallow circuits and some 2D state learning | ancilla sewing + band slicing | geometric consistency problem remains awkward |
| This paper | shallow lattice-prepared states | replacement process + covering scheme | brute-force local inversion search; ancillas |

## Limits / caveats

- The hidden constants in the region-size parameter $c$ are rough. This is a structural theorem first and a practical tomography routine second.
- The local inversion step uses a brute-force $\varepsilon$-net search, which is fine asymptotically for constant-size regions but not exactly pretty.
- The output circuit uses ancillas. That is built into the reconstruction story.
- The theorem relies on the lattice / shallow-depth promise. Without it, local marginals are nowhere near enough to reconstruct the global state.

## Reusable ideas

1. [[Replacement Process for State Reconstruction]] — use a local inversion, local reset, and inverse local inversion to rewrite state-preparation history while fixing the target state.
2. [[Covering Scheme for Lightcone-Terminating Reconstruction]] — arrange local replacement regions in layers so every output qubit’s backward lightcone hits a reset region.

## References within this paper

- [[Learning shallow quantum circuits (Huang-Liu-Broughton-Kim-Anshu-Landau-McClean 2024) — Paper Notes|Huang et al. (2024)]] — immediate predecessor; solved circuit learning and a more specialized shallow-state problem by different means.
- [[Efficient Quantum State Tomography (Cramer-Plenio-Flammia-Somma-Gross-Bartlett-Landon-Cardinal-Poulin-Liu 2010) — Paper Notes|Cramer et al. (2010)]] — structured state tomography via local reductions and disentangling in 1D.
- [[Predicting Many Properties from Very Few Measurements (Huang-Kueng-Preskill 2020) — Paper Notes|Huang, Kueng, Preskill (2020)]] — shadow-style local marginal estimation as a subroutine.
- Local Hamiltonian / lattice locality literature — geometric backdrop for the covering argument and lightcone bounds.

## Cross-links

### Paper notes

- [[Learning shallow quantum circuits (Huang-Liu-Broughton-Kim-Anshu-Landau-McClean 2024) — Paper Notes]]
- [[Efficient Quantum State Tomography (Cramer-Plenio-Flammia-Somma-Gross-Bartlett-Landon-Cardinal-Poulin-Liu 2010) — Paper Notes]]
- [[Predicting Many Properties from Very Few Measurements (Huang-Kueng-Preskill 2020) — Paper Notes]]

### Trick cards

- [[Replacement Process for State Reconstruction]]
- [[Covering Scheme for Lightcone-Terminating Reconstruction]]
- [[Band-Slicing Reduction for 2D Shallow-State Learning]]
- [[Sequential Disentanglement for MPS Tomography]]
