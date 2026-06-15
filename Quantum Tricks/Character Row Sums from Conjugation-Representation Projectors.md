# Character Row Sums from Conjugation-Representation Projectors

> **Source:** Bravyi--Chowdhury--Gosset--Havlicek--Zhu, arXiv:2302.11454
> **Tags:** #trick #representation-theory #symmetric-group #QMA #counting-complexity

## What it does

Expresses nonnegative row sums of the $S_n$ character table as scaled ranks of efficiently measurable projectors.

## The trick

Let $S_n$ act on $\mathbb C S_n$ by conjugation:
$$
\rho_c(\pi)|\sigma\rangle=|\pi\sigma\pi^{-1}\rangle .
$$
The character of this representation is
$$
\chi_c(\pi)=|Z(\pi)|,
$$
where $Z(\pi)$ is the centralizer of $\pi$. If
$$
\rho_c\cong \bigoplus_{\lambda\vdash n} m_\lambda\rho_\lambda,
$$
then character orthogonality and the identity $|C|\,|Z(C)|=n!$ imply
$$
m_\lambda
=\frac{1}{n!}\sum_{g\in S_n}\chi_\lambda(g)\chi_c(g)
=\sum_{C\vdash n}\chi_\lambda(C)
=R_\lambda.
$$
Therefore the representation projector
$$
\Pi^c_\lambda=\frac{d_\lambda}{n!}\sum_{g\in S_n}\chi_\lambda(g)\rho_c(g)
$$
has trace
$$
\operatorname{Tr}(\Pi^c_\lambda)=d_\lambda R_\lambda.
$$
Measure $\Pi^c_\lambda$ by [[Generalized Phase Estimation for Representation Projectors]].

## When to reach for it

Use this when a character-table sum is secretly a multiplicity in a permutation representation. The key move is to identify a natural group action whose character collapses the sum via orbit-stabilizer or centralizer counts.

## Complexity

For $S_n$, conjugating one explicit permutation by another is efficient, and Beals's $S_n$ QFT gives a $\operatorname{poly}(n)$ measurement circuit. Thus $d_\lambda R_\lambda\in\#\mathrm{BQP}$, relative-error approximation of $R_\lambda$ reduces to QXC, and positivity of $R_\lambda$ is in QMA.

## Caveat

This works because row sums of the $S_n$ character table are known to be nonnegative and match multiplicities of the conjugation representation. Arbitrary signed character sums will not automatically admit a projector-rank interpretation.

## Related notes

- [[Quantum Complexity of the Kronecker Coefficients (Bravyi-Chowdhury-Gosset-Havlicek-Zhu 2024) — Paper Notes]]
- [[Generalized Phase Estimation for Representation Projectors]]
- [[Kronecker Coefficient as a Diagonal-Invariant Fourier-Label Projector]]
