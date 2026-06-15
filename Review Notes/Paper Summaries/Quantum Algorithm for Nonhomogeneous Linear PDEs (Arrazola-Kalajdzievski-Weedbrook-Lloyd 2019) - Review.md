# Review: Quantum Algorithm for Nonhomogeneous Linear PDEs (Arrazola-Kalajdzievski-Weedbrook-Lloyd 2019)

Source note: [[Quantum Algorithm for Nonhomogeneous Linear PDEs (Arrazola-Kalajdzievski-Weedbrook-Lloyd 2019) — Paper Notes]]

## Primary Sources Checked

- Arrazola, Kalajdzievski, Weedbrook, and Lloyd, "Quantum algorithm for nonhomogeneous linear partial differential equations", PRA 2019 / arXiv:1809.02622.

## Verdict

Minor issues. The note gives a clear account of the CV inversion construction and includes the important state-preparation/readout caveats. The biggest risk is that the headline speedup can still be read as stronger than the paper's finite-precision CV model supports.

## Line-Anchored Comments

- Lines 17 and 65: "runtime polynomial in spatial dimension `N`" needs a finite-precision qualifier. In a continuous-variable algorithm, the effective Hilbert-space cutoff, squeezing/energy, homodyne precision, postselection window, and resource-state preparation are part of the computational cost.
- Lines 17 and 21: the "exponential improvement over classical methods" claim should be tied to producing a quantum state and to fixed-degree/high-dimensional structure. It is not an end-to-end exponential improvement for extracting a classical PDE solution.
- Lines 39-43: good description of the resource states. The note should distinguish heuristic neural-network preparation demonstrations from an efficient, proven state-preparation oracle.
- Lines 55-59 and 97-99: this is the right place to connect the postselection/finite-precision parameters to an effective condition-number cutoff. That caveat should be moved up near the headline complexity claim.
- Line 82: typo: `\|b\rangle` should be `|b\rangle`.
- Line 87: Berry-Childs-Ostrander-Wang 2017 is a linear ODE algorithm, not specifically a PDE discretisation paper. If used as a comparison, say "ODE/linear-system history-state approach" rather than "the discrete PDE approach."
- Lines 117-123: the section is titled "References within this paper", but line 123 cites a 2023 paper. That is a later cross-link, not a reference within the 2019 paper.

## Missing or Suggested Additions

- Add a short note on boundary conditions. For PDEs, the distinction between solving a particular inhomogeneous equation and imposing boundary conditions is not a detail.
- State explicitly whether `A` must be Hermitian/self-adjoint or whether the Fourier inversion is being applied to a Hermitian embedding.
