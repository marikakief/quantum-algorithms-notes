# Fast Quantum Algorithms for Approximating Some Irreducible Representations of Groups (Jordan 2009) — Paper Notes

> **Source:** Stephen P. Jordan, *Fast quantum algorithms for approximating some irreducible representations of groups*, arXiv:0811.0562 (2009)
> **Links:** [arXiv](https://arxiv.org/abs/0811.0562) · [PDF](https://arxiv.org/pdf/0811.0562)
> **Tags:** #quantum-algorithms #representation-theory #group-theory #algebraic-algorithms #quantum-fourier-transform

---

## The computational problem

Jordan studies additive estimation of individual matrix elements of unitary irreducible representations. The input is a group element, an irrep label, basis labels for two states in the representation space, and an additive error parameter $\epsilon$.

For the symmetric group the paper makes this explicit.

**Problem 1: matrix element of an $S_n$ irrep.**

Input:
- a Young diagram $\lambda \vdash n$ specifying an irrep $\rho_\lambda$ of $S_n$;
- a permutation $\pi \in S_n$;
- two standard Young tableaux $\Lambda,\Gamma$ of shape $\lambda$, indexing the Young--Yamanouchi basis;
- an additive precision $\epsilon = 1/\operatorname{poly}(n)$.

Output:
$$
\langle \Lambda | \rho_\lambda(\pi) | \Gamma \rangle
$$
to additive error $\pm \epsilon$, with high probability.

The Lie-group version replaces $(S_n,\lambda,\pi,\Lambda,\Gamma)$ by:
- $G \in \{U(n),SU(n),SO(n)\}$;
- a highest-weight label whose entries have size $\operatorname{poly}(n)$;
- a group element $g \in G$, given as an explicit $n \times n$ matrix to polynomial precision;
- two Gel'fand--Tsetlin patterns indexing basis states.

The complexity measure is circuit size and sampling repetitions as a function of $n$ and $1/\epsilon$. All approximations are additive, not multiplicative.

## What the paper does

This is a representation-theoretic circuit construction paper, not an HSP paper. Jordan shows that several exponentially large irreducible representations can be implemented implicitly by polynomial-size quantum circuits, so a Hadamard test estimates their matrix elements in time $\operatorname{poly}(n,1/\epsilon)$.

The interesting part is the split verdict. Individual matrix elements for $S_n$, $A_n$, and polynomial-highest-weight $U(n),SU(n),SO(n)$ are quantumly accessible even when classical algorithms would have to manipulate exponentially large matrices. But normalized characters are not where the speedup sits: for $S_n$ and for these Lie groups, Jordan gives or points to polynomial-time classical character algorithms.

My assessment: this is a useful early warning against treating "representation matrix elements" and "characters" as interchangeable computational targets. The former can behave like BQP-type amplitude estimation; the latter often collapses to randomized sampling or Weyl determinant formulas.

## The algorithm / construction

### 1. Matrix elements from implemented representations

If a unitary circuit implements $U$ and one can prepare $|\psi\rangle$, the Hadamard test estimates
$$
\operatorname{Re}\langle \psi|U|\psi\rangle
$$
from the probability
$$
p_0 = \frac{1+\operatorname{Re}\langle \psi|U|\psi\rangle}{2}.
$$
Initializing the control qubit as $(|0\rangle-i|1\rangle)/\sqrt{2}$ gives the imaginary part. Repeating $O(1/\epsilon^2)$ times gives additive precision $\epsilon$ by sampling. For off-diagonal entries one can use the usual polarization reductions, or prepare the relevant basis superpositions.

So the real task is to implement the chosen irrep $\rho(g)$ efficiently.

### 2. Finite groups with efficient QFTs

For a finite group $G$, the quantum Fourier transform is
$$
U_{\mathrm{FT}}
= \sum_{g\in G}\sum_{\rho\in \widehat G}\sum_{i,j=1}^{d_\rho}
\sqrt{\frac{d_\rho}{|G|}}\,\rho_{i,j}(g)\,|\rho,i,j\rangle\langle g|.
$$
The left-regular action
$$
U_g = \sum_{h\in G}|gh\rangle\langle h|
$$
satisfies
$$
U_{\mathrm{FT}}U_gU_{\mathrm{FT}}^{-1}
= \bigoplus_{\rho\in\widehat G}\rho(g^{-1})\otimes I_{d_\rho},
$$
written in the paper's indices as
$$
\sum_{\rho\in\widehat G}\sum_{i,j,i',j'}
\delta_{j,j'}\rho_{i,i'}(g^{-1})|\rho,i,j\rangle\langle \rho,i',j'|.
$$
Thus an efficient QFT plus efficient multiplication by $g$ gives access to all irreps appearing in the Fourier basis. This covers $S_n$ using Beals's symmetric-group QFT, and connects to [[The Power of Basis Selection in Fourier Sampling (Moore-Rockmore-Russell-Schulman 2003) — Paper Notes|Moore--Rockmore--Russell--Schulman]] on nonabelian Fourier sampling.

### 3. Schur transform route for selected $U(d)$ irreps

On $(\mathbb C^d)^{\otimes n}$, the collective $U(d)$ action $u^{\otimes n}$ commutes with the permutation action $M_\pi$ of $S_n$. The Schur transform block diagonalizes the joint action:
$$
U_{\mathrm{Schur}}M_\pi u^{\otimes n}U_{\mathrm{Schur}}^{-1}
= \bigoplus_\lambda \rho_\lambda(\pi)\otimes \nu_\lambda(u),
$$
where $\lambda$ ranges over partitions of $n$ into at most $d$ parts. [[The Quantum Schur Transform I Efficient Qudit Circuits (Bacon-Chuang-Harrow 2006) — Paper Notes|Bacon--Chuang--Harrow]] give a $\operatorname{poly}(n,d)$ circuit via [[Clebsch-Gordan Cascade for Schur Transforms]], while [[An Efficient High Dimensional Quantum Schur Transform (Krovi 2019) — Paper Notes|Krovi's later high-dimensional Schur transform note]] improves the local-dimension dependence.

This route only reaches the polynomial representations indexed by such partitions. Jordan's Gel'fand--Tsetlin construction below reaches all $U(n)$ irreps whose highest-weight entries are polynomially bounded, including negative weights.

### 4. Gel'fand--Tsetlin sparse-Hamiltonian construction for $U(n)$

A Gel'fand pattern of width $n$ has integer entries $m_{i,j}$ arranged in rows, with interlacing constraints
$$
m_{j,n+1}\ge m_{j,n}\ge m_{j+1,n+1}.
$$
The top row is the highest weight. The irrep acts on the span of all patterns with that weight.

The Gel'fand--Tsetlin formulas give sparse actions for adjacent Lie-algebra generators. With
$$
l_{p,q}=m_{p,q}-p,
$$
the raising and lowering generators act as
$$
a_{\vec m}(E_{p-1,p})M = \sum_{j=1}^{p-1} a^j_{p-1} M^{+j}_{p-1},
$$
$$
a_{\vec m}(E_{p,p-1})M = \sum_{j=1}^{p-1} b^j_{p-1} M^{-j}_{p-1},
$$
and the diagonal generator is
$$
a_{\vec m}(E_{p,p})M
= \left(\sum_{i=1}^p m_{i,p}-\sum_{j=1}^{p-1}m_{j,p-1}\right)M.
$$
The coefficients $a^j_{p-1}$ and $b^j_{p-1}$ are explicit products of nearby $l_{p,q}$ terms, so a row of the representation matrix can be listed in polynomial time when the highest weight is polynomially bounded.

For a two-level unitary $u_p=I_p\oplus u\oplus I_{n-p-2}$, choose $H_p$ with $e^{H_p}=u_p$. The represented generator $a_{\vec m}(H_p)$ is row-computable and row-sparse. Its entries are polynomially bounded, and Appendix A proves
$$
\|a_{\vec m}(H_p)\|
$$
is independent of $p$, reducing the norm bound to the $p=0$ case. Sparse Hamiltonian simulation then implements
$$
A_{\vec m}(u_p)=e^{a_{\vec m}(H_p)}
$$
in polynomial time. The paper cites Aharonov--Ta-Shma's sparse simulation method; in this vault this connects to [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes|Berry--Ahokas--Cleve--Sanders]] and the later trick cards [[Sparse Hamiltonian Simulation via Coloring Decomposition]] and [[1-Sparse Hamiltonian Simulation via 2×2 Blocks]].

Any $n\times n$ unitary is a product of polynomially many two-level unitaries, and any non-adjacent two-level unitary $E(u,i,j)$ can be built from adjacent two-level unitaries using swaps. Concatenating the corresponding represented circuits implements $A_{\vec m}(U)$.

### 5. Extensions to $SU(n)$ and $SO(n)$

For $SU(n)$, restrict $U(n)$ irreps. Two $U(n)$ weights $\vec l,\vec m$ are projectively equivalent iff $m_i=l_i+s$ for all $i$ and some integer $s$; choosing one representative from each class gives the $SU(n)$ irreps.

For $SO(n)$, the paper uses the orthogonal-group Gel'fand--Tsetlin bases. The even and odd cases have different interlacing rules and different sparse formulas for the adjacent generators $I_{q+1,q}=E_{q+1,q}-E_{q,q+1}$. The structure is the same for quantum algorithms: adjacent generators are sparse and row-computable on Gel'fand patterns, so sparse Hamiltonian simulation implements polynomial-highest-weight irreps.

### 6. Direct Young--Yamanouchi algorithm for $S_n$

The Young--Yamanouchi basis is indexed by standard Young tableaux. It is enough to implement adjacent transpositions $\sigma_i=(i,i+1)$, since bubble sort decomposes any $\pi\in S_n$ into $O(n^2)$ adjacent transpositions.

For a tableau $\Lambda$,
$$
\rho_\lambda(\sigma_i)\Lambda
= \frac{1}{\tau_i^\Lambda}\Lambda
+ \sqrt{1-\frac{1}{(\tau_i^\Lambda)^2}}\,\Lambda',
$$
where $\Lambda'$ swaps boxes $i$ and $i+1$, and $\tau_i^\Lambda$ is the axial distance from box $i+1$ to box $i$ (down/left positive, up/right negative). If the swapped tableau is nonstandard, then $\tau_i^\Lambda=\pm 1$ and the invalid component has coefficient zero.

Thus each adjacent-transposition representation is a direct sum of $1\times 1$ and $2\times 2$ blocks with easily computed entries. The paper invokes the same sparse-unitary implementation idea from Aharonov--Ta-Shma to implement each block-structured unitary in polynomial time.

### 7. Alternating-group irreps

Every $S_n$ irrep restricts to $A_n$. If the Young diagram $\lambda$ is not self-conjugate, the restriction remains irreducible. The $S_n$ algorithm already solves the $A_n$ matrix-element problem.

If $\lambda$ is self-conjugate, the restricted representation splits into two irreps $\rho_\lambda^+$ and $\rho_\lambda^-$. The invariant subspaces are the $\pm 1$ eigenspaces of the associator
$$
S\Lambda = i^{(n-d(\lambda))/2}\operatorname{sign}(w_\Lambda)\widehat\Lambda,
$$
where $d(\lambda)$ is the diagonal length, $\widehat\Lambda$ is the conjugate tableau, and $w_\Lambda$ maps $\Lambda$ to typewriter order.

A basis vector for either eigenspace is a simple two-term superposition of $\Lambda$ and $\widehat\Lambda$ (with phase depending on parity). Therefore any $A_n$ irrep matrix element is a linear combination of four $S_n$ Young--Yamanouchi matrix elements, each already estimable.

### 8. Classical algorithms for characters

For $S_n$, the normalized character is
$$
\frac{\chi_\lambda(\pi)}{d_\lambda}.
$$
Jordan uses Roichman's formula
$$
\chi^\lambda_\mu = \sum_\Lambda W_\mu(\Lambda),
$$
where $\Lambda$ ranges over standard Young tableaux of shape $\lambda$ and $W_\mu(\Lambda)\in\{-1,0,1\}$ is computable in polynomial time from local tableau relations. Greene--Nijenhuis--Wilf hook-walk sampling draws a uniformly random standard Young tableau in $O(n^2)$ time, so averaging $W_\mu(\Lambda)$ gives an additive estimate of the normalized character in randomized polynomial time.

For $U(n)$, $SU(n)$, and $SO(n)$, the Weyl character formula reduces to determinant ratios. For example, for $U(n)$ with eigenvalues $\lambda_1,\ldots,\lambda_n$ and highest weight $\vec m$, set
$$
l_i=m_i+n-i.
$$
Then
$$
\chi^{U(n)}_{\vec m}(u)=\frac{\det A}{\det B},\qquad
A_{ij}=\lambda_i^{l_j},\quad B_{ij}=\lambda_i^{n-j}.
$$
Degenerate spectra are handled by limits. These determinant formulas give classical polynomial-time character computation for the groups treated here.

## Key results

### Main positive results

For $S_n$ and $A_n$:
$$
\text{Any matrix element of any irrep can be estimated to }\pm\epsilon
\text{ in }\operatorname{poly}(n,1/\epsilon)\text{ quantum time.}
$$
The success probability is boosted in the usual way; the paper states the $S_n$ problem as solvable with probability $1-\delta$ in
$$
\operatorname{poly}(n,1/\epsilon,\log(1/\delta)).
$$

For $U(n)$, $SU(n)$, and $SO(n)$:
$$
\text{Any matrix element of any irrep with polynomial highest weight can be estimated to }\pm\epsilon
\text{ in }\operatorname{poly}(n,1/\epsilon)\text{ quantum time.}
$$

The Hadamard-test sampling cost is explicitly
$$
O(1/\epsilon^2)
$$
measurements for additive error $\epsilon$ in a real or imaginary part.

### Average-case caveat for $S_n$ matrix elements

For any unitary irrep $\rho_\lambda(\pi)$ of dimension $d_\lambda$, the RMS matrix element is
$$
\operatorname{RMS}(\rho_\lambda(\pi))
= \sqrt{\frac{1}{d_\lambda^2}\sum_{a,b}|\langle a|\rho_\lambda(\pi)|b\rangle|^2}
=\frac{1}{\sqrt{d_\lambda}}.
$$
So in exponentially large irreps, a random matrix element is typically exponentially small. A polynomial additive estimate is often no better than guessing zero.

Jordan identifies a more promising class of nontrivial instances using Biane's asymptotic character theory. For a fixed permutation $\pi\in S_k$ embedded into $S_n$, and Young diagrams $\lambda_n$ converging to a fixed shape $\omega$ after $1/\sqrt n$ scaling,
$$
\frac{\chi_{\lambda_n}(\pi)}{d_{\lambda_n}}
= C_\pi(\omega)n^{-|\pi|/2}+O(n^{-|\pi|/2-1}),
$$
where $|\pi|$ is the minimum number of arbitrary transpositions needed to obtain $\pi$.

He proposes candidate hard instances with
$$
s(\pi)=O(1),\qquad l(\pi)=\Omega(n),\qquad r(\pi)=\Omega(n),
$$
where $s(\pi)$ is the number of moved points, $l(\pi)$ the largest moved point, and $r(\pi)$ the minimum number of adjacent transpositions needed to construct $\pi$. The transposition $(1,n)$ is the simplest example. This is explicitly a hypothesis, not a hardness theorem.

### Character results

For normalized characters of $S_n$:
$$
\left|\chi_{\mathrm{out}}-\frac{\chi_\lambda(\pi)}{d_\lambda}\right|\le \epsilon
$$
can be achieved by a classical randomized polynomial-time algorithm.

For $U(n)$, $SU(n)$, and $SO(n)$ characters, determinant forms of the Weyl character formula give classical polynomial-time algorithms for the cases treated in the paper.

## Comparison with prior work

| Problem | Earlier route | Jordan's route | What changes |
|---|---|---|---|
| $S_n$ irrep matrix element | Use Beals's QFT over $S_n$ | Also gives direct Young--Yamanouchi adjacent-transposition circuit | Direct route extends to $A_n$ and makes the basis dependence explicit |
| Selected $U(d)$ irreps | [[The Quantum Schur Transform I Efficient Qudit Circuits (Bacon-Chuang-Harrow 2006) — Paper Notes|Bacon--Chuang--Harrow Schur transform]] | Schur route noted as immediate | Efficient for partitions arising in $(\mathbb C^d)^{\otimes n}$ |
| Polynomial-highest-weight $U(n)$ irreps | Classical algorithms manipulate full representation matrices | Gel'fand--Tsetlin basis + sparse Hamiltonian simulation | Avoids explicitly storing exponentially large matrices |
| $SU(n)$ and $SO(n)$ irreps | Classical representation-theory formulas | Sparse adjacent Lie-algebra generators in GT-type bases | Polynomial-time quantum matrix-element estimation for polynomial highest weight |
| $S_n$ normalized characters | Exact computation is #P-hard | Roichman formula + hook-walk sampling | Additive normalized characters are in BPP |
| Braid-group Jones representations | [[A Polynomial Quantum Algorithm for Approximating the Jones Polynomial (Aharonov-Jones-Landau 2006) — Paper Notes|AJL]] gives BQP-complete matrix-element estimation | Contrast case: symmetric-group matrix-element hardness unknown | Suggests braid-group representations are harder than their $S_n$ analogues |

## Limits / caveats

- The estimates are additive. For exponentially small matrix elements, additive $1/\operatorname{poly}(n)$ precision may carry little information.
- The paper does not prove BQP-hardness for $S_n$ or $A_n$ matrix elements. The proposed hard family is a plausibility argument based on avoiding obvious classical shortcuts.
- The Lie-group algorithms require polynomially bounded highest-weight entries. Scaling polynomially in the number of bits of the highest weight is left open.
- The $U(n)$ construction assumes an explicit $n\times n$ unitary input. It is not a polylogarithmic-in-$n$ representation algorithm for succinctly described huge unitaries.
- Characters are a poor target for speedup here: normalized $S_n$ characters are classically easy to additive precision, and classical determinant formulas handle the listed Lie groups.
- The sparse-Hamiltonian step uses the simulation technology available at the time. Modern simulation improves constants and precision scaling, but it does not change the qualitative statement.

## Reusable ideas

1. [[Irrep Extraction by Fourier-Conjugating the Regular Representation]] — use $\mathrm{QFT}_G U_g \mathrm{QFT}_G^{-1}$ to expose all irreps as blocks.
2. [[Gelfand-Tsetlin Sparse Hamiltonian Implementation of Lie-Group Irreps]] — turn sparse adjacent Lie-algebra formulas into simulable Hamiltonians for exponentially large representation spaces.
3. [[Young--Yamanouchi Adjacent-Transposition Blocks]] — implement $S_n$ irreps by decomposing permutations into adjacent swaps whose action is a direct sum of $1\times1$ and $2\times2$ blocks.
4. [[Normalized Character Estimation by Random Tableau Sampling]] — estimate $S_n$ normalized characters by sampling standard Young tableaux and averaging Roichman weights.

## References within this paper

- Robert Beals (1997), *Quantum computation of Fourier transforms over symmetric groups* — source of the efficient $S_n$ QFT used in the finite-group route; Zoo entry exists but is not yet a paper note.
- [[The Quantum Schur Transform I Efficient Qudit Circuits (Bacon-Chuang-Harrow 2006) — Paper Notes|Dave Bacon, Isaac Chuang, and Aram Harrow (2006/2007)]], *The quantum Schur transform I* — efficient Schur transform for qudits.
- Dorit Aharonov and Amnon Ta-Shma (2003), *Adiabatic quantum state generation and statistical zero knowledge* — cited for row-computable sparse Hamiltonian simulation.
- [[A Polynomial Quantum Algorithm for Approximating the Jones Polynomial (Aharonov-Jones-Landau 2006) — Paper Notes|Aharonov--Jones--Landau (2006)]] — comparison point: braid-group representation matrix elements give BQP-complete Jones estimation.
- [[The BQP-Hardness of Approximating the Jones Polynomial (Aharonov-Arad 2011) — Paper Notes|Aharonov--Arad (2006/2011)]] — related BQP-hardness result for Jones polynomial approximation.
- [[Power of One Bit of Quantum Information (Knill-Laflamme 1998) — Paper Notes|Knill--Laflamme (1998)]] and Shor--Jordan (2007/2008) — DQC1 trace-estimation context for normalized characters of braid representations.
- Yuval Roichman (1999), *Characters of the symmetric groups: formulas, estimates and applications* — formula enabling the BPP algorithm for normalized $S_n$ characters.
- Greene--Nijenhuis--Wilf (1979), *A probabilistic proof of a formula for the number of Young tableaux of a given shape* — hook-walk sampler for uniform standard Young tableaux.
- Philippe Biane (1998), *Representations of symmetric groups and free probability* — asymptotic character estimate used to identify nontrivial $S_n$ matrix-element regimes.
- Fulton--Harris (1991), *Representation Theory* — Weyl character formula references for Lie-group characters.
- Zalka (2004), *Implementing high dimensional unitary representations of $SU(2)$ on a quantum computer* — earlier small-rank representation implementation.

## Cross-links

### Paper notes

- [[The Power of Basis Selection in Fourier Sampling (Moore-Rockmore-Russell-Schulman 2003) — Paper Notes]]
- [[The Quantum Schur Transform I Efficient Qudit Circuits (Bacon-Chuang-Harrow 2006) — Paper Notes]]
- [[Quantum Complexity of the Kronecker Coefficients (Bravyi-Chowdhury-Gosset-Havlicek-Zhu 2024) — Paper Notes]] — later use of symmetric-group representation measurements for Kronecker coefficients and $\#\mathrm{BQP}$ counting
- [[An Efficient High Dimensional Quantum Schur Transform (Krovi 2019) — Paper Notes]]
- [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes]]
- [[A Polynomial Quantum Algorithm for Approximating the Jones Polynomial (Aharonov-Jones-Landau 2006) — Paper Notes]]
- [[The BQP-Hardness of Approximating the Jones Polynomial (Aharonov-Arad 2011) — Paper Notes]]
- [[Power of One Bit of Quantum Information (Knill-Laflamme 1998) — Paper Notes]]

### Trick cards

- [[Irrep Extraction by Fourier-Conjugating the Regular Representation]]
- [[Gelfand-Tsetlin Sparse Hamiltonian Implementation of Lie-Group Irreps]]
- [[Young--Yamanouchi Adjacent-Transposition Blocks]]
- [[Normalized Character Estimation by Random Tableau Sampling]]
- [[Sparse Hamiltonian Simulation via Coloring Decomposition]]
- [[1-Sparse Hamiltonian Simulation via 2×2 Blocks]]
