# Quantum Complexity of the Kronecker Coefficients (Bravyi-Chowdhury-Gosset-Havlicek-Zhu 2024) — Paper Notes

> **Source:** Sergey Bravyi, Anirban Chowdhury, David Gosset, Vojtěch Havlíček, and Guanyu Zhu, *Quantum complexity of the Kronecker coefficients*, arXiv:2302.11454; PRX Quantum 5, 010329 (2024)
> **Links:** [arXiv](https://arxiv.org/abs/2302.11454) · [PDF](https://arxiv.org/pdf/2302.11454)
> **Tags:** #quantum-complexity #representation-theory #symmetric-group #QMA #counting-complexity #quantum-algorithms

---

## The computational problem

The paper studies the computational status of Kronecker coefficients for the symmetric group. For partitions $\mu,\nu,\lambda \vdash n$, the Kronecker coefficient $g_{\mu\nu\lambda}$ is the multiplicity of the irrep $\rho_\lambda$ in the tensor product
$$
\rho_\mu \otimes \rho_\nu \cong \bigoplus_{\lambda \vdash n} g_{\mu\nu\lambda}\rho_\lambda .
$$
The input partitions are given in unary, so the input size is $O(n)$.

**ExactKron.** Given $\mu,\nu,\lambda\vdash n$, compute $g_{\mu\nu\lambda}$ exactly.

**ApproxKron.** Given $\mu,\nu,\lambda\vdash n$ and $\epsilon=\Omega(1/\operatorname{poly}(n))$, output $\tilde g$ satisfying
$$
(1-\epsilon)g_{\mu\nu\lambda}\leq \tilde g\leq (1+\epsilon)g_{\mu\nu\lambda}.
$$

The paper also treats the row-sum problem for the character table of $S_n$:
$$
R_\lambda = \sum_{\mu\vdash n}\chi_\lambda(\mu),
$$
where $\chi_\lambda(\mu)$ denotes the value of $\chi_\lambda$ on a conjugacy class of cycle type $\mu$.

## What the paper does

It shows that $g_{\mu\nu\lambda}$ is, up to the known factor $d_\mu d_\nu d_\lambda$, the rank of an efficiently measurable quantum projector. Hence $(\mu,\nu,\lambda)\mapsto d_\mu d_\nu d_\lambda g_{\mu\nu\lambda}$ is in $\#\mathrm{BQP}$, ApproxKron reduces to quantum approximate counting (QXC), and positivity of Kronecker coefficients is in QMA.

My assessment: the result does not give an efficient relative-error algorithm for Kronecker coefficients. Its value is a clean complexity placement: Kronecker coefficients look less like arbitrary GapP objects and more like dimensions of quantum witness spaces. That is a useful lens for the old “what do Kronecker coefficients count?” question.

## The algorithm / construction

### 1. Representation projectors from characters

For any unitary representation $\rho$ of $S_n$ on a Hilbert space $\mathcal H$, define
$$
\Pi_\lambda = \frac{d_\lambda}{n!}\sum_{\alpha\in S_n}\chi_\lambda(\alpha)\rho(\alpha).
$$
Using character orthogonality, the supplementary material proves that the operators $\{\Pi_\lambda:\lambda\vdash n\}$ are mutually orthogonal projectors and sum to identity. This is the irrep-label measurement for the representation $\rho$.

For the left-regular representation on $\mathbb C S_n$, the same measurement is weak Fourier sampling. Apply Beals's efficient quantum Fourier transform over $S_n$, measure the irrep label $\omega$, then uncompute the Fourier transform. This is the representation-theoretic infrastructure also used in [[Fast Quantum Algorithms for Approximating Some Irreducible Representations of Groups (Jordan 2009) — Paper Notes|Jordan's irrep-estimation algorithms]].

### 2. The Kronecker projector

Let $\Pi^L_\mu,\Pi^L_\nu,\Pi^L_\lambda$ be the left-regular projectors on three copies of $\mathbb C S_n$. On $(\mathbb C S_n)^{\otimes 3}$ define the diagonal-invariance projector
$$
Q = \frac{1}{n!}\sum_{\sigma\in S_n}\sigma\otimes\sigma\otimes\sigma .
$$
This is the projector onto the trivial irrep of the threefold tensor-product representation $\rho(\sigma)=\sigma\otimes\sigma\otimes\sigma$. Since $Q$ commutes with $\Pi^L_\mu\otimes\Pi^L_\nu\otimes\Pi^L_\lambda$, define
$$
P_{\mu\nu\lambda}
=Q(\Pi^L_\mu\otimes\Pi^L_\nu\otimes\Pi^L_\lambda)
=(\Pi^L_\mu\otimes\Pi^L_\nu\otimes\Pi^L_\lambda)Q.
$$
This projector selects the simultaneous subspace with Fourier labels $(\mu,\nu,\lambda)$ and diagonal $S_n$-invariance.

### 3. Measuring the projector

The circuit for $\{P_{\mu\nu\lambda},I-P_{\mu\nu\lambda}\}$ is:

1. On each of the three $\mathbb C S_n$ registers, perform weak Fourier sampling and reject unless the irrep labels are $\mu,\nu,\lambda$.
2. Use generalized phase estimation for the representation $\rho(\sigma)=\sigma\otimes\sigma\otimes\sigma$ and reject unless the measured irrep is the trivial representation $\tau$.
3. Accept iff both tests accept.

The controlled representation map needed by generalized phase estimation is
$$
C_\rho|\alpha\rangle|\psi\rangle=|\alpha\rangle\rho(\alpha)|\psi\rangle,
$$
which is efficient here because it only left-multiplies three permutation registers. The circuit size is $\operatorname{poly}(n)$, assuming Beals's $S_n$ QFT.

### 4. Why the rank is the Kronecker coefficient

The character formula
$$
g_{\mu\nu\lambda}=\frac{1}{n!}\sum_{\sigma\in S_n}\chi_\mu(\sigma)\chi_\nu(\sigma)\chi_\lambda(\sigma)
$$
combined with the fact that the trace of a non-identity left-regular permutation is zero gives
$$
\operatorname{Tr}(P_{\mu\nu\lambda})=d_\mu d_\nu d_\lambda g_{\mu\nu\lambda}.
$$
So the number of $+1$ eigenvectors of an efficiently measurable projector is exactly $d_\mu d_\nu d_\lambda g_{\mu\nu\lambda}$.

### 5. Row sums of the character table

For the conjugation representation
$$
\rho_c(\pi)|\sigma\rangle = |\pi\sigma\pi^{-1}\rangle
$$
on $\mathbb C S_n$, the multiplicity of $\rho_\lambda$ is the row sum $R_\lambda$. Generalized phase estimation measures the projector
$$
\Pi^c_\lambda=\frac{d_\lambda}{n!}\sum_{g\in S_n}\chi_\lambda(g)\rho_c(g),
$$
and the trace identity is
$$
\operatorname{Tr}(\Pi^c_\lambda)=d_\lambda R_\lambda.
$$
Thus $d_\lambda R_\lambda\in\#\mathrm{BQP}$, relative-error approximation of $R_\lambda$ reduces to QXC, and positivity of $R_\lambda$ is in QMA.

### 6. Additive estimation of normalized Kronecker coefficients

The paper also describes a direct sampling algorithm implicit in Moore--Russell--Śniady. Prepare the maximally mixed state on the image of $\Pi^L_\mu\otimes\Pi^L_\nu$, then measure the irrep label $\lambda$ for the tensor-product regular representation. The probability of observing $\lambda$ is
$$
p(\lambda)=\frac{d_\lambda g_{\mu\nu\lambda}}{d_\mu d_\nu}.
$$
With $O(1/\epsilon^2)$ samples one estimates $p(\lambda)$ to additive error $\epsilon$, hence estimates $g_{\mu\nu\lambda}$ to additive error
$$
\delta=\epsilon\frac{d_\mu d_\nu}{d_\lambda}.
$$
This solves ExactKron when $\min(d_\mu,d_\nu,d_\lambda)\leq \operatorname{poly}(n)$ by taking $\epsilon=1/\operatorname{poly}(n)$ small enough to make $\delta<1/2$.

## Key results

| Result | Statement | Consequence |
|---|---|---|
| Lemma 1 | $g_{\mu\nu\lambda}=\frac{1}{d_\mu d_\nu d_\lambda}\operatorname{Tr}(P_{\mu\nu\lambda})$ | Kronecker coefficients are scaled projector ranks |
| Theorem 1 | For any $\mu,\nu,\lambda\vdash n$, a $\operatorname{poly}(n)$-size quantum circuit implements $\{P_{\mu\nu\lambda},I-P_{\mu\nu\lambda}\}$ | Efficient QMA verifier for positivity |
| Corollary 1 | $(\mu,\nu,\lambda)\mapsto d_\mu d_\nu d_\lambda g_{\mu\nu\lambda}$ is in $\#\mathrm{BQP}$; ApproxKron reduces to QXC | Relative-error approximation is no harder than quantum approximate counting |
| Corollary 2 | Deciding whether $g_{\mu\nu\lambda}>0$ is in QMA | Complements NP-hardness by Ikenmeyer--Mulmuley--Walter |
| Theorem 2 | A $\operatorname{poly}(n)$ circuit measures $\{\Pi^c_\lambda,I-\Pi^c_\lambda\}$ and $\operatorname{Tr}(\Pi^c_\lambda)=d_\lambda R_\lambda$ | Row sums get the same $\#\mathrm{BQP}$/QXC/QMA placement |
| Lemma 2 | A $\operatorname{poly}(n)$ quantum circuit samples $p(\lambda)=d_\lambda g_{\mu\nu\lambda}/(d_\mu d_\nu)$ | Additive estimator for normalized Kronecker coefficients |

## Comparison with prior work

| Work | What it says | Relation to this paper |
|---|---|---|
| Bürgisser--Ikenmeyer (2008) | ExactKron lies in GapP; #P-hardness reductions are known | Prior exact-counting complexity baseline |
| Ikenmeyer--Mulmuley--Walter (2017) | Positivity of Kronecker coefficients is NP-hard | This paper adds containment in QMA |
| Brown--Flammia--Schuch (2011) | #BQP formalises counting accepting quantum witnesses | Supplies the counting class used here |
| Bravyi--Chowdhury--Gosset--Wocjan (2022) | Quantum approximate counting/QXC captures thermal quantum Hamiltonian tasks | This paper gives a non-many-body natural reduction to QXC |
| Beals (1997) | Efficient QFT over $S_n$ | Enables weak Fourier sampling over the symmetric group |
| [[Fast Quantum Algorithms for Approximating Some Irreducible Representations of Groups (Jordan 2009) — Paper Notes|Jordan (2009)]] | Efficient quantum access to selected irrep matrix elements and normalized characters | Closest vault neighbour: same $S_n$ QFT infrastructure, but additive character/matrix-element estimation rather than counting multiplicities |
| [[The Quantum Schur Transform I Efficient Qudit Circuits (Bacon-Chuang-Harrow 2006) — Paper Notes|Bacon--Chuang--Harrow (2006)]] | Efficient Schur transform for Schur--Weyl decompositions | Related representation-transform machinery, though this paper uses Beals's $S_n$ QFT rather than the qudit Schur transform |
| [[Polynomial Time Classical Versus Quantum Algorithms for Representation Theoretic Multiplicities (Panova 2025) — Paper Notes|Panova (2025)]] | Classical algorithms for several polynomial-dimensional Kronecker and plethysm regimes | Narrows the possible quantum-speedup regimes for multiplicity algorithms built around this projector-rank viewpoint |

## Limits / caveats

- This is not an efficient relative-error algorithm for $g_{\mu\nu\lambda}$. Reducing ApproxKron to QXC is only as useful as one's ability to solve quantum approximate counting.
- The positivity result is an upper bound in QMA, not a tight classification. The paper does not prove QMA-hardness for Kronecker positivity.
- The input convention matters: partitions are given in unary. Other papers sometimes use binary encodings for partition parts.
- Additive sampling of $p(\lambda)$ can be strong enough when one irrep dimension is polynomial, but in the generic high-dimensional regime the induced additive error for $g_{\mu\nu\lambda}$ may be huge.
- My assessment: the strongest conceptual point is the projector-rank identity. The algorithmic payoff is narrower, but the identity is clean enough that it should be reusable in other multiplicity-counting questions.

## Reusable ideas

1. [[Kronecker Coefficient as a Diagonal-Invariant Fourier-Label Projector]] — encode a tensor-product multiplicity as the rank of a simultaneous irrep-label and diagonal-invariance projector.
2. [[Generalized Phase Estimation for Representation Projectors]] — implement the character projector $\Pi_\lambda$ for any efficiently controlled group representation.
3. [[Normalized Kronecker Sampling from Regular-Representation Projectors]] — sample $\lambda$ with probability $d_\lambda g_{\mu\nu\lambda}/(d_\mu d_\nu)$ by preparing mixed irrep sectors and measuring a product representation.
4. [[Character Row Sums from Conjugation-Representation Projectors]] — turn character-table row sums into ranks of conjugation-representation projectors.

## References within this paper

- Beals (1997), *Quantum computation of Fourier transforms over symmetric groups* — efficient $S_n$ QFT used for weak Fourier sampling.
- Brown--Flammia--Schuch (2011), *Computational difficulty of computing the density of states* — #BQP as quantum counting.
- Bravyi--Chowdhury--Gosset--Wocjan (2022), *Quantum Hamiltonian complexity in thermal equilibrium* — QXC/quantum approximate counting context.
- Ikenmeyer--Mulmuley--Walter (2017), *On vanishing of Kronecker coefficients* — NP-hardness of Kronecker positivity.
- Pak--Panova (2017), *On the complexity of computing Kronecker coefficients* — classical complexity of ExactKron and special cases.
- [[Fast Quantum Algorithms for Approximating Some Irreducible Representations of Groups (Jordan 2009) — Paper Notes|Jordan (2009)]] — additive approximation of normalized characters and irrep matrix elements.
- Moore--Russell--Śniady (2007), *On the impossibility of a quantum sieve algorithm for graph isomorphism* — implicit source of the normalized Kronecker sampling routine.
- [[Polynomial Time Classical Versus Quantum Algorithms for Representation Theoretic Multiplicities (Panova 2025) — Paper Notes|Panova (2025)]] — later comparison showing that polynomial-dimensional Kronecker regimes are classically tractable.
- [[Quantum Arthur-Merlin Games (Marriott-Watrous 2005) — Paper Notes|Marriott--Watrous (2005)]] — QMA amplification background used in the supplementary discussion of #BQP.
- Stanley (1999), *Positivity problems and conjectures in algebraic combinatorics* — source of the open positivity/counting questions.

## Cross-links

### Paper notes

- [[Fast Quantum Algorithms for Approximating Some Irreducible Representations of Groups (Jordan 2009) — Paper Notes]]
- [[The Quantum Schur Transform I Efficient Qudit Circuits (Bacon-Chuang-Harrow 2006) — Paper Notes]]
- [[Quantum Arthur-Merlin Games (Marriott-Watrous 2005) — Paper Notes]]
- [[Quantum NP — Local Hamiltonian is QMA-Complete (Kitaev 1999) — Paper Notes]]
- [[Quantum Algorithms for Algebraic Problems (Childs-van Dam 2010) — Paper Notes]]
- [[Polynomial Time Classical Versus Quantum Algorithms for Representation Theoretic Multiplicities (Panova 2025) — Paper Notes]]

### Trick cards

- [[Kronecker Coefficient as a Diagonal-Invariant Fourier-Label Projector]]
- [[Generalized Phase Estimation for Representation Projectors]]
- [[Normalized Kronecker Sampling from Regular-Representation Projectors]]
- [[Character Row Sums from Conjugation-Representation Projectors]]
- [[Young--Yamanouchi Adjacent-Transposition Blocks]]
- [[Subgroup-Adapted Basis Encoding for Schur-Weyl Registers]]
