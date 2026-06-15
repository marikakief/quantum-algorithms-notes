# Review: Single-Ancilla Ground State Preparation via Lindbladians (Ding-Chen-Lin 2023)

Source note: [[Single-Ancilla Ground State Preparation via Lindbladians (Ding-Chen-Lin 2023) — Paper Notes]]

## Primary Sources Checked

- Ding, Chen, and Lin, "Single-ancilla ground state preparation via Lindbladians," Phys. Rev. Research 6, 033147 (2024), arXiv:2308.15676.

## Verdict

Major issue in the cost discussion; otherwise a strong summary. The construction, one-ancilla dilation, and mixing-time caveats are well captured, but the assessment paragraph inserts an exponential cost that contradicts the theorem statements later in the same note.

## Line-Anchored Comments

- Lines 11-15: "zero overlap" needs a qualifier. The algorithm does not require initial amplitude on the ground state in the QPE/QSVT sense, but success still requires the Lindbladian to connect the initial support to the ground state and mix under the chosen coupling operator.
- Line 23: "detailed balance" is not quite the right phrase for the zero-temperature cooling filter as written. The filter suppresses upward transitions and makes the ground state dark; detailed balance should be reserved for a thermal reversible process unless the paper explicitly formulates it that way.
- Lines 27-29: good summary of the three technical contributions. Keep this structure.
- Line 31: the displayed exponential scalings `tilde O(e^{Theta(T^2/epsilon)})` and `tilde O(e^{Theta(T^{1+1/p}/epsilon^{1/p})})` appear wrong and conflict with lines 103, 113, and 119, which give polynomial dependence on `T` and `1/epsilon` up to polylog factors. This is the most important correction.
- Lines 31 and 146: once the exponential typo is removed, the comparison to QSVT/QET-U should be reframed. The Lindbladian method is not asymptotically worse because of an `e^{...}` simulation overhead; its unknown is the mixing time `T = t_mix` and the strength of the ETH/cascade assumptions.
- Lines 45-51: the sign convention for the jump operator is clearly explained. It would help to say explicitly that transitions from an eigenstate `j` to `i` are weighted by `hat f(lambda_i - lambda_j)`.
- Lines 101-121: these theorem statements are the resource summary the note should rely on. Include the missing dependence on `Delta`, `||H||`, and implementation of `A` when comparing to other preparation algorithms.
- Line 121: taking `p -> infinity` is an asymptotic statement about the formula, not a free practical limit. High-order product formulas have constants and implementation overheads.
- Lines 137-144: the comparison table needs cleanup. Lin-Tong 2020 requires block-encoding ancillas `m` plus additional ancillas, not merely `O(log(1/epsilon))`; QET-U's short-depth ground-state preparation scaling should not be mixed with its energy-estimation `epsilon^-1` scaling.
- Lines 140 and 146: "no initial-state overlap requirement" should be stated as "no direct overlap parameter appears; convergence is controlled by the Lindbladian mixing/connectivity assumptions."
- Line 143: "no multi-qubit control" should be scoped to the primitive circuit model using Hamiltonian simulation and simple controlled-`A` operations. Compiling the Hamiltonian simulation can still be costly.

## Missing or Suggested Additions

- Add an access-model box: available Hamiltonian simulation `e^{±iHt}`, implementable local/simple coupling `A`, mid-circuit measurement/reset, a spectral gap lower bound for the filter width, and assumptions on the coupling-induced transition graph.
- Put the ETH/random-coupling and cascade assumptions near the headline "can work from zero overlap" claim, not only in the theorem section.

