# Review: Entanglement in a Simple Quantum Phase Transition (Osborne-Nielsen 2002)

Source note: [[Entanglement in a Simple Quantum Phase Transition (Osborne-Nielsen 2002) — Paper Notes]]

## Verdict

Minor issues. The note is a strong condensed-matter-facing summary and correctly distinguishes single-site entropy, concurrence, and thermal entanglement. The main revisions are about symmetry-breaking conventions and avoiding the phrase "maximally entangled" without the relevant measure.

## Line-Anchored Comments

- Lines 15-23: The historical framing is good. Add that the paper is not an algorithms paper; it is included because it shaped the entanglement/resource language used later.

- Lines 25-49: The Hamiltonian and Jordan-Wigner solution are clear. State the boundary-condition/thermodynamic-limit convention, since finite-size parity sectors are a common source of sign differences in XY-chain formulas.

- Lines 55-65: The one-site reduced density matrix uses `⟨sigma^x⟩`. In a finite parity-symmetric ground state this expectation vanishes; the nonzero value corresponds to the symmetry-broken thermodynamic-limit branch. Add that convention.

- Lines 63-65: "A single site is maximally entangled with the rest" is too strong if read literally. Say the single-site entropy is maximized within the studied Ising-family curve, not that it reaches the maximum possible one-qubit entropy in all conventions.

- Lines 67-80: The concurrence details are good. Add that zero concurrence for `r >= 3` does not imply absence of multipartite entanglement or long-range quantum correlations.

- Lines 82-90: The monogamy explanation is a useful physical picture. Keep it framed as interpretation, not a theorem deriving the concurrence peak shift.

- Lines 108-115: The later Calabrese-Cardy comparison is excellent. It clarifies why block entropy, not single-site entropy, became the universal critical diagnostic.

## Missing or Suggested Additions

- Add a one-line comparison with Osterloh et al. on concurrence-derivative scaling.
- Define the convention for `lambda` as inverse field strength near the model statement.
