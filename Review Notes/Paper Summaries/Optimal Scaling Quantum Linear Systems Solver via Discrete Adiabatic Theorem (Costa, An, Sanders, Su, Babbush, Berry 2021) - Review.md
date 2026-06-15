# Review: Optimal Scaling Quantum Linear Systems Solver via Discrete Adiabatic Theorem (Costa-An-Sanders-Su-Babbush-Berry 2021)

Source note: [[Optimal Scaling Quantum Linear Systems Solver via Discrete Adiabatic Theorem (Costa, An, Sanders, Su, Babbush, Berry 2021) — Paper Notes]]

## Primary Sources Checked

- Costa, An, Sanders, Su, Babbush, and Berry, "Optimal scaling quantum linear systems solver via discrete adiabatic theorem", PRX Quantum 2022 / arXiv:2111.08152.

## Verdict

Minor issues. The technical arc is well summarized. The main problems are style and over-specific practical claims: the "Yuval" aside is too informal for a paper note, and the constant-factor/crossover comments should be tied to the follow-up paper or softened.

## Line-Anchored Comments

- Lines 9-17: good QLSP setup and headline optimal `O(kappa log(1/epsilon))` statement.
- Line 19: remove or rephrase "Yuval is a co-author." Use the surname and explain the technical connection to the Sanders/Berry/Babbush adiabatic-walk line if it matters.
- Lines 31-45: the Hamiltonian/eigenpath setup and schedule are well presented.
- Lines 49-63: good discrete adiabatic theorem summary. The explicit bound is dense but valuable.
- Lines 65-79: excellent explanation of Dolph-Chebyshev filtering and the two-ancilla implementation.
- Lines 83-95: theorem summary is strong. Make clear that the `O(kappa log(1/epsilon))` count is in block-encoding/oracle calls; sparse-entry access incurs block-encoding overhead.
- Line 113: the claim that filtering dominates only below `epsilon < 10^{-180}` should be attributed to the numerical follow-up or phrased as an example from the authors' cost model, not a general fact.
- Lines 121-129: good caveats, especially the sparse-access versus block-encoding distinction.

## Missing or Suggested Additions

- Add a one-sentence statement of what is being lower-bounded: normalized solution-state preparation with block-encoding access. This prevents readers from applying the bound to full classical readout.

