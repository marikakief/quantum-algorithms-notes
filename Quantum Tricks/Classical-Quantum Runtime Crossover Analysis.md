# Classical-Quantum Runtime Crossover Analysis

> **Source:** Goings, White, Lee, Tautermann, Degroote, Gidney, Shiozaki, Babbush, Rubin, arXiv:2202.01244
> **Tags:** #trick #quantum-advantage #resource-estimation #DMRG #qubitization #surface-code #fault-tolerant

## What it does

Establishes an empirical quantum advantage boundary by plotting classical runtime (DMRG CPU hours at varying bond dimension) and quantum runtime (QPE hours at varying factory count) against the same axis — active space size — for the same set of Hamiltonians.

## The trick

For each active space in a [[Hierarchical Active Space Construction for Convergence Analysis|hierarchy]]:

1. Run DMRG at multiple bond dimensions $M$. Record wall time × threads = CPU hours.
2. Compute [[Qubitization (Quantum Walk for Spectral Encoding)|qubitized]] phase estimation resources (Toffoli count, logical qubits) using the active space Hamiltonian.
3. Compile to surface code: fix the number of Toffoli factories, physical error rate, and cycle time. Convert Toffoli count to QPU hours.
4. Plot both on the same log-log plot.

The crossover point — where QPU time drops below DMRG time at a given bond dimension — is the advantage boundary. Critically, you must specify the bond dimension: DMRG at $M=500$ is fast but inaccurate, while $M=3000$ is slow but (possibly) converged. The advantage boundary shifts depending on what accuracy you demand.

To make the comparison fair:
- DMRG time is parallelizable (more cores); QPU time is parallelizable (more factories). Compare at equivalent resource levels (e.g., 32 threads vs 6 factories).
- Plot the DMRG energy error (relative to extrapolated DMRG) against bond dimension. QPE error is $\varepsilon$ (the target precision). Ensure you're comparing methods at comparable accuracy.

For CYP models: the crossover occurs at ~31 orbitals for $M \geq 1000$ with 2 Toffoli factories (0.1% error rate).

## When to reach for it

- Making quantum advantage claims for specific chemical systems (not generic "exponential speedup" arguments)
- Resource estimation papers that compare quantum costs to *specific* classical baselines
- Evaluating whether a particular quantum hardware roadmap is on track for a chemistry milestone
- Anytime you need to answer: "at what system size does the quantum computer actually win?"

## Complexity

The classical side: DMRG scales as $O(k^3 M^3)$ for $k$ orbitals at bond dimension $M$. The quantum side: THC [[Qubitization (Quantum Walk for Spectral Encoding)|qubitization]] scales empirically as $O(N^{2.5})$ Toffolis for CYP-like systems, with surface code overhead depending on code distance and factory count.

## Caveat

The comparison is between DMRG and QPE for the *specific task* of ground-state energy estimation in an active space. It does not account for:
- QPE's inability to directly produce reduced density matrices (needed for NEVPT2 corrections)
- Other classical methods (AFQMC, selected CI) that may have different scaling profiles
- The initial state overlap cost, which adds a $\mathrm{poly}(1/S)$ multiplicative factor to QPE runtime
- Practical overheads: DMRG is a mature software stack; QPE on surface codes has never been run at scale

The crossover point is system-dependent. FeMoCo-like systems with more strongly correlated orbitals may cross over at different sizes than CYP's moderately multireference Cpd I.

## Related notes
- [[Reliably Assessing the Electronic Structure of Cytochrome P450 (Goings, Babbush, Rubin et al 2022) — Paper Notes]]
- [[Hierarchical Active Space Construction for Convergence Analysis]]
- [[EQA Assessment Framework (State Preparation vs Classical Heuristic)]]
- [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes]]
- [[Is There Evidence for Exponential Quantum Advantage in Quantum Chemistry (Lee, Babbush, Chan et al 2022) — Paper Notes]]
