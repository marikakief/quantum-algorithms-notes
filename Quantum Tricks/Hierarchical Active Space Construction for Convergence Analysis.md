
> **Source:** Goings, White, Lee, Tautermann, Degroote, Gidney, Shiozaki, Babbush, Rubin, arXiv:2202.01244
> **Tags:** #trick #quantum-chemistry #active-space #DMRG #convergence #resource-estimation

## What it does

Constructs a nested hierarchy of active spaces (each a strict superset of the previous) designed to track how computed properties converge toward the exact limit as the active space grows, rather than to minimize computational cost at a fixed accuracy.

## The trick

Start with the singly-occupied orbitals (the minimum set for spin-state physics). Expand outward in shells of chemical relevance:

1. **Core metal orbitals:** Remaining 3d, 4s, and nearby antibonding
2. **Axial ligand bonding/antibonding:** Direct metal-ligand bonds
3. **Equatorial bonding:** Metal-nitrogen σ orbitals
4. **Porphyrin π system:** First near-nitrogen, then near-carbon
5. **Distal substituent orbitals:** Cysteine C-S bonds, then heme C-N σ

At each level, add both occupied and virtual orbitals of the same character to maintain a roughly constant filling fraction (~50%). This ensures that correlation effects within each shell are balanced — you don't add bonding orbitals without their antibonding partners.

The key difference from standard active space selection: most approaches seek the *smallest* active space that captures the relevant physics (e.g., automated orbital selection based on natural orbital occupations). This approach instead builds the largest feasible hierarchy and watches convergence. The active space series A ⊂ B ⊂ C ⊂ ... ⊂ X lets you plot any property against active space size and see whether it's converged.

## When to reach for it

- Benchmarking classical vs quantum resource costs on the same system at multiple scales
- Establishing whether an active space calculation has actually converged (rather than just hoping)
- Comparing different classical methods (DMRG, CCSD(T), selected CI) across a controlled set of problem sizes
- Any situation where you need to make a credible quantum advantage argument: the advantage boundary is only meaningful if you can show classical methods failing at a specific scale

## Complexity

The hierarchy itself is a classical orbital analysis step with negligible cost (Pipek-Mezey localization + manual classification). The expense comes from running calculations at each level — but that's the whole point. Each DMRG or CCSD(T) run at level $k$ has its own scaling in the $k$-orbital active space.

## Caveat

The orbital hierarchy is system-specific and requires chemical intuition to construct. The ordering of shells (axial before equatorial, π before σ) reflects physical expectations about which orbitals matter most for the property of interest. A different property (e.g., optical gaps vs spin gaps) might demand a different ordering. Also, maintaining 50% filling is a heuristic — there's no guarantee it yields the best correlation balance at every level.

## Related notes
- [[Reliably Assessing the Electronic Structure of Cytochrome P450 (Goings, Babbush, Rubin et al 2022) — Paper Notes]]
- [[Classical-Quantum Runtime Crossover Analysis]]
- [[Hybrid Active Space Construction]]
