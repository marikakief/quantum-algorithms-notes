# Review: Fixed-Base Order Finding for Reusable Factoring Circuits

Source note: [[Fixed-Base Order Finding for Reusable Factoring Circuits]]

## Primary Sources Checked

- Grosshans, Lawson, Morain, and Smith, "Factoring Safe Semiprimes with a Single Quantum Query", arXiv: https://arxiv.org/abs/1511.04385

## Verdict

Minor issues. The card captures the safe-semiprime idea, but it should not sound like a generic Shor optimization.

## Line-Anchored Comments

- Lines 8 and 20-31: reusable fixed base

  Correct under the safe-semiprime promise. The source emphasizes that the advantage is practical as well as probabilistic: one can rerun the same QOFA circuit if the rare classical post-processing failure occurs.

- Lines 20-29: base `a = 2`

  Add the promise and exclusions explicitly: this is for safe semiprimes in the paper's setting, and the base choice must be independent of the hidden factors. It is not a general recommendation to use base 2 for arbitrary RSA moduli.

- Lines 28-31: order constraints

  Plausible, but should cite the specific number-theory lemma. Readers need to know this is a theorem from the safe-prime structure, not a generic order fact.

- Lines 41-45: expected runs

  Good in spirit, but the exact failure probability should be tied to the paper's theorem. The abstract says "almost always" with one QOFA call; exact constants depend on the safe-semiprime parameters.

- Lines 47-49: caveat

  Excellent. This is the most important warning: a base chosen using factor knowledge produces misleading compiled-Shor demonstrations.

## Missing or Suggested Additions

- Add a comparison sentence: for general composites, Shor's random-base order finding remains the standard algorithm; this fixed-base trick is promise-specific.
