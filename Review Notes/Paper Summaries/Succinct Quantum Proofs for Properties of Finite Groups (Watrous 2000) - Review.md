# Review: Succinct Quantum Proofs for Properties of Finite Groups (Watrous 2000)

Source note: [[Succinct Quantum Proofs for Properties of Finite Groups (Watrous 2000) — Paper Notes]]

## Verdict

Minor issues. The note gives a clear account of group non-membership in QMA and the subgroup-state certificate. The main improvements are to keep black-box group assumptions explicit and to avoid overstating relativized separations.

## Line-Anchored Comments

- Lines 7-15: The black-box group model is central. Add input-size conventions: unique encodings, oracle gates, and the role of the group oracle in the relativized statements.

- Lines 17-25: "First natural problem shown to separate QMA from MA in a relativised setting" is fair, but emphasize "relative to a group oracle." It is evidence, not an unconditional separation.

- Lines 31-42: The invariance test is well explained. Add that Babai's generator is only nearly uniform, so completeness/soundness carry small statistical errors.

- Lines 44-58: The controlled-multiplication/coset test is excellent. State explicitly that `|H>` and `|Hh>` are identical if `h in H` and orthogonal disjoint coset states otherwise.

- Lines 60-69: The completeness line says YES acceptance is `1/2`; clarify whether the table is before amplification and how many copies are needed for standard QMA completeness.

- Lines 100-109: The extensions to other group properties are terse. Mark them as sketches unless the note expands the precise certificates.

## Missing or Suggested Additions

- Add a short comparison between this QMA certificate and the later BQP algorithms for solvable groups.
- Add a note on why a classical string cannot succinctly certify subgroup non-membership in the oracle setting.
