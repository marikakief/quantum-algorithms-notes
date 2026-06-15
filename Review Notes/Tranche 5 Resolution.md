# Tranche 5 Resolution

## Scope

Processed the Tranche 5 feedback covering the Childs/Farhi/Gutmann continuous-time walk sequence, formula-evaluation walk papers, KMO interpolated search, Montanaro backtracking, BCG continuous-query conversion, bounded-degree graph property testing, and the corresponding trick cards.

## Implemented Corrections

- Corrected the CFG 2002 note and transport tricks to distinguish the paper's graph-generator Hamiltonian convention from pure adjacency, and replaced the over-simple Bessel front asymptotic with stationary-phase/Airy-front wording.
- Corrected the 2003 welded-trees note to emphasize the query/oracle traversal task, the column-uniform initial subspace, edge-colouring oracle assumptions, random-labelled lower bound, and EXIT-only task.
- Reworked Childs-Goldstone spatial search notes and tricks to separate raw evolution time from success probability, especially for $d=4$ and $d<4$, and to avoid exact two-dimensional/phase-transition overclaims.
- Scoped Childs 2009 universality to wire-label encoding, exponential graph size, CNOT-as-wire-permutation, $\Theta(\log m)$ filtering, and polynomial overhead from $\Omega(1/m^4)$ success probability.
- Corrected Childs 2010's Perron-vector ratio in $|\psi_j\rangle$, rewrote the lazy-limit section, narrowed the no-fast-forwarding claim, added implementability requirements for the reflections/state preparation, removed the glued-trees sign-problem example, and softened the qubitization/QSVT lineage.
- Updated AND-OR/NAND formula notes: Szegedy theorem provenance, NAND leaf-disconnection convention, zero-energy-to-$\pm i$ phase convention, CCJYM author spelling, FGG runtime $O(\sqrt{N\log N})$, product-formula order scoping, and later Reichardt/span-program context.
- Updated Montanaro backtracking notes and tricks: definitions of $T,n,\delta$, local-diffusion normalization, upper-bound-on-$T$ requirement, hidden child-computation costs, degree dependence, effective-resistance bound, unique-search theorem complexity, and average-case distribution caveats.
- Corrected the KMO interpolation parameter in both the paper note and trick card to $s^*=1-p_M/(1-p_M)$ for $p_M<1/2$, and clarified stationary-weighted $|U\rangle,|M\rangle$, reversibility, $\mathrm{HT}^+$ vs. $\mathrm{HT}$, and 2D-grid notation.
- Scoped BCG 2012 to efficiently implementable driving Hamiltonians and self-inverse query oracles, removed the questionable QIC page range instead of asserting a replacement, and clarified low-Hamming-weight compression.
- Added bounded-degree property-testing parameter caveats and marked the bipartiteness lower-bound gap as "left open in this paper" rather than making an unverified current-literature claim.
- Updated walk trick cards for equitable column partitions, swap-conjugation clean subspace, graph-scattering phases/effective lengths, momentum filtering, formula Szegedy walks, interpolated/incremental eigenvalue estimation, and the discriminant/overlap spectral theorem.

## Feedback Not Directly Implemented

- BCG QIC pagination: the note's page range was removed rather than replaced with a specific range, because the local review identified the existing range as suspect but I did not independently verify the journal pagination during this pass.
- Property-testing current lower-bound status: I did not claim the bipartiteness lower-bound gap remains open today; the note now states only what was left open by Ambainis-Childs-Liu.
- Several "universal" statements were not merely reworded but narrowed, because the supervisor feedback correctly identified them as overbroad rather than wrong in every special case.

## Verification

- Targeted residual searches found no remaining occurrences of the main bad strings: the incorrect KMO square-root $s^*$, `Childs-Cleve-Jordan-Yeung`, `O(\sqrt{N} \log N)` for FGG, `log\Theta`, the broad Hamiltonian-oracle conversion statement, the random-cycle-breaks-column-symmetry caveat, `||M||`, the glued-trees sign-problem example, and the spurious $O(\sqrt{Tn}/\delta^3)$ unique-search bound.
