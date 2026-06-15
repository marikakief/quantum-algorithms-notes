# Review: Iterative Phase Estimation (Kitaev)

Source note: [[Iterative Phase Estimation (Kitaev)]]

## Primary Sources Checked

- Kitaev, "Quantum measurements and the Abelian Stabilizer Problem": https://arxiv.org/abs/quant-ph/9511026
- O'Malley et al., "Scalable Quantum Simulation of Molecular Energies": https://arxiv.org/abs/1512.06860

## Verdict

Major issues. The card blends Kitaev's original eigenvalue-measurement procedure with semiclassical inverse-QFT/iterative phase-estimation circuits used in chemistry demos.

## Line-Anchored Comments

- Lines 7 and 13: "Kitaev's iterative version"

  Kitaev's original approach estimates eigenphases through repeated controlled-unitary experiments estimating sine/cosine components. The feed-forward bit-by-bit circuit with compensating rotations is closer to semiclassical/iterative QPE as used in later experiments. Distinguish these two.

- Lines 18-22: feed-forward formula and energy expression

  This appears chemistry-demo-specific, not a general Kitaev theorem. Define `t_0`, sign conventions, and bit ordering. As written it is too easy to apply with the wrong phase convention.

- Lines 24-26: majority vote with overlap `>0.5`

  This is specific to extracting the dominant eigenphase from a non-eigenstate input. If the input is an exact eigenstate, no overlap condition is needed. If the input is a superposition, QPE samples eigenvalues according to overlap; majority vote only works when one eigencomponent dominates.

- Lines 39-42: total circuit executions and confidence

  The `O(b/delta^2)` expression is not a standard high-confidence scaling as written. Usually shot count depends logarithmically on failure probability and quadratically on the bias gap. Rewrite with explicit per-bit error probability and confidence parameter.

- Line 54: "outperforms Kitaev by ~10^7x"

  This is a very specific comparative claim from Wiebe-Granade. It should be source-checked in that paper before being repeated here.

## Missing or Suggested Additions

- Split into two cards if possible: "Kitaev Eigenvalue Measurement" and "Semiclassical/Iterative Phase Estimation for Chemistry".
