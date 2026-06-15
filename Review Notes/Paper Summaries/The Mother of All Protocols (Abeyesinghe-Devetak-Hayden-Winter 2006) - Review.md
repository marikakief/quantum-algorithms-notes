# Review: The Mother of All Protocols (Abeyesinghe-Devetak-Hayden-Winter 2006)

Source note: [[The Mother of All Protocols (Abeyesinghe-Devetak-Hayden-Winter 2006) — Paper Notes]]

## Verdict

Minor issues. The note accurately presents FQSW as the decoupling-based unification of several quantum Shannon protocols. The main improvement is to temper universal "all protocols" language and clarify how state merging is obtained from the fully quantum resource inequality.

## Line-Anchored Comments

- Lines 11-24: The resource inequality is the right centerpiece. Add a short explanation of each resource symbol for readers who do not already know the Devetak-Harrow-Winter calculus.

- Lines 26-37: The protocol steps are clear. Note that `A_1` is the part sent and `A_2` is the part left behind to become entanglement; this makes the `1/2 I(A;R)` and `1/2 I(A;B)` rates intuitive.

- Lines 39-52: The one-shot theorem and decoupling theorem are useful. Label the displayed constants as one-shot/finite-dimensional bounds before moving to the asymptotic FQSW inequality.

- Lines 54-69: "Every known single-sender/single-receiver quantum Shannon protocol" is too sweeping. Safer: the paper derives the standard one-way single-sender/single-receiver family known at the time via resource inequalities.

- Lines 71-79: The comparison to state merging is good. Instead of "state merging + entanglement distillation simultaneously," state that FQSW coherently transfers Alice's share while distilling entanglement; classical state merging follows by consuming or converting resources.

- Lines 81-86: The efficient-encoding caveat is right. If retaining the Clifford statement, specify that approximate randomizing/decoupling ensembles can replace Haar-random unitaries in the coding proof; this is not a practical low-depth circuit construction.

## Missing or Suggested Additions

- Add a mini glossary for `[q -> q]`, `[qq]`, coherent communication, and resource inequalities.
- Add a diagram or one-line chain showing mother -> state merging by teleportation/resource conversion.
