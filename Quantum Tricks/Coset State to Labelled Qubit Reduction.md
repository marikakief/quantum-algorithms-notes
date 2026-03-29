# Coset State to Labelled Qubit Reduction

> **Source:** Ettinger-Høyer (1999); used extensively in Kuperberg (2005), arXiv:quant-ph/0302112
> **Tags:** #trick #hidden-subgroup-problem #dihedral-group #representation-theory

## What it does
Reduces the dihedral hidden subgroup problem from a mixed state over coset states to a collection of individually labelled single-qubit states, making the problem amenable to sieve-based attacks.

## The trick
For $G = D_N$ with hidden reflection $H = \langle yx^s \rangle$:

1. Query the oracle with the uniform superposition $|D_N\rangle$ and discard the output, obtaining $\rho_{D_N/H}$
2. Decompose the rotation register using the [[Quantum Fourier Transform Circuit|QFT]] on $\mathbb{Z}/N$: this is (almost) the character transform for $D_N$
3. Measure the Fourier label $k \in \mathbb{Z}/N$

The result: a known random label $k$ and a single qubit in state
$$|\psi_k\rangle \propto |0\rangle + e^{2\pi i ks/N}|1\rangle$$

This works because $D_N$ has 2-dimensional irreducible representations $V_k$ (for most $k$), and the coset state $|Ha\rangle$ projected onto $V_k$ yields an $H$-invariant state in a 2-dimensional space — which is a qubit. The phase encodes the hidden slope $s$.

The representation-theoretic perspective (Proposition 8.1 in Kuperberg): after the character measurement, you get irrep $V$ with probability $\propto (\dim V)(\dim V^H)|H|/|G|$ and the uniform state on $V^H$. For $D_N$ reflections, $\dim V^H = 1$ for all $V_k$, so each call yields exactly one qubit.

Note that $|\psi_{-k}\rangle = X|\psi_k\rangle$ (bit flip), so $k$ and $-k$ carry equivalent information.

## When to reach for it
- Any hidden subgroup problem on a group with low-dimensional representations (dihedral, generalised dihedral, semidirect products with small complement)
- When the character transform is efficiently computable and the target representations are qubits or small systems
- As the first step before applying a [[Quantum Sieve for Labelled Qubits|quantum sieve]]

## Complexity
One oracle call + one QFT on $\mathbb{Z}/N$ (which is $O(\log^2 N)$ gates) per labelled qubit.

## Caveat
- For the dihedral group, a single labelled qubit carries very little information about $s$ — the measurement in any basis is a cosine-biased coin flip. Exponentially many qubits are needed *if processed one at a time*. The power comes from processing them collectively via the sieve.
- This reduction does not help for groups with large irreps (e.g., the symmetric group $S_n$, where typical irreps have dimension $\gg 1$). The labelled qubit picture breaks down — you get high-dimensional states instead.
- The row space $V^*$ in the Burnside decomposition carries no useful information (it's always the uniform state). This is a known obstruction for nonabelian HSP in general.

## Related notes
- [[A Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2005) — Paper Notes]]
- [[A Subexponential Time Algorithm for the Dihedral Hidden Subgroup Problem with Polynomial Space (Regev 2004) — Paper Notes]]
- [[Quantum Sieve for Labelled Qubits]]
- [[Coset Sampling via Fourier Transform]]
- [[Entangled Multi-Copy Measurements for Nonabelian HSP]]
- [[Pretty Good Measurement for State Discrimination]]
- [[From Optimal Measurement to Efficient Quantum Algorithms for the HSP over Semidirect Product Groups (Bacon-Childs-van Dam 2005) — Paper Notes]]
