
> **Source:** Rubin, Berry, Malone, White, Khattar, DePrince, Sicolo, Kühn, Kaicher, Lee, Babbush, arXiv:2302.05531
> **Tags:** #trick #block-encoding #qubitization #QROAM #symmetry #periodic-systems #translational-symmetry

## What it does

Reduces the Toffoli cost of the PREPARE oracle in [[Qubitization (Quantum Walk for Spectral Encoding)|qubitized]] simulation by a factor of $\sqrt{|G|}$, where $|G|$ is the order of an Abelian symmetry group (e.g., the translational group with $|G| = N_k$ k-points). The savings come from feeding only symmetry-unique data into [[QROAM (Space-Time Tradeoff for QROM)|QROAM]].

## The trick

In [[Qubitization (Quantum Walk for Spectral Encoding)|qubitization]] of a Hamiltonian $H = \sum_\ell \omega_\ell U_\ell$, the PREPARE oracle prepares amplitudes $\sqrt{\omega_\ell/\lambda}$ via [[Coherent Alias Sampling for PREPARE|coherent alias sampling]], which requires a [[QROAM (Space-Time Tradeoff for QROM)|QROAM]] call that loads "ind", "alt", and "keep" values for $d$ items of data with $m$ bits per item. The QROAM cost is $O(\sqrt{dm})$.

When the Hamiltonian has an Abelian symmetry (translational symmetry for periodic systems), many LCU coefficients $\omega_\ell$ are related by symmetry. Instead of tabulating all $d$ entries, tabulate only the $d/|G|$ symmetry-unique entries. The symmetry-redundant indices are generated coherently by:

1. Preparing the symmetry label (e.g., crystal momentum $Q$) in superposition
2. Using a few control qubits to swap between symmetry-equivalent index registers
3. Computing the symmetry-derived indices from the unique ones (e.g., $k \ominus Q$ from $k$ and $Q$ via modular subtraction)

The QROAM now loads $d/|G|$ items instead of $d$, with output size unchanged. Since QROAM cost scales as $O(\sqrt{dm})$, this gives a $\sqrt{|G|}$ reduction.

**Concrete example (single factorization for periodic Hamiltonians):** The inner PREPARE loads $O(N_k^2 N^3)$ data in the symmetry-adapted case vs. $O(N_k^3 N^3)$ for the supercell, where the reduction comes from $M = O(N)$ auxiliary index scaling (vs. $O(N_k N)$ for the supercell). The QROAM cost drops from $\tilde{O}(N_k^{3/2} N^{3/2})$ to $\tilde{O}(N_k N^{3/2})$.

The approach works identically for [[Single Factorization for Qubitized Chemistry|single factorization]], sparse, and [[Double Factorization of Two-Electron Integrals|double factorization]] LCUs. For [[THC Non-Orthogonal Diagonal Coulomb Representation|THC]], the savings in QROAM are eventually dominated by the $O(N_k N)$ cost of unary iteration, so no asymptotic speedup survives.

## When to reach for it

- [[Qubitization (Quantum Walk for Spectral Encoding)|Qubitizing]] any Hamiltonian with discrete Abelian symmetry (translational, point group, particle-hole)
- When the QROAM for PREPARE is the cost bottleneck (true for sparse, SF, and DF representations)
- Periodic electronic structure in atom-centered bases (Bloch orbitals)
- Any LCU construction where the data loaded by QROAM has redundancy under a known symmetry group

## Complexity

- Per-step Toffoli savings: factor of $\sqrt{|G|}$ when QROAM dominates
- Ancilla savings: also $\sqrt{|G|}$ (QROAM ancilla count scales with its Toffoli cost)
- Overhead: modular arithmetic on symmetry labels ($O(\log |G|)$ Toffolis), controlled swaps to generate symmetry-equivalent indices ($O(N_k N)$ for periodic systems)

## Caveat

The savings apply only to the QROAM-dominated parts of PREPARE. If SELECT or other primitives (like unary iteration over the full basis) cost $\Omega(N_k N)$, the overall speedup may be less than $\sqrt{|G|}$. This is exactly what happens for THC: the unary iteration floor absorbs the QROAM savings.

For DF and THC, the symmetry-adapted tensor compression has less variational freedom, which can *increase* $\lambda$. Since total Toffoli cost scales as $\lambda \times (\text{cost per step})$, the per-step savings may be canceled by a larger $\lambda$. This tradeoff must be evaluated numerically for each system.

## Related notes
- [[Fault-Tolerant Quantum Simulation of Materials Using Bloch Orbitals (Rubin, Berry, Babbush et al 2023) — Paper Notes]]
- [[QROAM (Space-Time Tradeoff for QROM)]]
- [[Coherent Alias Sampling for PREPARE]]
- [[Qubitization (Quantum Walk for Spectral Encoding)]]
- [[Single Factorization for Qubitized Chemistry]]
- [[Double Factorization of Two-Electron Integrals]]
- [[THC Non-Orthogonal Diagonal Coulomb Representation]]
- [[k-Point THC Factorization (Bloch Orbital THC)]]
