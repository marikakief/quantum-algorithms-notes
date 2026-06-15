# Review: QIP = PSPACE (Jain-Ji-Upadhyay-Watrous 2011)

Source note: [[QIP = PSPACE (Jain-Ji-Upadhyay-Watrous 2011) — Paper Notes]]

## Verdict

Minor-to-major issues. The high-level story is correct and well written. The SDP and MMW details need auditing against the paper, and the note inherits some possible confusion from the Watrous constant-round note around `QIP(2)` versus `QIP(3)`.

## Line-Anchored Comments

- Lines 11-19: The overview is good. Keep "PSPACE algorithm for the SDP" as the core contribution; it prevents readers from thinking this is an efficient simulation.

- Lines 23-32: The single-coin QMAM reduction is correctly identified as the structural simplification. Add exact completeness/soundness parameters from Marriott-Watrous or state that they are amplified constants.

- Lines 34-52: The SDP formulation should be checked carefully against the paper. In particular, verify the placement of `Q^{-1/2}`, the definition of `Phi`, and the primal objective. A small typo in this SDP would change the theorem.

- Lines 54-70: The MMW iteration is useful but very detailed. Add the choice/meaning of `gamma`, `epsilon`, and `delta`; currently the update reads like code without parameter context.

- Lines 72-80: "Each iteration is in NC, and their composition over O(log N) steps remains in NC" should say NC(poly) or polynomial-space uniform NC as appropriate. Sequentially composing logarithmically many NC computations is fine for PSPACE, but the exact parallel-depth statement deserves care.

- Lines 86-96: The equality chain should avoid saying anything about two-message QIP unless separately verified. The safe statement is `QIP = QIP(3) = PSPACE` for polynomially bounded messages.

## Missing or Suggested Additions

- Add a one-paragraph derivation of why NC(poly) equals PSPACE.
- Add a compact "why MMW rather than ellipsoid/interior-point" note: parallelizable matrix exponentials and spectral projections.
