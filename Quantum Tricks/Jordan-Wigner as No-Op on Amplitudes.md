# Jordan-Wigner as No-Op on Amplitudes

> **Source:** Verstraete, Cirac, Latorre, arXiv:0804.1888
> **Tags:** #trick #Jordan-Wigner #fermions #encoding #quantum-circuits

## What it does
The [[Jordan-Wigner Transformation for Chemistry Hamiltonians|Jordan-Wigner transformation]] between spin and fermionic representations acts only on the *labelling* of basis states — it does not change the state amplitudes. In circuit terms, it costs zero gates.

## The trick

The Jordan-Wigner map $c_i = \prod_{m<i} \sigma_m^z \cdot (\sigma_i^x - i\sigma_i^y)/2$ transforms a spin state

$$|\psi\rangle = \sum_{i_1, \ldots, i_n} \psi_{i_1 \cdots i_n} |i_1, i_2, \ldots, i_n\rangle$$

into the fermionic state

$$|\psi\rangle = \sum_{i_1, \ldots, i_n} \psi_{i_1 \cdots i_n} (c_1^\dagger)^{i_1} (c_2^\dagger)^{i_2} \cdots (c_n^\dagger)^{i_n} |\Omega\rangle$$

The coefficients $\psi_{i_1 \cdots i_n}$ are identical. No unitary operation on the register is needed — the transformation is a reinterpretation of what the computational basis states mean.

The price: from this point on, any exchange of mode labels must carry a fermionic sign ($-1$ for swapping two occupied modes). This is handled by [[Fermionic SWAP for Anticommutation Enforcement|fermionic SWAP gates]] in subsequent circuit operations.

## When to reach for it

- Starting point for any fermionic simulation that uses Jordan-Wigner encoding — the encoding itself is free
- Simplifying gate counts: don't budget gates for the Jordan-Wigner map, only for the subsequent operations that respect fermionic statistics
- Pedagogical clarity: separating the "encoding overhead" (zero) from the "locality overhead" (the string operators in JW, which manifest as fermionic SWAPs)

## Complexity

Zero gates. The transformation is purely notational.

## Caveat

The Jordan-Wigner transformation *does* change the locality structure of operators. A local spin operator $\sigma_i^x \sigma_{i+1}^x$ becomes a local fermionic operator $c_i^\dagger c_{i+1} + \text{h.c.}$, but a periodic boundary term $\sigma_n^x \sigma_1^x$ maps to a string operator spanning the whole chain. The "no-op" statement is about the *state*, not the *Hamiltonian* structure.

## Related notes
- [[Quantum Circuits for Strongly Correlated Quantum Systems (Verstraete-Cirac-Latorre 2009) — Paper Notes]]
- [[Jordan-Wigner Transformation for Chemistry Hamiltonians]]
- [[Fermionic SWAP for Anticommutation Enforcement]]
- [[Fermionic Swap Network]]
