# Review: Shadow Kernel for Nonlinear Phase Classification

Source note: [[Shadow Kernel for Nonlinear Phase Classification]]

## Primary Sources Checked

- Huang, Kueng, Torlai, Albert, and Preskill, arXiv:2106.12627 / Science 377, eabk3333 (2022), for shadow-based many-body learning context.

## Verdict

Minor issues. The card is well scoped and its main caveat is appropriate.

## Line-Anchored Comments

- Lines 12-29: the nonlinear local-marginal feature map is explained clearly.
- Lines 37-43: the complexity and phase-boundary caveat are good. Add sample complexity/shadow-shot dependence if this card is meant to guide algorithm selection.
- Line 43: the correlation-length warning is important. Move it into "When to reach for it" as a negative condition.

## Missing or Suggested Additions

- Add a sentence distinguishing this kernel from the projected quantum kernel used for supervised QML: both use shadows/local features, but the physical assumptions and tasks differ.

