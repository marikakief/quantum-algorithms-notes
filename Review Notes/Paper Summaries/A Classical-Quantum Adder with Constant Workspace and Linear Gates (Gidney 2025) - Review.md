# Review: A Classical-Quantum Adder with Constant Workspace and Linear Gates (Gidney 2025)

Source note: [[A Classical-Quantum Adder with Constant Workspace and Linear Gates (Gidney 2025) — Paper Notes]]

## Primary Sources Checked

- Gidney, "A Classical-Quantum Adder with Constant Workspace and Linear Gates", arXiv:2507.23079. Checked current arXiv metadata.

## Verdict

Minor issues. This is a strong note. The main fixes are to avoid overstating depth lower bounds and to be more precise about controlled variants.

## Line-Anchored Comments

- Lines 9-15: good problem statement. The CQ versus QQ distinction is exactly the right framing.
- Lines 19-22: the headline resource counts match the arXiv abstract: `4n +- O(1)` Toffolis with 3 clean ancillae, and `3n +- O(1)` with dirty workspace.
- Lines 29-35: the venting explanation is clear. Consider adding a tiny note that "Z-redundant" is a statement about a computational-basis function still recoverable from surviving registers.
- Lines 39-43: good explanation of carry-xor resolving phasing tasks. State explicitly that the dirty target register must be restored, not merely consumed.
- Lines 57-60: "zero extra Toffoli cost" for controlled variants should be tied to the classical-offset structure. Do not generalize it to arbitrary controlled adders.
- Line 86: "constant workspace rules out parallelism" is too strong as a theorem. Say this construction is linear depth and that known low-depth approaches spend workspace.
- Lines 90-92: excellent caveat about Toffoli versus T count. Keep it near the resource table.

## Missing or Suggested Additions

- Add a short comparison to the 2025 log-depth QFT paper: both are modern responses to the arithmetic depth/workspace tradeoff, but they optimize different metrics.

