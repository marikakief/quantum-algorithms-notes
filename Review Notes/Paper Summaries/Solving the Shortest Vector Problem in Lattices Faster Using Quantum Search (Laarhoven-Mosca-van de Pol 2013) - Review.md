# Review: Solving the Shortest Vector Problem in Lattices Faster Using Quantum Search (Laarhoven-Mosca-van de Pol 2013)

Source note: [[Solving the Shortest Vector Problem in Lattices Faster Using Quantum Search (Laarhoven-Mosca-van de Pol 2013) — Paper Notes]]

## Primary Sources Checked

- Laarhoven, Mosca, and van de Pol, "Solving the Shortest Vector Problem in Lattices Faster Using Quantum Search", arXiv:1301.6176 / PQCrypto 2013.

## Verdict

Minor issues. The note is accurate and appropriately skeptical about QRACM. The biggest risk is that readers may overgeneralize the Groverized-list-search idea to enumeration or to lower bounds for lattice cryptography.

## Line-Anchored Comments

- Lines 11-25: good problem statement and cost model. The QRACM assumption is introduced early, which is important.
- Lines 29-37: excellent high-level assessment. This is not a hidden-subgroup lattice break; it is a constant-exponent update for known SVP attacks.
- Lines 43-50: the shared subroutine is the right abstraction. Emphasize that the predicate evaluation must be coherently queryable over an indexed list.
- Lines 53-83: the Nguyen-Vidick sieve discussion is clear. The heuristic uniformity assumption must remain attached to the `2^0.312n` exponent.
- Lines 88-94: good summary of the two-level sieve; same leading asymptotic does not mean same constants.
- Lines 96-135: the Pujol-Stehle saturation stages are well handled. The dummy list `T` is a proof device, and the note says that correctly.
- Lines 160-173: the enumeration/Voronoi caveat is valuable and should stay. It prevents the common but wrong "just Grover every exponential algorithm" conclusion.
- Lines 179-198: the theorem statements are useful. Keep provable and heuristic rows separate.
- Lines 219-223: good QRACM caveat. This is the main architectural assumption behind the heuristic speedups.

## Missing or Suggested Additions

- Add a one-line distinction between QRACM and full coherent quantum RAM: the paper's model still assumes fast coherent indexed access to exponentially large classical lists.
- When using this for post-quantum parameter discussions, explicitly state whether the attack cost is time, space, or area-time; the paper mostly reports time and space exponents.

