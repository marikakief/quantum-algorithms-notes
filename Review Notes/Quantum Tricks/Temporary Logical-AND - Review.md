# Review: Temporary Logical-AND

Source note: [[Temporary Logical-AND]]

## Primary Sources Checked

- Gidney, "Halving the cost of quantum addition", arXiv: https://arxiv.org/abs/1709.06648
- ar5iv rendering: https://ar5iv.labs.arxiv.org/html/1709.06648
- Jones, "Low-overhead constructions for the fault-tolerant Toffoli gate", DOI: https://doi.org/10.1103/PhysRevA.87.022328

## Verdict

Minor issues. The core primitive is described correctly. The use-case bullets need slightly tighter conditions.

## Line-Anchored Comments

- Lines 8 and 12-18: compute/erase cost

  Correct. Gidney's Fig. 3 gives a T-count 4 computation and a T-count 0 measurement-based uncomputation.

- Line 14: circuit description

  The text is acceptable as a conceptual description, but if this card is used as an implementation reference it should point to the exact figure. Small gate-order mistakes in this primitive change phases.

- Lines 20-22: correctness condition

  Good and important. Consider moving this above the "When to reach for it" bullets, because it is the difference between a theorem and an unsafe replacement rule.

- Lines 26-28: "any circuit"

  Too broad. The right phrase is "any circuit exposing suitable compute/uncompute Toffoli pairs satisfying the phase-insensitivity condition." Classical reversible Grover predicates often have this form; arbitrary hand-optimized oracles may not.

- Lines 39-43: opportunity cost

  Good. The note correctly warns that held ancillae consume physical area that might otherwise host T factories.

## Missing or Suggested Additions

- Add a one-line distinction between a Toffoli gate, a temporary AND value, and a persistent cleanly uncomputed AND value. They have different phase/correctness obligations.
