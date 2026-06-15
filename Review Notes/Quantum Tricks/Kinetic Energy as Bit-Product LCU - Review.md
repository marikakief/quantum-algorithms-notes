# Review: Kinetic Energy as Bit-Product LCU

Source note: [[Kinetic Energy as Bit-Product LCU]]

## Primary Sources Checked

- Su et al., arXiv:2105.12767, kinetic-energy LCU construction.

## Verdict

Minor issues. The bit-product trick is accurately described. The main needed edit is to make clear which costs are for the kinetic SELECT itself versus electron-register selection and swaps in the full block encoding.

## Line-Anchored Comments

- Lines 7 and 15-19: the decomposition of the squared momentum into bit products is the right core idea.
- Line 19: the PREPARE over `r,s` using cascading controlled Hadamards is source-specific and should identify the signed-momentum bit convention.
- Line 21: "SEL cost for `T` is only `5(n_p-1)+2`" is a local cost for the kinetic primitive. The full qubitization step also includes choosing/controlling the electron register and other block-encoding logic.
- Lines 31-33: the naive comparison `O(eta n_p^2)` is useful, but in the source algorithm the real savings depend on how the electron index is prepared and how often the kinetic branch is selected.
- Line 37: the non-cubic caveat is correct for the clean cubic bit-product form. The 2024 pseudopotential paper later handles general Gramians by arithmetic rather than this trick.
- Line 39: good distinction from the interaction-picture kinetic phasing problem.

## Missing or Suggested Additions

- Add a line saying this trick is for the qubitized LCU block encoding of `T`; it is not a generic way to apply `exp(-iTt)` as a phase.

