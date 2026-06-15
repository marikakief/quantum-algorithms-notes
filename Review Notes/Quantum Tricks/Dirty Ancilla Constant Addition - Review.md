# Review: Dirty Ancilla Constant Addition

Source note: [[Dirty Ancilla Constant Addition]]

## Primary Sources Checked

- Haner, Roetteler, and Svore, "Factoring using 2n+2 qubits with Toffoli based modular multiplication", arXiv: https://arxiv.org/abs/1611.07995
- ar5iv rendering: https://ar5iv.labs.arxiv.org/html/1611.07995

## Verdict

Minor issues. The card captures the construction's purpose and asymptotics. It should distinguish the paper's dirty-only tradeoff from later constant-workspace clean-ancilla additions.

## Line-Anchored Comments

- Lines 8 and 12-22: construction outline

  Good. The divide-and-conquer carry computation and dirty incrementer are the core of the HRS constant adder.

- Line 16: carry-gate Toffoli count

  Plausible source match, but this is a detail worth tying to the exact CARRY figure/subsection. It is easy to be off by boundary constants in this construction.

- Lines 28-30: Toffoli count and depth

  Correct asymptotically: `O(n log n)` size and `O(n)` depth. If using the displayed `8n(log_2 n - 2) + O(1)` expression, state the power-of-two/boundary convention.

- Lines 42-43: controlled version

  This needs more precision. HRS observe that in the controlled modular-multiplier context only certain final CNOTs in the CARRY circuit become multi-controlled; the full controlled adder/modular-addition cost still includes comparator and control overhead.

- Lines 47-51: caveat and supersession

  Good. The 2025 Gidney note may supersede this for clean-constant-workspace CQ addition, but HRS remains relevant when the workspace is strictly dirty/borrowed and qubit count is the controlling constraint.

## Missing or Suggested Additions

- Add definitions of "dirty" and "clean" ancilla in the first paragraph. The trick is easy to misread as using unknown qubits as free scratch; the essential obligation is returning them exactly to their original states.
