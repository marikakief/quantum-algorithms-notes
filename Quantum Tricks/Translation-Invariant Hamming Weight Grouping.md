
> **Source:** Kivlichan, Gidney, Babbush et al., arXiv:1902.10673, Quantum **4**, 296 (2020)
> **Tags:** #trick #Trotter #fault-tolerant #translation-invariance #T-gate-reduction #circuit-optimization

## What it does

Exploits translation invariance of interaction coefficients in periodic Hamiltonians to group $ZZ$ rotations by displacement vector, so that each group contains $N/2$ identical-angle rotations — enabling [[Hamming Weight Phasing for Equiangular Rotations|Hamming weight phasing]] on the full group.

## The trick

In a translation-invariant system (jellium, Hubbard, periodic materials), the density-density interaction $V_{pq}$ depends only on the displacement $s = p - q$, not on $p$ and $q$ individually. Order the $O(N^2)$ $ZZ$ terms in the split-operator Trotter step by displacement vector $s$. For each $s$, there are $N/2$ pairs — all with the same coefficient $\tilde{V}_s$.

Apply [[Hamming Weight Phasing for Equiangular Rotations|Hamming weight phasing]] to each layer of $N/2$ equiangular rotations. This reduces the total rotation synthesis cost for the potential energy from $O(N^2 \log(1/\varepsilon))$ to $O(N^2 + N \log N \log(1/\varepsilon))$: the $N^2$ Toffoli gates for Hamming weight computation plus only $O(N \log N)$ synthesized rotations across all $O(N)$ displacement groups.

## When to reach for it

- Any periodic-boundary Hamiltonian with translation-invariant two-body interactions
- Split-operator Trotter steps for the [[Plane-Wave Dual Basis|plane-wave dual basis]] electronic structure Hamiltonian
- Hubbard model simulation (though the on-site-only interaction means there's only one displacement group, so the benefit is simpler)

## Complexity

- **Rotation synthesis cost:** $O(N \log N \log(1/\varepsilon))$ instead of $O(N^2 \log(1/\varepsilon))$
- **Toffoli overhead:** $O(N^2)$ for the Hamming weight computations across all groups
- **Net effect:** synthesis cost becomes subdominant; Toffoli cost (which doesn't require synthesis) dominates

## Caveat

Requires exact translation invariance. For systems with defects, boundaries, or non-uniform external potentials, the grouping breaks and you're back to $O(N^2)$ distinct angles. The nuclear potential $U_p$ term is not translation-invariant in molecular calculations (different nuclear charges at different sites), but this is a single-qubit-rotation layer and typically a minor contribution.

## Related notes
- [[Improved Fault-Tolerant Quantum Simulation of Condensed-Phase Correlated Electrons via Trotterization (Kivlichan, Gidney, Babbush et al 2020) — Paper Notes]] — source paper
- [[Hamming Weight Phasing for Equiangular Rotations]] — the underlying rotation reduction technique
- [[Plane-Wave Dual Basis]] — the basis where potential energy is diagonal and this grouping applies
- [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes]] — introduces the split-operator algorithm optimized here
