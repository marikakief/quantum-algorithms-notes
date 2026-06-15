# Review: Universal Quantum Hamiltonians (Cubitt-Montanaro-Piddock 2018)

Source note: [[Universal Quantum Hamiltonians (Cubitt-Montanaro-Piddock 2018) — Paper Notes]]

## Primary Sources Checked

- Cubitt, Montanaro, and Piddock, "Universal quantum Hamiltonians," Proceedings of the National Academy of Sciences 115(38), 9497-9502 (2018) / arXiv:1701.05182.

## Verdict

Minor issues. This is a very good note. The main needed qualification is that "simulate all physics" always means finite-dimensional/truncated targets, low-energy encodings, cutoffs, and controlled approximation parameters.

## Line-Anchored Comments

- Lines 7-23: the computational problem is framed well. Add "below an energy cutoff and under an encoding" to the headline list of reproduced physics.
- Lines 27-31: the universality claim is correct but should mention variable couplings and large energy-scale separations immediately. Otherwise "2D Heisenberg is universal" sounds more experimentally direct than the theorem.
- Lines 39-53: excellent description of the encoding theorem. It is the right centerpiece of the note.
- Lines 55-62: the approximate simulation definition is summarized cleanly. Specify that `eta` controls closeness of the encoded subspace/isometry while `epsilon` controls effective Hamiltonian error.
- Lines 64-78: good discussion of symmetry-breaking encodings. Add that the generated logical interactions emerge perturbatively, so the construction inherits gadget overheads.
- Lines 88-104: the complex-to-real and fermion/boson extensions are correct at a high level. For bosons, explicitly say finite truncation is part of the input model.
- Lines 143-151: the preservation guarantees are important. "Local noise" preservation should be stated as a mapping under the local encoding up to controlled errors, not as a laboratory noise-threshold statement.
- Lines 170-176: caveats are excellent and should remain.

## Missing or Suggested Additions

- Add a "not a lab recipe" sentence near the top: universality is mathematically powerful, but perturbative gadgets, variable couplings, and large energy scales dominate practical implementation.

