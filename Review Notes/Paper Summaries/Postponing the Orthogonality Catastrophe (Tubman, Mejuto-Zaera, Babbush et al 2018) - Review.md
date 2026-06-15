# Review: Postponing the Orthogonality Catastrophe (Tubman, Mejuto-Zaera, Babbush et al 2018)

Source note: [[Postponing the Orthogonality Catastrophe (Tubman, Mejuto-Zaera, Babbush et al 2018) — Paper Notes]]

## Primary Sources Checked

- Tubman et al., "Postponing the orthogonality catastrophe: efficient state preparation for electronic structure simulations on quantum devices", arXiv:1809.05523 (2018).

## Verdict

Minor issues. The note is historically useful and mostly accurate. It should more carefully distinguish what the 2018 evidence supports from what later Fe-S/FeMoCo studies complicated.

## Line-Anchored Comments

- Lines 21-23: the orthogonality-catastrophe framing is clear and accurate.
- Line 25: "justifies assumptions earlier FeMoco took on faith" is too sweeping. It provides evidence against catastrophic overlap loss in the studied models, but later Fe-S and FeMoCo state-preparation work found a more nuanced picture.
- Lines 49-52: the Method A cost discussion should separate the number of selected configurations from the cost of implementing the isometry. "O(L)" can be true for the list length while hiding nontrivial synthesis overhead.
- Lines 83-86: the reported FeMoCo overlap values are useful, but they should not be treated as settled for all active spaces, spin sectors, or later candidate ground states.
- Line 125: the lack of T-gate/fault-tolerant compilation analysis is an important caveat and should appear earlier in the resource-estimate discussion.

## Missing or Suggested Additions

- Add a historical note: this paper weakens a simple orthogonality-catastrophe objection, but it does not by itself solve modern fault-tolerant initial-state preparation for strongly correlated transition-metal clusters.

