# Review: Adiabatic State Preparation for Chemistry

Source note: [[Adiabatic State Preparation for Chemistry]]

## Primary Sources Checked

- Aspuru-Guzik et al., *Simulated Quantum Computation of Molecular Energies*, arXiv:quant-ph/0604193: https://arxiv.org/abs/quant-ph/0604193
- Farhi et al., *Quantum Computation by Adiabatic Evolution*, arXiv:quant-ph/0001106: https://arxiv.org/abs/quant-ph/0001106

## Verdict

Major issues. The basic interpolation idea is correct, but the runtime/step-size condition is dimensionally and conceptually wrong. Adiabatic runtime depends on the minimum spectral gap and on derivatives/norms of the Hamiltonian path, not simply on `Delta s << gap^2`.

## Line-Anchored Comments

- Lines 10-15: The interpolation from an easy Hartree-Fock-like Hamiltonian to the full CI/electronic Hamiltonian is the right picture. The initial Hamiltonian should be described as chosen to have the HF determinant as an easy ground state; "only the HF energy as its non-trivial element" is too schematic.

- Lines 16-18: The evolution variable is physical time, not `s` itself. A discretized implementation simulates `H(s(t))` for time intervals `Delta t`, with a schedule `s(t)`.

- Line 20: `Delta s << (Delta E)^2` is not a correct adiabatic theorem statement. A safer phrasing is that the total runtime scales with path-derivative norms and inverse powers of the minimum gap, often heuristically like `||dH/ds|| / gap_min^2`.

- Lines 23-25: Good use case list, but "when HF has poor overlap" is only partly right. ASP can help when there is a gapped path from HF to the target; if the gap closes, poor overlap is not solved.

- Lines 28-30: `T = O(poly(1/Delta E_min))` is too vague but acceptable if labeled heuristic. Avoid identifying `T` with the number of Trotter steps unless the timestep and simulation error budget are specified.

- Lines 32-35: The gap caveat is good and should be moved earlier because it is the central limitation.

## Missing or Suggested Additions

- Add the three separate errors: adiabatic error, Hamiltonian-simulation/Trotter error, and final phase-estimation projection error.
- Include "schedule choice" as part of the algorithm; local adiabatic schedules can matter.
