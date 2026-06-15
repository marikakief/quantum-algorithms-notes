# Review: Split-Operator Real-Space Simulation on a Quantum Computer

Source note: [[Split-Operator Real-Space Simulation on a Quantum Computer]]

## Primary Sources Checked

- Kassal et al., *Polynomial-time quantum algorithm for the simulation of chemical dynamics*, arXiv:0801.2986: https://arxiv.org/abs/0801.2986
- Childs, Leng, Li, Liu, and Zhang, *Quantum simulation of real-space dynamics*, arXiv:2203.17006: https://arxiv.org/abs/2203.17006

## Verdict

Minor issues. The card is concise and mostly correct. It should state QFT convention/order and scratch uncomputation assumptions more carefully.

## Line-Anchored Comments

- Lines 10-18: The split-operator idea is right. The displayed order is convention-dependent; if the state starts in position basis, one often applies the position potential, changes to momentum basis, applies kinetic phase, and changes back.

- Line 21: Fourier-eigenstate kickback can avoid uncomputing the value register when the oracle is pure modular addition into a Fourier eigenstate. Any scratch used to compute `T` or `V` still needs to be cleanly uncomputed or avoided.

- Lines 29-31: The complexity line correctly uses `O(B^2 m^3)` for Coulomb pair arithmetic, which is better than the inconsistent `m^2` headline in the Kassal paper note.

- Line 31: Per-step first-order error should be stated as local error scaling; total simulation error over many steps accumulates and depends on commutators and total time.

- Line 34: The caveat about magnetic/velocity-dependent potentials is good. Also mention singular Coulomb potentials and grid resolution as major real-space complications.

## Missing or Suggested Additions

- Add "position basis -> potential phase -> QFT -> kinetic phase -> inverse QFT" as the default convention.
- Add the error-budget components: discretization, arithmetic phase precision, product-formula error, and sampling.
