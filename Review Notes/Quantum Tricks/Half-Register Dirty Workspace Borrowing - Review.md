# Review: Half-Register Dirty Workspace Borrowing

Source note: [[Half-Register Dirty Workspace Borrowing]]

## Primary Sources Checked

- Gidney, arXiv:2507.23079.

## Verdict

Minor issues. The card is accurate and concise.

## Line-Anchored Comments

- Lines 12-18: the scheduling constraint is explained well.
- Lines 28-30: the resource tradeoff matches the source note: dirty-workspace variant to self-supplied-workspace variant costs an extra pass and one clean ancilla.
- Lines 32-36: good caveat. Keep the warning about circular dependencies when reusing the pattern elsewhere.

## Missing or Suggested Additions

- Add a small timeline diagram showing when each half is data, dirty workspace, and finished data. This trick is schedule-sensitive.

