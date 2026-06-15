# Review: MAJ-UMA Decomposition for Reversible Adders

Source note: [[MAJ-UMA Decomposition for Reversible Adders]]

## Primary Sources Checked

- Cuccaro, Draper, Kutin, and Moulton, arXiv:quant-ph/0410184.

## Verdict

Minor issues. The paired compute/uncompute structure is well explained. The exact gate counts should be sourced to a named variant.

## Line-Anchored Comments

- Lines 12-20: the MAJ/UMA sweep explanation is clear and captures why temporary logical-ANDs later apply.
- Lines 14-16: the mapping notation depends on wire order. Add the source's wire ordering or a circuit diagram reference so readers do not copy the tuple transform incorrectly.
- Line 30: the exact CNOT/NOT/depth counts should say which optimized Cuccaro variant they describe.
- Lines 32-34: the caveat is good: this is a linear-depth ripple technique, not a carry-lookahead trick.

## Missing or Suggested Additions

- Add the distinction between in-place addition with final carry and addition modulo `2^n`; MAJ/UMA variants differ slightly at the boundaries.

