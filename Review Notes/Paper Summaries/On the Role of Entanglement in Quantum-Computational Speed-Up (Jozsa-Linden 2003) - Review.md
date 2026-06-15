# Review: On the Role of Entanglement in Quantum-Computational Speed-Up (Jozsa-Linden 2003)

Source note: [[On the Role of Entanglement in Quantum-Computational Speed-Up (Jozsa-Linden 2003) — Paper Notes]]

## Verdict

Minor-to-major issues. The note is thoughtful and captures the pure-state theorem and mixed-state caveat. The main technical problem is the claim that the simulation remains polynomial for `p = O(log n)`; the theorem as presented in the paper assumes fixed `p`, and the brute-force repartitioning step has a worse dependence on `p` than the note acknowledges.

## Line-Anchored Comments

- Lines 7-15: The headline should say "bounded multipartite entanglement in the `p`-blocked sense," not just "bounded entanglement." MPS-bounded Schmidt rank and stabilizer structure are different notions.

- Lines 17-29: "`p`-blocked iff no more than `p` qubits are mutually entangled" is acceptable intuition for pure states, but not a precise entanglement measure. Keep the partition definition primary.

- Lines 35-45: The exact simulation proof depends on finding/recovering a valid small-block tensor decomposition after gates. For variable `p`, the cost of checking all partitions is not just `2^p`.

- Lines 47-61: The approximate theorem is correctly summarized, including the exponential-in-`T` tolerance. Stress that this bound is far too small to be a practical entanglement threshold.

- Lines 76-86: The Shor arithmetic-progression discussion is useful but should be stated cautiously: it shows unbounded entanglement for generic periods/instances, not a full independent proof of Shor speedup.

- Lines 88-100: The mixed-state caveat is excellent and should remain prominent. It is the part most often lost in informal retellings.

- Lines 114-121: The claim that `p = O(log n)` still gives an efficient simulation is not justified by the proof sketch. The state vector size is polynomial, but repartition search can be quasi-polynomial for `p = Theta(log n)`. Either restrict to fixed `p` as in the theorem, or carefully state the extra assumptions needed for a growing-block simulation.

## Missing or Suggested Additions

- Add a comparison of `p`-blocked simulation versus Vidal/MPS simulation: block size versus Schmidt rank.
- Add a warning that entanglement is necessary but not sufficient for pure-state computational speedup.
