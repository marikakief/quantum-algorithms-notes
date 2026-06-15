# Review: Efficient Classical Simulation of Slightly Entangled Quantum Computations (Vidal 2003)

Source note: [[Efficient Classical Simulation of Slightly Entangled Quantum Computations (Vidal 2003) — Paper Notes]]

## Primary Sources Checked

- Vidal, "Efficient classical simulation of slightly entangled quantum computations," Phys. Rev. Lett. 91, 147902 (2003); arXiv:quant-ph/0301063.

## Verdict

Minor-to-major issues. The note is good on the MPS update mechanics, but the necessary-condition wording is off: exponential quantum speedup requires superpolynomial Schmidt-rank growth in the relevant ordering, not merely "at least polynomial" entanglement growth.

## Line-Anchored Comments

- Lines 9-17: good setup. Replace "must grow at least polynomially" with "must grow superpolynomially" for exponential speedup, since polynomial `chi` is exactly the efficiently simulable regime.
- Lines 17-19: good historical context. This is indeed one of the roots of MPS/tensor-network simulation in quantum information.
- Lines 27-37: the definition of `chi` needs alignment with the algorithm. The MPS representation tracks Schmidt ranks across cuts of a chosen linear ordering; if one takes a maximum over all bipartitions, that is stronger than needed and removes some ordering subtleties.
- Lines 41-47: canonical MPS form is well explained.
- Lines 51-68: gate-update costs are clear. The non-adjacent gate overhead should be included in the theorem-level runtime if arbitrary circuit connectivity is allowed.
- Lines 74-82: theorem summary is mostly correct, but again "subexponential `chi` implies no exponential speedup" should be stated as no exponential separation in runtime from this simulation, not no possible quantum advantage of any kind.
- Lines 101-103: the ordering caveat conflicts somewhat with line 35's "all bipartitions" definition. Fix by saying the efficient simulation depends on low Schmidt rank across the cuts induced by an efficient tensor-network layout.
- Lines 103-105: excellent point about exact Schmidt rank being discontinuous and truncated Schmidt decompositions being the practical version.
- Lines 105-107: mixed-state caveat is important. DQC1 is the standard warning against treating entanglement alone as a complete speedup criterion.

## Missing or Suggested Additions

- Add a precise statement of the tensor-network graph/order used by the simulation.
- Add a short "exact rank vs effective rank" note connecting Vidal to later TEBD/DMRG practice.

