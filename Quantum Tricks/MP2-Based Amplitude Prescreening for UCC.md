# MP2-Based Amplitude Prescreening for UCC

> **Source:** Romero, Babbush, McClean, Hempel, Love, Aspuru-Guzik, arXiv:1701.02691 (2018)
> **Tags:** #trick #VQE #UCC #quantum-chemistry #parameter-reduction #MP2

## What it does

Reduces the number of variational parameters in a [[Unitary Coupled Cluster (UCC) Ansatz|UCCSD]] ansatz by discarding excitation operators whose second-order Møller-Plesset (MP2) amplitude is below a threshold. Cuts parameter count roughly in half with sub-chemical-accuracy error for weakly to moderately correlated systems.

## The trick

UCCSD has $O(N^2\eta^2)$ variational parameters — most are small for typical molecules. Estimate their magnitude cheaply using the MP2 approximation (a single classical computation):

$$t^{ab}_{ij}{}^{\!\text{MP2}} = \frac{h_{ijba} - h_{ijab}}{\epsilon_i + \epsilon_j - \epsilon_a - \epsilon_b}$$

where $\epsilon_p$ are Hartree-Fock orbital energies and $h_{ijba}$ are two-electron integrals. Singles vanish: $t^a_i{}^{\!\text{MP2}} = 0$ by Brillouin's theorem.

**Prescreening rule:** Include excitation operator $\tau_j$ in the ansatz only if $|t_j^\text{MP2}| \geq d$. Discard the rest — set those parameters to zero and remove their circuit blocks.

The threshold $d$ trades accuracy for circuit depth:
- $d = 10^{-3}$: retains $\sim 50\%$ of parameters; PES deviation $< 7\times10^{-4}$ kcal/mol (well inside chemical accuracy)
- $d = 10^{-2}$: retains $\sim 46\%$; PES deviation $\leq 0.07$ kcal/mol for typical geometries (borderline, path-dependent)

MP2 amplitudes also provide a good **initialization** for the remaining parameters — start the classical optimizer at $\vec{t}_0 = \vec{t}^\text{MP2}$ rather than zero. This reduces the number of optimizer iterations significantly.

## When to reach for it

- Any time you're implementing [[Variational Quantum Eigensolver (VQE)|VQE]] with a UCCSD ansatz on limited hardware and want to reduce circuit depth without redesigning the ansatz.
- For molecules where MP2 gives a reasonable approximation (weak to moderate correlation). Works well for molecules near equilibrium geometry.
- As a first pass before deciding whether a full active-space restriction ([[Active Space Restriction for VQE (CAS-UCC)]]) is needed.

## Complexity

- **MP2 cost:** $O(N^5)$ classically — cheap compared to the quantum circuit overhead. Run once, use the amplitudes for the entire VQE optimization.
- **Parameter reduction:** from $O(N^2\eta^2)$ full UCCSD to $\sim O(N^2\eta^2/2)$ at $d = 10^{-3}$ in practice. Asymptotic scaling unchanged; constant factor reduced.
- **Circuit depth reduction:** proportional to parameter reduction (each excitation contributes independently in the Trotterized product form).

## Caveat

- **Strongly correlated systems:** MP2 fails for near-degenerate ground states (e.g., bond breaking, transition metal complexes). Discarding amplitudes based on an unreliable MP2 estimate can remove important excitations. Use $d = 0$ (no pruning) or switch to a multireference reference (DMRG-based natural orbitals) to select active spaces instead.
- **Not guaranteed:** The error from pruning is not bounded analytically. The $7\times10^{-4}$ kcal/mol figure is empirical for H4. For other systems or larger thresholds, errors could exceed chemical accuracy.
- **Geometry dependence:** At stretched geometries (bond breaking), strong correlation grows and MP2 amplitudes become unreliable, so pruning errors increase. Paper shows that the "linear" path in H4 (which passes through stretched geometries) has larger errors at $d = 10^{-2}$ than the trapezoidal or parallel paths.

## Related notes
- [[Strategies for Quantum Computing Molecular Energies Using the UCC Ansatz (Romero, Babbush et al 2018) — Paper Notes]]
- [[Unitary Coupled Cluster (UCC) Ansatz]]
- [[Variational Quantum Eigensolver (VQE)]]
- [[Active Space Restriction for VQE (CAS-UCC)]]
