# Review: Addition on a Quantum Computer (Draper 2000)

Source note: [[Addition on a Quantum Computer (Draper 2000) — Paper Notes]]

## Primary Sources Checked

- Draper, "Addition on a Quantum Computer", arXiv:quant-ph/0008033 (2000).

## Verdict

Minor issues. The core Fourier-basis adder explanation is good. The note needs a few orientation and historical-qualification fixes.

## Line-Anchored Comments

- Lines 9 and 61-64: register orientation is inconsistent. The opening says the output is `|a>|a+b>`, but the full procedure adds `b` into the first register and returns `|a+b>|b>`. Either variant is possible, but the note should pick one convention and stick to it.
- Line 11: "classical reversible adders require `3n` qubits" should be historical: prior adders at the time used that many carry/work qubits. Later Cuccaro-style ripple adders changed the comparison.
- Lines 23-35: the QFT product-state description is clear and useful.
- Lines 65-71: the gate-count discussion is fine for abstract controlled rotations. Add a fault-tolerant caveat before quoting the approximate improvement, because rotation synthesis changes the practical cost model.
- Lines 77-80: the parallelism statement applies to the addition rotations. The full QFT-adder depth also includes the forward and inverse QFT circuits, whose parallel depth depends on the QFT implementation and connectivity.
- Lines 95-96: the Shor qubit-count comparison should be labeled as the historical Zalka/Draper setting; later Beauregard and Toffoli-based constructions give different qubit/gate tradeoffs.

## Missing or Suggested Additions

- Add "abstract rotations" versus "Clifford+T/T-count" as a resource distinction. This is the main reason QFT adders are no longer automatically preferable in fault-tolerant arithmetic.

