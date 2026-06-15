# Review: Solving Strongly Correlated Electron Models on a Quantum Computer (Wecker-Hastings-Wiebe-Clark-Nayak-Troyer 2015)

Source note: [[Solving Strongly Correlated Electron Models on a Quantum Computer (Wecker, Hastings, Wiebe, Clark, Nayak, Troyer 2015) — Paper Notes]]

## Primary Sources Checked

- Wecker, Hastings, Wiebe, Clark, Nayak, Troyer, "Solving strongly correlated electron models on a quantum computer," PRA 92, 062318 (2015) / arXiv:1506.05135.

## Verdict

Sound with minor caveats. This is a very thorough note and captures the paper's value as an end-to-end Hubbard simulation pipeline rather than a single algorithmic primitive.

## Line-Anchored Comments

- Lines 21-35: the six-part contribution list is accurate and useful. The final wall-clock estimate should be framed as a 2015 logical-gate extrapolation under optimistic fault-tolerant assumptions.
- Lines 43-50: "phase-diagram-by-elimination" is a good extracted idea, but it relies on a candidate-phase list and polynomially small adiabatic gaps. Keep those assumptions adjacent to the idea.
- Lines 77-85: the `O(N)` gates and `O(log N)` depth per Trotter step are model- and layout-specific. Make clear this is the Hubbard/lattice-fermion structure, not a generic molecular Hamiltonian result.
- Lines 109-111: the non-destructive measurement plus amplitude-estimation speedup is subtle. Specify the required ability to project/measure the ground-state subspace via QPE.
- Lines 143-151: good inclusion of boundary cancellation and the time-dependent Trotter appendix. The relation to later rigorous Trotter-error work should be marked as historical context.
- Lines 250-260: the assessment is strong but a bit editorial. It is fine for review notes, but if this source note becomes a public summary, move the assessment to a clearly labeled commentary section.

## Missing or Suggested Additions

- Add a small "assumption stack" box: fault-tolerant logical gates, polynomial adiabatic gap, physically motivated candidate phases, and efficient lattice-specific Trotter circuits.
