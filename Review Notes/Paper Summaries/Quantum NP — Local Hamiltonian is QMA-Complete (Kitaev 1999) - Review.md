# Review: Quantum NP - Local Hamiltonian is QMA-Complete (Kitaev 1999)

Source note: [[Quantum NP — Local Hamiltonian is QMA-Complete (Kitaev 1999) — Paper Notes]]

## Primary Sources Checked

- Kitaev, Shen, and Vyalyi, "Classical and Quantum Computation," AMS Graduate Studies in Mathematics 47 (2002), containing Kitaev's local-Hamiltonian QMA-completeness result.

## Verdict

Minor omissions. The note is concise and mostly accurate, but it should say a little more about QMA membership and about which spectral-gap statement is being used.

## Line-Anchored Comments

- Lines 7-12: good high-level framing. "Hard even for quantum computers" should be interpreted as "unless QMA = BQP"; a QMA verifier with a witness is stronger than an ordinary quantum algorithm.
- Lines 16-26: the local-Hamiltonian problem statement is clear. Add that the verifier for membership in QMA estimates the energy of a purported low-energy witness by measuring randomly chosen local terms.
- Lines 30-47: the history-state construction is right. It would help to mention that the clock construction is unary precisely to keep the propagation/checking terms local.
- Lines 49-51: the `Omega(1/L^2)` gap statement should be scoped to the propagation/path-graph analysis. The full promise gap after adding input/output/clock terms and rescaling is inverse polynomial.
- Lines 55-59: the geometric lemma is important, but the displayed form is a simplified memory aid. If this note is used as a reference, add the exact hypotheses about nullspaces/angles and positive semidefinite terms.
- Lines 63-68: the reusable ideas are exactly the right ones.

## Missing or Suggested Additions

- Add a short "membership in QMA" paragraph. The note currently focuses on hardness, but QMA-completeness also requires explaining why the local-Hamiltonian promise problem is in QMA.

