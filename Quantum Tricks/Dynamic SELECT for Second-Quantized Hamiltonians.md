# Dynamic SELECT for Second-Quantized Hamiltonians

> **Source:** Babbush, Berry, Kivlichan, Wei, Love, Aspuru-Guzik, arXiv:1506.01020
> **Tags:** #trick #LCU #quantum-chemistry #select-oracle #fermionic

## What it does

Implements the SELECT oracle for a second-quantized chemistry Hamiltonian in $O(N)$ gates by constructing each Pauli string dynamically from the orbital-index register, rather than using a lookup table over all $\Gamma = O(N^4)$ Hamiltonian terms.

## The trick

After [[Jordan-Wigner Transformation for Chemistry Hamiltonians|Jordan-Wigner mapping]], each fermionic term $a_p^\dagger a_q^\dagger a_r a_s$ becomes a Pauli string with a specific structure:

- Pauli $X$ or $Y$ operators on qubits $p, q, r, s$ (the orbital indices)
- Pauli $Z$ strings between paired indices (the Jordan-Wigner parity strings)
- Identity elsewhere

Rather than hard-coding a separate controlled unitary for each of the $O(N^4)$ terms, the SELECT oracle reads the orbital indices $(p, q, r, s)$ from the ancilla register and applies the corresponding Pauli operators dynamically:

1. For each orbital index in the ancilla, apply a controlled-$\sigma_+$ or $\sigma_-$ on the target qubit, with the control being the index register.
2. Apply the parity $Z$-string between the relevant qubits, controlled on the index values.

This uses standard controlled operations and multi-qubit addressing — the point is that the structure of Jordan-Wigner strings is regular enough that you don't need a separate circuit branch for each term.

## When to reach for it

- Any [[Linear Combination of Unitaries (LCU)|LCU]]-based simulation of a second-quantized Hamiltonian where the number of terms $\Gamma$ is large but each term has structured Pauli-string form.
- Generalises to any Hamiltonian where the SELECT oracle can exploit regularity in the Pauli decomposition rather than brute-forcing over all terms.

## Complexity

$O(N)$ gates per SELECT call (dominated by the length of the Jordan-Wigner parity string). With [[Jordan-Wigner Transformation for Chemistry Hamiltonians|Bravyi-Kitaev]] mapping the parity string length drops to $O(\log N)$, potentially reducing SELECT cost further.

## Caveat

The $O(N)$ cost comes from the Jordan-Wigner $Z$-string length. For very large $N$ this can still be significant. The Bravyi-Kitaev transform reduces this to $O(\log N)$ in principle, but with a more complex addressing scheme.

## Related notes

- [[Exponentially More Precise Quantum Simulation of Fermions in Second Quantization (Babbush-Berry-Kivlichan-Wei-Love-Aspuru-Guzik 2015) — Paper Notes]]
- [[Jordan-Wigner Transformation for Chemistry Hamiltonians]]
- [[Linear Combination of Unitaries (LCU)]]
- [[Sublinear-T Block-Encodings for Second-Quantized Hamiltonians (arXiv 2510.08644) — Paper Notes]]
