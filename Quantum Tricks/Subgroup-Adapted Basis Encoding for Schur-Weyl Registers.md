# Subgroup-Adapted Basis Encoding for Schur-Weyl Registers

> **Source:** Bacon--Chuang--Harrow, arXiv:quant-ph/0601001
> **Tags:** #trick #Schur-transform #representation-theory #basis-encoding #Young-tableaux

## What it does
Chooses Schur-basis labels so restriction along subgroup towers becomes reversible register arithmetic.

## The trick
The Schur decomposition uses two representation labels:

$$
(\mathbb C^d)^{\otimes n}\cong \bigoplus_\lambda Q^d_\lambda\otimes P_\lambda.
$$

For the $U(d)$ side, use the subgroup tower

$$
U(1)\subset U(2)\subset\cdots\subset U(d).
$$

A Gelfand--Zetlin basis vector is a chain of interlacing partitions

$$
q=(q_d=\lambda,q_{d-1},\ldots,q_1),
\qquad q_{k-1}\preceq q_k.
$$

For the symmetric-group side, use

$$
S_1\subset S_2\subset\cdots\subset S_n.
$$

A Young--Yamanouchi basis vector is a chain

$$
p=(p_n=\lambda,p_{n-1},\ldots,p_1=(1)),
$$

where each $p_{k-1}$ is obtained from $p_k$ by removing one box.

Because both branching rules are multiplicity-free, no additional multiplicity label is needed at each restriction step. The basis vector is just the path through the branching graph.

## When to reach for it
- You need a coherent register format for irrep labels and multiplicity spaces.
- A representation decomposes along a subgroup tower with multiplicity-free branching.
- You want an algorithm where adding/removing boxes corresponds to simple reversible updates.

## Complexity
A crude encoding of a Gelfand--Zetlin pattern uses $O(d^2\log n)$ bits. A Young--Yamanouchi path can be stored as $O(n\log d)$ row-addition labels, or compressed further using [[Young-Path Ranking for Multiplicity Register Compression]].

## Caveat
The encoding is basis-dependent. It is excellent for CG recursion and Schur transforms, but downstream circuits must keep the same phase and branching conventions.

## Related notes
- [[The Quantum Schur Transform I Efficient Qudit Circuits (Bacon-Chuang-Harrow 2006) — Paper Notes]]
- [[Clebsch-Gordan Cascade for Schur Transforms]]
- [[Wigner-Eckart Recursion for Unitary-Group Clebsch-Gordan Transforms]]
- [[Young-Path Ranking for Multiplicity Register Compression]]
- [[An Efficient High Dimensional Quantum Schur Transform (Krovi 2019) — Paper Notes]]
