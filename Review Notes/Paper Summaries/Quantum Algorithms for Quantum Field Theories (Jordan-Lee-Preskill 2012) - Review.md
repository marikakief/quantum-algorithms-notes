# Review: Quantum Algorithms for Quantum Field Theories (Jordan-Lee-Preskill 2012)

Source note: [[Quantum Algorithms for Quantum Field Theories (Jordan-Lee-Preskill 2012) — Paper Notes]]

## Primary Sources Checked

- arXiv: https://arxiv.org/abs/1111.3633
- Science DOI linked in the source note.

## Verdict

Minor issues. The note is a strong high-level summary of the algorithmic pipeline. The main corrections are to temper speedup language, keep spacetime versus spatial dimension notation clear, and distinguish EFT power counting from a fully rigorous computer-science error theorem.

## Line-Anchored Comments

- Lines 11-15: "Exponential speedup over classical methods" should be phrased as a comparison to known classical methods for strong-coupling/high-precision scattering, not as an unconditional classical lower bound.

- Lines 17-20: The three highlighted techniques are well chosen. Add that the paper studies scalar `phi^4` theory first; later work extends to gauge theories and fermions.

- Lines 28-35: The field-discretization and Gaussian-vacuum preparation discussion is good. Keep the distinction between quantum gate cost and classical preprocessing cost for the covariance factorization; the note currently blends them by calling it "gates."

- Lines 42-51: The adiabatic wavepacket phase-cancellation description is useful. It would be safer to say the construction cancels the dominant dynamical phases in the adiabatic limit, not that phases simply cancel for finite discretization.

- Lines 55-56: The locality-improved Trotter statement should carry the assumptions: spatially local lattice Hamiltonian, bounded local terms after field cutoff, fixed local dimension from discretization parameters.

- Lines 67-73: The exponent table is too terse to be source-auditable. State which dimension convention is used: the paper often uses spacetime dimension `D` and spatial dimension `d = D-1`.

- Lines 77-90: The note correctly calls the EFT argument "physics-style." Keep that caveat near the discretization-error headline too; otherwise `epsilon = O(a^2)` may look like a rigorous black-box numerical-analysis theorem with explicit constants.

- Lines 94-98: "Completed Feynman's programme" is rhetorically strong. Better: it was the first polynomial-time quantum algorithmic treatment of scattering in an interacting continuum QFT model.

## Missing or Suggested Additions

- Add a one-line output-task statement: the algorithm samples scattering outcomes/momentum occupation numbers, not the full classical wavefunction.

- Add a cross-link to the 2018 BQP-completeness scattering note as a complement: JLP 2012 gives an efficient quantum algorithm; JKLP 2018 gives hardness for a related QFT scattering task.

