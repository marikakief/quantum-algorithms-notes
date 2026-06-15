# Review: Semiclassical QFT Register Recycling for Abelian Period Finding

Source note: [[Semiclassical QFT Register Recycling for Abelian Period Finding]]

## Primary Sources Checked

- Griffiths and Niu, "Semiclassical Fourier Transform for Quantum Computation", arXiv: https://arxiv.org/abs/quant-ph/9511007
- Proos and Zalka, "Shor's discrete logarithm quantum algorithm for elliptic curves", arXiv: https://arxiv.org/abs/quant-ph/0301141

## Verdict

Minor issues. The core idea is sound, but the source attribution should name Griffiths-Niu first, with Proos-Zalka as the ECDLP application.

## Line-Anchored Comments

- Line 3: source attribution

  Incomplete. The semiclassical QFT observation is Griffiths-Niu 1996. Proos-Zalka use it for elliptic-curve discrete logs and register elimination, but they are not the original source of the trick.

- Lines 8 and 12: "replaces a full QFT input register"

  Slightly imprecise. The replacement is valid when the Fourier/control register is prepared in product superposition, controls known unitaries, and would then be inverse-QFTed and immediately measured. The coherent register is replaced by sequential prepare-control-measure-feedforward steps.

- Lines 20-27: bit processing

  Good, but specify the bit order depends on the inverse-QFT convention. The phrase "from high to low significance" is not universal unless the surrounding QFT convention is fixed.

- Lines 35-37: saved qubits

  Correct for the intended ECDLP resource estimate. Gate count is "essentially unchanged" only at the algorithmic level; latency and adaptive single-qubit rotations/classical feed-forward become part of the physical schedule.

- Lines 39-41: caveat

  Correct and important. If later coherent computation needs the Fourier bits, measurement/recycling changes the algorithm.

## Missing or Suggested Additions

- Add a small reference to Beauregard-style factoring circuits too; this same semiclassical-QFT register recycling is also central to small-qubit Shor implementations.
