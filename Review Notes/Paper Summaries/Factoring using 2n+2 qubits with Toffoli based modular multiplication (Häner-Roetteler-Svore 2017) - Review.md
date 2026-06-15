# Review: Factoring using 2n+2 qubits with Toffoli based modular multiplication (Häner-Roetteler-Svore 2017)

Source note: [[Factoring using 2n+2 qubits with Toffoli based modular multiplication (Häner-Roetteler-Svore 2017) — Paper Notes]]

## Primary Sources Checked

- Häner, Roetteler, and Svore, "Factoring using 2n+2 qubits with Toffoli based modular multiplication", arXiv:1611.07995 / Quantum Information and Computation 17 (2017).
- Checked later Gidney-Ekerå comparison against arXiv:1905.09749 abstract formula.

## Verdict

Major issues. The paper itself is summarized well, but the later comparison to Gidney-Ekerå appears to understate their Toffoli count by roughly an order of magnitude.

## Line-Anchored Comments

- Lines 17-20: the headline contribution is accurately framed: same low qubit count as QFT-based low-qubit Shor implementations, but with Toffoli arithmetic instead of synthesized rotations.
- Lines 27-44: the dirty-ancilla constant-adder description is clear. It would help to distinguish "dirty target restored at the end" from ordinary scratch consumption.
- Lines 54-60: the modular multiplication and swap-and-inverse cleanup are well described.
- Lines 68-76: the asymptotic Toffoli count is useful. Keep emphasizing that this is an abstract Toffoli-network count, not a physical surface-code runtime.
- Lines 95-110: the comparison table is mostly good, but the QFT rows should make clear that the extra logarithm is from rotation synthesis precision assumptions.
- Line 119: likely numerical error. Gidney-Ekerå 2019 reports abstract Toffoli count `0.3 n^3 + 0.0005 n^3 lg n`, which is about `2.6e9` Toffolis at `n=2048`, not `3e8`. If a different later parameter set is intended, cite it explicitly.
- Line 123: good caveat that Toffoli-network testability does not extend to arbitrary circuits.

## Missing or Suggested Additions

- Add a small "later landscape" note separating Gidney-Ekerå 2019, newer lower-qubit factoring estimates, and this 2017 low-qubit Toffoli construction. They optimize different cost surfaces.

