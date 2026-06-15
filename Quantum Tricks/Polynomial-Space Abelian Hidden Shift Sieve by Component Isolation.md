# Polynomial-Space Abelian Hidden Shift Sieve by Component Isolation

> **Source:** Childs--Jao--Soukharev, arXiv:1012.4019
> **Tags:** #trick #hidden-shift #quantum-sieve #abelian-groups #polynomial-space

## What it does

Extends Regev's polynomial-space hidden-shift sieve from $\mathbb Z_{2^n}$ to arbitrary finite abelian groups by first isolating one cyclic component at a time.

## The trick

For a hidden shift over
$$
A=\mathbb Z_{N_1}\times\cdots\times\mathbb Z_{N_t},
$$
Fourier sampling gives labelled qubit states
$$
|\psi_x\rangle=\frac{1}{\sqrt2}\left(|0\rangle+
\exp\!\left(2\pi i\sum_{j=1}^t \frac{s_jx_j}{N_j}\right)|1\rangle\right),
$$
with known uniformly random $x\in A$.

The sieve wants states whose labels are standard basis labels, e.g.
$$
(0,\ldots,0,2^j,0,\ldots,0),
$$
so that a short inverse-QFT reconstruction recovers the corresponding shift component.

The component-isolation step combines $k$ labelled qubits and measures a coarse quotient of the mixed-radix value
$$
\mu(x)=\sum_{j=1}^{t-1}x_j\prod_{j'<j}N_{j'}.
$$
Pairing two basis strings with the same coarse quotient replaces the label by a difference $x'$. This shrinks $\mu(x')$ while preserving uniformity in the remaining cyclic component. Repeating this drives all unwanted components to zero.

After isolation, use cyclic label reduction:

- for odd $N_i$, apply the high-bit reduction under the automorphism $x\mapsto 2^{-j}x$;
- for $N_i=2^n$, first cancel low-order bits, then reduce high-order bits.

This produces the states needed for Høyer's reconstruction lemma.

## When to reach for it

- Hidden shift over a non-cyclic abelian group.
- Dihedral-HSP-style algorithms where the group decomposition is known but labels live in several cyclic coordinates.
- Cases where quantum space is the binding resource and a Kuperberg pile of states is too expensive.

## Complexity

The algorithm solves hidden shift over a finite abelian group $A$ in
$$
L_{|A|}\!\left(\frac12,\sqrt2\right)
$$
time and query complexity using
$$
\operatorname{poly}(\log |A|)
$$
space. The loops over cyclic components add only polynomial overhead.

## Caveat

This saves quantum space but worsens the time constant relative to Kuperberg's original sieve. In [[Constructing Elliptic Curve Isogenies in Quantum Subexponential Time (Childs-Jao-Soukharev 2014) — Paper Notes]], that extra hidden-shift cost appears additively in the $L$-constant for the isogeny attack.

## Related notes

- [[Constructing Elliptic Curve Isogenies in Quantum Subexponential Time (Childs-Jao-Soukharev 2014) — Paper Notes]]
- [[A Subexponential Time Algorithm for the Dihedral Hidden Subgroup Problem with Polynomial Space (Regev 2004) — Paper Notes]]
- [[A Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2005) — Paper Notes]]
- [[Batch Qubit Combination via Modular Inner Products]]
- [[Quantum Sieve for Labelled Qubits]]
- [[Injective Group-Action Hidden Shift from Principal Homogeneous Spaces]]
