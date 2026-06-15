# Review: Classical Simulation of Commuting Quantum Computations Implies Collapse of the Polynomial Hierarchy (Bremner-Jozsa-Shepherd 2011)

Source note: [[Classical Simulation of Commuting Quantum Computations Implies Collapse of the Polynomial Hierarchy (Bremner-Jozsa-Shepherd 2011) — Paper Notes]]

## Primary Sources Checked

- Bremner, Jozsa, and Shepherd, "Classical simulation of commuting quantum computations implies collapse of the polynomial hierarchy," arXiv:1005.1407; Proc. R. Soc. A 467, 459-472 (2011).

## Verdict

Minor issues. The note explains the postselection argument well. The main caveat is that this 2011 result is multiplicative-error sampling hardness; later additive-error/average-case results should not be retrofitted into the theorem statement.

## Line-Anchored Comments

- Lines 7-19: strong motivation. "They cannot even do basic arithmetic" is a useful intuition but should not be treated as a formal limitation of every IQP-related model.
- Lines 15-17: theorem statement is right: the collapse follows from efficient weak simulation to multiplicative error `c < sqrt(2)`.
- Lines 25-33: IQP circuit form is correct. The diagonal-basis convention should be kept explicit because different papers put the Hadamards at different places.
- Lines 39-49: the Hadamard gadget and post-IQP = PP proof are well summarized.
- Lines 52-64: classical-simulation-to-collapse argument is clear. If quoting the exact collapse level, use the paper's convention carefully: the point is collapse to the third level of PH via `postBPP`.
- Lines 70-75: the `O(log n)` output-line simulability result is useful and often forgotten.
- Lines 80-85: the "any class with post-C = PP" template is good, but examples such as constant-depth circuits should be tied to the precise gate sets/postselection constructions in the cited papers.
- Lines 104-110: caveats are exactly right. Multiplicative error is the unrealistic part that BMS 2016 fixes conditionally.

## Missing or Suggested Additions

- Add a short "2011 vs 2016" box: multiplicative-error hardness here; additive-error hardness later requires Stockmeyer plus average-case conjectures and anticoncentration.
- Add the phrase "weak sampling simulation" near the theorem, since this is not a strong simulation/probability-estimation result.

