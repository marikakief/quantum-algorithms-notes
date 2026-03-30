# QFT over Permutation Modules

> **Source:** Hari Krovi, arXiv:1804.00055
> **Tags:** #trick #representation-theory #Fourier-transform #symmetric-group #induced-representation

## What it does
Block-diagonalises a permutation module of the symmetric group $S_n$ — an induced representation from the trivial representation of a Young subgroup $Y_T$ — into irreducible representations of $S_n$.

## The trick
A permutation module $P(T) = \uparrow^{S_n}_{Y_T} \mathbf{1}$ has computational basis $\{|t\rangle\}$ indexed by coset representatives (transversal elements). The algorithm:

1. Append ancilla register of $\lceil \log |Y_T| \rceil$ qubits, prepare equal superposition over elements of $Y_T$ (via inverse QFT over $Y_T$).
2. Perform **generalised phase estimation** on the right regular representation $R$ of $S_n$, controlled by $Y_T$. This decomposes the multiplicity space: SYT basis vectors are projected to $\lambda(Y_T)|T_1\rangle$, which are non-zero iff the SYT corresponds to a valid SSYT (horizontal strip condition).
3. Apply the QFT over $S_n$ (Beals' algorithm) to complete the block diagonalisation into $|\lambda, i, j\rangle$.

The GPE step ensures the multiplicity space basis aligns with SSYTs, not arbitrary SYTs. The key technical lemma: if consecutive integers in a set $A$ share a column in a SYT $T$, then $\lambda(S_A)|T\rangle = 0$.

## When to reach for it
- Any problem requiring the Schur transform on high-dimensional systems ($d \gg n$).
- Problems with $S_n$-permutational symmetry where you need to decompose an induced representation.
- Potential applications in quantum algorithms for element distinctness or collision finding, where permutation-module structure appears naturally.

## Complexity
$O(\mathrm{poly}(n, \log(1/\varepsilon)))$ elementary gates. Independent of the ambient dimension $d$.

## Caveat
Works specifically for induced representations from the trivial rep of Young subgroups. More general induced representations would need adapted GPE steps. The polynomial in $n$ is not tightly optimised.

## Related notes
- [[An Efficient High Dimensional Quantum Schur Transform (Krovi 2019) — Paper Notes]]
- [[Alphabet Reduction to n for Schur Transform]]
- [[GPE for Multiplicity Space Decomposition]]
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes]] — phase estimation underpins GPE
