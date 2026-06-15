# Review: Faster Algorithmic Quantum and Classical Simulations by Corrected Product Formulas (Bagherimehrab-Berry-Schleich-Aldossary-Angulo-Aspuru-Guzik 2024)

Source note: [[Faster Algorithmic Quantum and Classical Simulations by Corrected Product Formulas (Bagherimehrab-Berry-Schleich-Aldossary-Angulo-Aspuru-Guzik 2024) — Paper Notes]]

## Primary Sources Checked

- Bagherimehrab et al., "Faster Algorithmic Quantum and Classical Simulations by Corrected Product Formulas," arXiv:2409.08265: https://arxiv.org/abs/2409.08265

## Verdict

Sound with minor caveats. The note captures the corrected-product-formula idea and the perturbed-system regime well.

## Line-Anchored Comments

- Lines 15-21: the main error-scaling claims are clearly stated. Keep the distinction between non-perturbed two-extra-orders and perturbed factor-`alpha` improvement.
- Lines 30-35: the symplectic-corrector cancellation across repeated steps is the key insight and is explained well.
- Lines 72-78: corrector compilation is not a detail; it should be kept close to the headline algorithm because the corrector is not directly available as a primitive in most gate models.
- Lines 107 and 125: THRIFT is cited as 2024 in the text but the vault note title is 2025/Nature Communications. Use one convention consistently: 2024 arXiv, 2025 journal.
- Line 131: examples like "Hubbard with weak hopping" should be checked against which partition is dominant/easy in the source's benchmark models. The broad "perturbed lattice" claim is safe; specific chemistry examples need care.

## Missing or Suggested Additions

- Add a diagram-level comparison between CPF and THRIFT: both achieve `alpha^2` in a perturbed setting, but CPF uses boundary commutator correctors while THRIFT uses an interaction-picture product formula.
