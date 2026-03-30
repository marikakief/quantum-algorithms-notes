# GPE for Multiplicity Space Decomposition

> **Source:** Hari Krovi, arXiv:1804.00055; see also Aram Harrow, PhD thesis (2005)
> **Tags:** #trick #representation-theory #phase-estimation #multiplicity-space #induced-representation

## What it does
Uses generalised phase estimation (GPE) — a group-theoretic extension of [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|Kitaev's phase estimation]] — to decompose the multiplicity space of an irrep within an induced representation into its irreducible components under a subgroup.

## The trick
Given an induced representation $\uparrow^G_H \sigma$ of a subgroup $H \leq G$, each irrep $\lambda$ of $G$ appears with some multiplicity. The multiplicity space has a natural but non-canonical basis (e.g., SYTs for $S_n$). GPE reorganises this basis to align with specific irreps of $H$ — in particular, the trivial irrep when working with permutation modules.

The procedure:
1. Prepare a uniform superposition over $H$ in an ancilla register.
2. Apply controlled right multiplication $\sum_{h \in H} |h\rangle\langle h| \otimes R(h)$ on the main register.
3. Apply QFT over $H$ on the ancilla, then measure the irrep label.

When applied to permutation modules ($\sigma = \mathbf{1}$), measuring the trivial irrep of $Y_T$ projects the multiplicity-space basis from SYTs onto vectors corresponding to SSYTs — the Gelfand-Tsetlin basis for the unitary group.

The correctness relies on the horizontal strip criterion: $\lambda(S_A)|T\rangle = 0$ whenever consecutive integers in $A$ share a column in the SYT $T$.

## When to reach for it
- Decomposing induced representations of finite groups on a quantum computer.
- Any scenario requiring a specific basis for a multiplicity space (not just an arbitrary one).
- Constructing the Gelfand-Tsetlin basis for $U(d)$ irreps via the symmetric group side.

## Complexity
$2 \cdot T_{\mathrm{QFT}(H)} + T_{C_\rho}$, where $T_{C_\rho}$ is the cost of controlled multiplication in the representation. For Young subgroups $Y_T$ of $S_n$: $O(\mathrm{poly}(n, \log(1/\varepsilon)))$.

## Caveat
Requires efficient QFT over $H$ and efficient controlled representation multiplication. For arbitrary groups $H$, these may not be available. Works cleanly for Young subgroups of $S_n$ because they decompose as direct products of smaller symmetric groups.

## Related notes
- [[An Efficient High Dimensional Quantum Schur Transform (Krovi 2019) — Paper Notes]]
- [[QFT over Permutation Modules]]
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes]] — phase estimation is the abelian special case of GPE
