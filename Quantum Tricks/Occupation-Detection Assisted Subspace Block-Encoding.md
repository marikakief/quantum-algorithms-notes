
> **Tags:** #trick #occupation-oracles #subspace #qsvt
> **Source:** arXiv:2510.08644

## What it does
Builds block-encodings that directly target physically relevant particle-number sectors, reducing normalization overhead.

## The trick
Use occupation-detection oracles plus helper copy/uncompute structure to restrict [[Standard-Form Encoding (Prepare + Signal Oracle)|block-encoding]] operations to valid occupied-index transitions.

## When to reach for it
- Electronic-structure style Hamiltonians with fixed $\eta$

## Complexity
Reduces subnormalization factor $\alpha$ from $O(L)$ (full-space) to $O(\sqrt{L})$ for the $\eta$-particle sector, where $L$ = number of interaction terms. For a molecular Hamiltonian with $n$ spin orbitals and $L \sim n^4$, this is a reduction from $O(n^4)$ to $O(n^2\eta^2)$. Since simulation cost scales linearly with $\alpha$, this directly lowers gate overhead.

## Caveat
The occupation oracle $O_\text{occ}$ adds non-trivial circuit complexity — it maps rank index $i \in [\eta]$ to the $i$-th occupied spin-orbital. Implementation via comparators over the $n$-qubit occupation register adds $O(n)$ Clifford and $O(\log n)$ T gate overhead per call. Ancilla overhead also increases vs the full-space construction.

## Related Paper Notes
- [[Sublinear-T Block-Encodings for Second-Quantized Hamiltonians (arXiv 2510.08644) — Paper Notes]]
