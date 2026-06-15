# Review: Quantum Computing with Realistically Noisy Devices (Knill 2005)

Source note: [[Quantum Computing with Realistically Noisy Devices (Knill 2005) — Paper Notes]]

## Verdict

Minor issues. The note correctly captures the postselected `C4/C6` architecture and its historical importance. The main correction is to keep "evidence/numerical threshold estimate" attached to all high-threshold claims.

## Line-Anchored Comments

- Lines 15-27: "Possible above 3%" should be immediately qualified as evidence under a specific stochastic depolarizing model with large overhead, not a rigorous universal threshold theorem.

- Lines 31-52: The code descriptions are clear. Add distance/error-detection capability for `C4` and `C6`, since "error-detecting rather than correcting" is the architecture's key idea.

- Lines 54-62: Good explanation of error-correcting teleportation. Mention that the Bell-pair preparation/verification cost is where much of the overhead moves.

- Lines 64-82: The postselection bridge is well described. Keep "restart when detected" separate from scalable fault tolerance; postselected computation itself is not the final computing model.

- Lines 94-104: The resource numbers are useful but should be presented as architecture/model-specific estimates. They should not be compared to later surface-code thresholds without caveats.

- Lines 124-132: The caveats are strong and should remain prominent. The idealized noise model and all-to-all assumptions are not secondary details.

## Missing or Suggested Additions

- Add a small table separating postselected threshold, standard `C4/C6` threshold, and the speculative/high-capacity-code bridge.
- Add a sentence linking the `|\pi/8>` state to the later magic-state-distillation framing.
