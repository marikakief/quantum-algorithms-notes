# Review: Randomizing Multi-Product Formulas for Hamiltonian Simulation (Faehrmann-Steudtner-Kueng-Kieferová-Eisert 2022)

Source note: [[Randomizing Multi-Product Formulas for Hamiltonian Simulation (Faehrmann-Steudtner-Kueng-Kieferová-Eisert 2022) — Paper Notes]]

## Primary Sources Checked

- Faehrmann, Steudtner, Kueng, Kieferová, Eisert, "Randomizing multi-product formulas for Hamiltonian simulation," arXiv:2101.07808: https://arxiv.org/abs/2101.07808

## Verdict

Needs rewrite. The high-level motivation is right, but the observable-estimation protocol is described in a way that appears to drop the cross terms of the coherent multi-product formula.

## Line-Anchored Comments

- Line 1: the source title contains a wikilink inside the title. Use plain bibliographic text.
- Lines 9-10: if `U approx sum_q C_q V_q`, then an expectation value after `U` contains terms `V_q^\dagger O V_{q'}`. A single-index average over `V_q^\dagger O V_q` does not estimate the coherent MPF expectation in general.
- Lines 31-34: the Hadamard-test step is therefore incomplete or wrong as written. The randomized MPF observable estimator must account for signed cross terms, typically by sampling a pair of indices and estimating the corresponding interference term. Re-check the paper's algorithm and rewrite this section carefully.
- Lines 36 and 80-86: because the estimator is misstated, the claims of unbiasedness need re-derivation from the correct sampled quantity.
- Lines 68-74: the `Xi^4` intuition is directionally right: two coefficient samples/rescalings give a squared resolution-factor penalty in the estimator and fourth power in variance. But this should be connected to the correct two-index estimator.
- Lines 131-135: the caveats are good and should stay after the protocol is fixed.

## Missing or Suggested Additions

- Add a compact formula for the target observable expectation showing the double sum over MPF coefficients. This will prevent the most serious misunderstanding.
