# Clebsch-Gordan Cascade for Schur Transforms

> **Source:** Bacon--Chuang--Harrow, arXiv:quant-ph/0601001
> **Tags:** #trick #Schur-transform #Clebsch-Gordan #representation-theory #quantum-circuits

## What it does
Builds the Schur transform by adding one copy of the defining representation at a time.

## The trick
For Schur-Weyl duality,

$$
(\mathbb C^d)^{\otimes n}\cong \bigoplus_{\lambda\in I_{d,n}} Q^d_\lambda\otimes P_\lambda.
$$

Rather than diagonalising the whole tensor power at once, process the qudits sequentially. Start with

$$
|\lambda^{(1)}\rangle=|(1)\rangle,\qquad |q_1\rangle=|i_1\rangle.
$$

At step $k$, apply the defining-irrep Clebsch--Gordan transform

$$
Q^d_{\lambda^{(k)}}\otimes Q^d_{(1)}
\cong \bigoplus_{j:\,\lambda^{(k)}+e_j\in \mathbb Z^d_{++}} Q^d_{\lambda^{(k)}+e_j}
$$

to

$$
|\lambda^{(k)}\rangle |q_k\rangle |i_{k+1}\rangle.
$$

The output is

$$
|j_k\rangle |\lambda^{(k+1)}\rangle |q_{k+1}\rangle,
\qquad \lambda^{(k+1)}=\lambda^{(k)}+e_{j_k}.
$$

After $n-1$ steps, the final irrep label and Gelfand--Zetlin vector are $|\lambda^{(n)}\rangle|q_n\rangle$, while the history $|j_1,\ldots,j_{n-1}\rangle$ is the multiplicity register: it is equivalent to a Young--Yamanouchi path for $P_\lambda$.

## When to reach for it
- Implementing Schur-Weyl transforms where the tensor factors arrive one at a time.
- Turning a global irrep decomposition problem into repeated two-factor decompositions.
- Building multiplicity labels coherently rather than computing them after the fact.

## Complexity
If one defining-irrep CG transform costs $T_{\mathrm{CG}}(n,d,\varepsilon)$, the cascade costs

$$
n\bigl(T_{\mathrm{CG}}(n,d,\varepsilon/n)+O(1)\bigr),
$$

plus optional $\operatorname{poly}(n)$ time to compress the Young path.

## Caveat
The cascade is only as efficient as the CG transform. For BCH, this gives polynomial dependence on $d$; for very large $d$, [[An Efficient High Dimensional Quantum Schur Transform (Krovi 2019) — Paper Notes|Krovi's later algorithm]] is better.

## Related notes
- [[The Quantum Schur Transform I Efficient Qudit Circuits (Bacon-Chuang-Harrow 2006) — Paper Notes]]
- [[Wigner-Eckart Recursion for Unitary-Group Clebsch-Gordan Transforms]]
- [[Subgroup-Adapted Basis Encoding for Schur-Weyl Registers]]
- [[An Efficient High Dimensional Quantum Schur Transform (Krovi 2019) — Paper Notes]]
