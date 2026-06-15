# Wigner-Eckart Recursion for Unitary-Group Clebsch-Gordan Transforms

> **Source:** Bacon--Chuang--Harrow, arXiv:quant-ph/0601001
> **Tags:** #trick #Clebsch-Gordan #Wigner-Eckart #unitary-group #representation-theory

## What it does
Reduces a $U(d)$ add-one-box Clebsch--Gordan transform to a $U(d-1)$ Clebsch--Gordan transform plus a controlled $d\times d$ reduced-Wigner unitary.

## The trick
The target transform is

$$
Q^d_\mu\otimes Q^d_{(1)}
\cong \bigoplus_{j:\,\mu+e_j\in\mathbb Z^d_{++}} Q^d_{\mu+e_j}.
$$

Write the Gelfand--Zetlin basis vector in $Q^d_\mu$ as

$$
|q\rangle=|\mu'\rangle |q^{(d-2)}\rangle,
\qquad \mu'\preceq \mu,
$$

where $\mu'$ labels the $U(d-1)$ irrep appearing under restriction. Express the $U(d)$ CG transform through tensor operators

$$
U^{[d]}_{\mathrm{CG}}|\mu\rangle|q\rangle|i\rangle
=|\mu\rangle\sum_j |\mu+e_j\rangle T^{\mu,j}_i|q\rangle.
$$

Restrict the tensor operators to $U(d-1)$. If $i<d$, the components transform as the defining irrep $Q^{d-1}_{(1)}$; if $i=d$, they transform trivially. Wigner--Eckart then factors the matrix elements as

$$
\langle q'|T^{\mu,j,\mu',j'}_i|q\rangle
=\widehat T^{\mu,j,\mu',j'}
\langle \mu',\mu'+e_{j'},q'|U^{[d-1]}_{\mathrm{CG}}|\mu',q,i\rangle.
$$

So the algorithm is:

1. recursively perform $U^{[d-1]}_{\mathrm{CG}}$ when $i<d$;
2. relabel $i=d$ as $j'=0$ in the trivial branch;
3. apply the reduced-Wigner unitary $\widehat T^{[d]}_{\mu,\mu'}$ on the $j'$ register;
4. repack the Gelfand--Zetlin label for $Q^d_{\mu+e_j}$.

The reduced coefficients are explicit Biedenharn--Louck products of differences of shifted partition entries. They are computable using $\operatorname{poly}(d,\log n)$ arithmetic.

## When to reach for it
- Implementing Clebsch--Gordan transforms for $U(d)$ in subgroup-adapted bases.
- Avoiding construction of a huge CG matrix by exploiting the subgroup tower $U(1)\subset\cdots\subset U(d)$.
- Any representation-theoretic circuit where tensor-operator matrix elements factor into a reduced coefficient times a lower-rank transform.

## Complexity
BCH state the resulting add-one-box CG transform has runtime

$$
d^3\operatorname{poly}(\log n,\log(1/\varepsilon)).
$$

The recursion depth is $d$, and each level uses a controlled $d\times d$ unitary whose entries are computed to the required precision.

## Caveat
The method is tied to having a clean subgroup tower with multiplicity-free branching and efficiently computable reduced Wigner coefficients. It is not an automatic recipe for arbitrary groups.

## Related notes
- [[The Quantum Schur Transform I Efficient Qudit Circuits (Bacon-Chuang-Harrow 2006) — Paper Notes]]
- [[Clebsch-Gordan Cascade for Schur Transforms]]
- [[Subgroup-Adapted Basis Encoding for Schur-Weyl Registers]]
